---
{"dg-publish":true,"permalink":"/04-project/winter-framework/006-jdbc/","tags":["personal/blog"]}
---


# 基本概述
此模块针对数据库持久层的处理进行简化，主要在两个方面，一是对数据库操作 api 的简化，而是对事务的简化。仿照 spring 框架，我们将提供一个 JdbcTemplate 和声明式事务的实现。

# JdbcTemplate
## 配置 DataSource
首先要做的是，数据库连接池的配置。由于我们之前已经实现了 spring-ioc 容器，所以，我们可以直接通过依赖注入的方式动态获取用户的配置。

我们设计一个 `JdbcConfiguration` 类，该类用注入我们的 DataSource。我们仿照 spring-boot 默认注入 Hikari DataSource。

定义如下几个属性：
```properties
winter.datasource.url
winter.datasource.username
winter.datasource.password
winter.datasource.driver-class-name
winter.datasource.maximum-pool-size
winter.datasource.minimum-pool-size
winter.datasource.connection-timeout
```

然后通过， `@Value("${winter.datasource.url:default-v}")` 的形式注入。

## 定义 JdbcTemplate
我们仿照 Spring 中的 JdbcTemplate 的结构，利用 Lamada 表达式重构模板模式。

我们以方法 `public <T> T queryForObject(String sql, Class<T> clazz, Object... args)` 来说明整个 JdbcTemplate 的运行逻辑。

![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/20231218182422.png)


### Mapper
该方法通过接受不同的 Class 类型，调用不同的 Mapper 来达到执行 sql 和对 sql 运行返回值转换的功能。所以，各种类型的 Mapper 的作用，就是对返回的结果进行映射，BeanMapper 就是将返回的一个单位的数据（通常指一行）转换成对应的 Java Bean；而 StringMapper，就是将返回结果转化为 String 类型。

### execute
然后该方法通过调用方法 `public <T> T queryForObject(String sql, RowMapper<T> rowMapper, Object... args)` 去准备真正执行 sql，并返回结果。

该方法的设计目标是可以**执行所有的 sql 语句，并给用户一定粒度的控制**。

我们定义了两个 `execute` 方法：
 + `public <T> T execute(PreparedStatementCreator psc, PreparedStatementCallback<T> action)`。我们暂且称其为“method 1”；
 + `public <T> T execute(ConnectionCallback<T> action)`。我们暂且称其为“method 2”；

这两个方法是核心方法，是支撑整个 JdbcTemplate 得以正确运行的关键。

两个函数式接口实现了设计模式中的模板方法模式。在通俗的定义中，我们需要定义抽象类，该抽象类中需要声明一个抽象方法，和一个 final 修饰的方法。我们可以通过子类实现这些抽象方法来对 final 方法进行“补全”，抽象方法担任了黑盒的角色。

所以，本质是“补全”，而不是类的结构。而 Java 的函数式编程就给“补全”以极大的方便。

通过实现 method 1 中的 PreparedStatementCallback 接口，我们就相当于实现了一个黑盒（模板模式中的抽象方法）。通过实现该接口，我们可以自定义 sql 的执行逻辑（发送到 db 进程之前的部分逻辑），也自定义了数据的返回结构。比如说：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/20231218183627.png)
	在上述方法中，我们将结果集 rs 的数据封装到了 list 之中。

也就是说，我们可以追踪到 sql 语句执行的过程，并做相关操作。

***
接下来看，method 2，该方法通过实现 `ConnectionCallback` 接口，得以让我们实现通过给定的 connection，执行任意数量的操作。

### 总结-聚焦资源使用的思想
假如，按照之前的方式实现执行任意 sql 语句的过程是什么？
 1. 获取 DataSource 的连接 connection；
 2. 对 connection 进行一些设置（例如 auto commit）；
 3. 通过 connection 获取 statement 对象；
 4. 通过 statement 对象执行 sql 语句；
	 1. 判断语句类型，是 DDL 还是查询语句？
	 2. 根据语句的类型执行不同的方法；
 5. 对结果集（通常是 ResultSet）进行处理；

故而，上述所有的设计都是对该过程的简化。我们通过 `PreparedstatementCallback` 自定义了步骤 3 和 4 。通过 `ConnectionCallback` 定义了步骤 2， 3， 4， 5 ， method 2 中我们定义了步骤 1 。

可以看到，这种设计的核心思想是**资源的利用方式**，通过 method 2 以及用户定义的 callback 函数，我们定义了 connection 的利用方式；通过 method 1 以及相关 callback，我们定义了 preparedStatement 的使用方式。

通过对两种资源（Connection、PreparedStatement）使用方式的定义，我们完成了模板方法模式的设计。


# 声明式事务
声明式事务的支持需要首先实现 aop 机制。为了简化实现，我们定义注解 `@Transactional` 仅仅只能在类上修饰。

让该注解实现 aop 的效果，我们只需要继承类 `AnnotationProxyBeanPostProcessor<A extends Annotation>` 即可。

重点在于如何实现事务的提交回滚。我们知道 Spring 的 Jdbc 支持多种传播模式，为了简便起见，我们实现应用最广的 `REQUIRED`，即只要事务中出现一个错误，便执行全局回滚。

## 事务的提交回滚

### 获取 Connection
事务是建立在连接上的，所以为了实现事务，我们首先要得到连接。

我们首先创建了一个事务管理类（也是一个 Handler，实现事务的代理）`DataSourceTransactionManager`。由于，我们使用的数据库连接池，所以，我们需要将 DataSource 依赖注入到这个类中。

即在 JdbcConfiguration 中注册该组件：
```java
    @Bean
    PlatformTransactionManager platformTransactionManager(@Autowired DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

```

这样，我们就可以从该管理类获取连接了。

### 实现提交回滚
不是所有的 commit 都需要事务管理，所以，就有了两种状态，需要事务，和不需要事务管理。

我们使用状态模式，定义一个 `TransactionStatus` 来表示状态，如果该类持有连接实例，就表示该连接受到事务管辖。同时，为了保证事务的状态是线程安全的，同时保证在线程内部的共享性，我们将事务的状态封装到 ThreadLocal 中。

剩下的就是 invoke 方法的逻辑了，无非就是方法调用后提交，如果出现异常就回滚。过于简单，这里就不赘述了。

### 修改 JdbcTemplate
在直接我们实现的 JdbcTemplate 是通过 DataSource 拿到连接，这种方式并不能实现事务（因为不能保证每一次拿到的连接是同一个）。这时候，ThreadLocal 中存储的 `TransactionStatus` 就派上用场了，我们可以从其中拿到连接，这样，由于在同一个线程中，拿到的 connection 也必然是同一个，如果结果为 null，就说明不需要事务管理，还是按照原来的逻辑获取连接即可。

