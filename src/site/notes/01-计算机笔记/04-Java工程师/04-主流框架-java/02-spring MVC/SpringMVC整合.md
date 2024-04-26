---
{"dg-publish":true,"permalink":"/01/04-java/04-java/02-spring-mvc/spring-mvc/","tags":["blog","spring","springmvc","mybatis","IDEA"]}
---

# 基础环境搭建
## IDEA 创建项目
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240418144007.png)

## pom 文件
```xml
<dependencies>
    <!-- JUnit 测试框架的依赖，用于编写和执行测试用例 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>

    <!-- Spring Web MVC 框架的依赖，提供了构建Web应用程序的MVC架构 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.8</version>
    </dependency>
    
    <!-- Spring AOP 的依赖，提供了面向切面编程的实现 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>5.3.8</version>
    </dependency>

    <!-- Spring JDBC 的依赖，简化了JDBC数据访问操作 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.3.8</version>
    </dependency>

    <!-- MyBatis 持久层框架的依赖，用于操作数据库 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.13</version>
    </dependency>

    <!-- MyBatis-Spring 的依赖，用于集成 MyBatis 到 Spring 中 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.6</version>
    </dependency>

    <!-- 阿里巴巴的 Druid 数据库连接池依赖，提供数据库连接池服务 -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.6</version>
    </dependency>

    <!-- MySQL 数据库驱动的依赖，用于连接 MySQL 数据库 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.49</version>
    </dependency>
</dependencies>

```

##  `web.xml` 配置
```xml
<!DOCTYPE web-app PUBLIC
        "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
        "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
    <!-- 应用的显示名称 -->
    <display-name>Archetype Created Web Application</display-name>
	<absolute-ordering/>
    <!-- Spring的上下文参数，指定Spring配置文件的位置 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>

    <!-- ContextLoaderListener监听器，用于在Web容器启动时装配ApplicationContext -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!-- SpringMVC的DispatcherServlet配置，处理所有请求 -->
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- 字符编码过滤器，用于设置请求和响应的编码格式为UTF-8 -->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!-- HiddenHttpMethodFilter用于支持REST风格的HTTP方法 -->
    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>

```

##  `dispatcher-servlet.xml` 创建
在 `WEB-INF` 目录下创建该文件：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <context:component-scan base-package="com.yelanyanyu.ssm_test" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/"></property>
        <property name="suffix" value=".html"></property>
    </bean>
    <mvc:default-servlet-handler/>
    <mvc:annotation-driven/>
</beans>

```

##  `applicationContext` 创建
在 `resource` 目录下创建：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <context:component-scan base-package="com.yelanyanyu.ssm_test">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

</beans>

```

## 创建测试 Controller
创建好对应的包，以及测试 Controller。
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240418143723.png)

## 测试
1. 配置 Tomcat；
2. maven 执行 clean 命令；
3. 在 `WEB-INF/views/` 下创建测试 html `hi.html`；
4. 打开 Tomcat 后，浏览器输入 `localhost:port/test` 会有如下结果：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240418143850.png)

# mybatis 整合
## jdbc 配置
创建配置文件 `resources\jdbc.properties`：
```properties
jdbc.driver=com.mysql.jdbc.Driver (or other driver)
jdbc.url=your url
jdbc.user=your username
jdbc.pwd=your pwd

```

## 配置数据源 DataSource
