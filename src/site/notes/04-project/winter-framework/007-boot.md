---
{"dg-publish":true,"permalink":"/04-project/winter-framework/007-boot/","tags":["personal/blog"]}
---


# 启动 Tomcat
实际上就是使用嵌入式的 tomcat。这能够让我们从 main 方法中启动直接 tomcat。
嵌入式 tomcat 的设置是一个固定的模式，这里直接展示：
```java
    public void start(String webDir, String baseDir, Class<?> configClass, String... args) throws Exception {
        // start info:
        final long startTime = System.currentTimeMillis();
        final int javaVersion = Runtime.version().feature();
        final long pid = ManagementFactory.getRuntimeMXBean().getPid();
        final String user = System.getProperty("user.name");
        final String pwd = Paths.get("").toAbsolutePath().toString();
        log.info("Starting {} using Java {} with PID {} (started by {} in {})", configClass.getSimpleName(), javaVersion, pid, user, pwd);

        var propertyResolver = WebUtils.createPropertyResolver();
        Server server = startTomcat(webDir, baseDir, configClass, propertyResolver);

        // started info:
        final long endTime = System.currentTimeMillis();
        final String appTime = String.format("%.3f", (endTime - startTime) / 1000.0);
        final String jvmTime = String.format("%.3f", ManagementFactory.getRuntimeMXBean().getUptime() / 1000.0);
        log.info("Started {} in {} seconds (process running for {})", configClass.getSimpleName(), appTime, jvmTime);

        server.await();
    }

    protected Server startTomcat(String webDir, String baseDir, Class<?> configClass, PropertyResolver propertyResolver) throws Exception {
        int port = propertyResolver.getProperty("${server.port:8080}", int.class);
        String contextPath = propertyResolver.getProperty("${server.context-path:}");
        log.info("starting Tomcat at port {}...", port);
        Tomcat tomcat = new Tomcat();
        tomcat.setPort(port);
        tomcat.getConnector().setThrowOnFailure(true);
        Context ctx = tomcat.addWebapp(contextPath, new File(webDir).getAbsolutePath());

        WebResourceRoot resources = new StandardRoot(ctx);
        resources.addPreResources(new DirResourceSet(resources, "/WEB-INF/classes", new File(baseDir).getAbsolutePath(), "/"));
        ctx.setResources(resources);
        ctx.addServletContainerInitializer(new ContextLoaderInitializer(configClass, propertyResolver), Set.of());

        tomcat.start();
        log.info("Tomcat started at port {}...", port);

        return tomcat.getServer();
    }

```

重点是如何注册我们最为熟悉的 Servlet 组件，包括 Servlet、Filter、Listener 等。在这个实现上 Spring 的架构实在是赏心悦目。为了尽早看到效果，我们暂时简单的实现。

当我们启动 tomcat 容器的时候，我们可以注册一个 ServletContainerInitializer。这样，Tomcat 启动后，就会自动执行里面的 onStartUp 方法，我们可以在里面注册组件，比如说 ioc 容器、filter、DispatcherServlet 等。

这里直接给出实现：
```java
    @Override
    public void onStartup(Set<Class<?>> set, ServletContext servletContext) throws ServletException {
        WebMvcConfiguration.setServletContext(servletContext);
        // Start the application context.
        ApplicationContext ioc = new AnnotationConfigApplicationContext(configClass, propertyResolver);
        log.info("ioc: {}", ioc);
        registerFilters(servletContext);
        WebUtils.registerDispatcherServlet(servletContext, propertyResolver);
    }
```

对于 `registerFilters` 方法，就是将 spring 注册到 ServletContext 中，大致的过程就是，从 ioc 容器中拿到所有的 FilterRegistrationBean，该 Bean 封装了所有的 Filter 信息和 Filter 实例，将这些信息注册到 ServletContext 就行了。


# 动态注册 Servlet 组件
Spring 的实现架构如下：
![FilterRegistrationBean.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/FilterRegistrationBean.png)
>## 解读
>+ 当观察 RegistrationBean 的源码后，可以断定这是一种模板方法模式。该类提供一个 final 类型的 onStartUp 方法，用来注册各个组件，还有一个抽象方法 register 由子类实现；
## 动态注册 Filter
### 概述
要完成的设置一个 Filter 需要注入以下属性：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/20231221215228.png)


这么多的属性，我们不可能在一个类中全部完成。而且这不利于拓展，例如，如果还要实现动态注册 Servlet 的功能，那么有很多属性是重复的，就会造成代码的冗余和冲突。当然，这里有一些标签，我们是不打算完整支持，因为这样的工作量就太大了。不打算实现的属性有：
 + value；
 + asyncSupport；
 + display-name；
 + servletNames；
 + description；

我们曾经在 web 模块下有这样一个工具类 `WebUtils`，我们可以使用这个工具类注册 DispatcherServlet。

但是当初架构的时候，并没有考虑到我们可能会进行动态注册组件，所以这里就造成了组件拓展的困难。所以，我们不得不借助 Spring 的架构进行重构。

### RegistrationBean
Spring 中提供了以下两种基础属性：
```java
	private int order = Ordered.LOWEST_PRECEDENCE;
	private boolean enabled = true;
```
order 属性很重要，这直接规定的过滤器的执行顺序，

### DynamicRegisterationBean
>### 概述
>+ 定义 register 的基本逻辑（关键部分子类实现）：
>	+ 向 Servlet 容器中加入动态组件；
>	+ 向该组件注册相关参数说明，例如 url-pattern，init-param 等；
>+ 声明动态组件的名字；


该 Bean 是所有动态创建组件类的父类，用来表示可以动态创建的组件。在该类中实现了 RegistrationBean 中的抽象方法 register。

随后，又声明了一个抽象方法 `addRegisteration()` 等待实现。该返回的类型必须继承 `Registration.Dynamic`。因为，Servlet-api 中，是通过 Dynamic 来进行各种参数的绑定的，例如 url-pattern，同时，也是组件动态注册的支持之一。

在子类的实现中，`addRegisterration()` 中应该至少完成组件的实例化，例如调用 `servletContext.addFilter(...)`。
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/20231221205856.png)

### AbstractFilterRegistrationBean
>### 概述
>+ 设置 url-pattern；
>+ 设置 dispatcherTypes，用于制定 filter 的拦截方式。[Filter过滤器的拦截方式配置_dispatchertypes-CSDN博客](https://blog.csdn.net/lgl782519197/article/details/107806739)。
>+ 以及对过滤器顺序起绝对作用的 isMatchAfter 属性的设置；

这里 Spring 设置了一个默认的 url-pattern（`/*`），全部拦截。若 filter 并没有设置 url-pattern，那么就默认拦截所有请求。
### FilterRegistrationBean
>### 概述
>+ 设置 filter 实例——通过构造器获得；

### 确定执行顺序
在传统的 JavaWeb 开发中，Servlet 容器是根据过滤器在 `web.xml` 中的注册顺序来确定过滤器的执行顺序的（前的优先级高）。

但是 boot 应用是脱离 xml 配置文件的，所以，我们需要想办法重新设置 filter 的执行顺序。`Servlet 3.0` 后引入了 `@WebFilter` 注解，tomcat 根据 filterName 的字典序来确定执行顺序。

问题，出现了，动态组件的注册是否也遵循这个规律呢？如何验证这个规律？这恐怕得要设计实验进行验证了。

我暂时找到了如下的资料：
[动态注册之Servlet+Filter+Listener - 简书 (jianshu.com)](https://www.jianshu.com/p/cbe1c3174d41)
[【Java Web开发学习】Spring MVC添加自定义Servlet、Filter、Listener - 翠微 - 博客园 (cnblogs.com)](https://www.cnblogs.com/yangchongxing/p/9968483.html)
里面讲到 Filter 的执行顺序由其在 onStartup 方法中的添加顺序决定，为 false 的优先级比为 true 高。

***
>### 这或许预示着，我需要进行如下测试，来确定机制：
>+ 将 isMatcherAfter 全部设置为 true 或 false，添加多个 filter，查看调用顺序是否按照添加顺序排序；
>+ 添加多个 filter，但是有一个 filter（非最后一个添加到 context 中） 设置为了 false，其余全部为 true。观察该 filter 是否是最后一个被调用；
>+ 添加多个 filter，但是有一个 filter（非最后一个添加到 context 中） 设置为了 true，其余全部为 false。观察是否该 filter 是否是第一个被调用；
***

经过我一天的努力，这个问题已经被我完全的解决了。[[04-project/winter-framework/011-测试记录#Servlet-Filter 执行顺序测试\|011-测试记录#Servlet-Filter 执行顺序测试]]。

### 使用
1. 先在 `@Configuration` 中注册 FilterRegistrationBean（用 `@Bean` 标识）；
2. 设置各种属性，如果要设置执行顺序，请务必设置 order 属性，用 `@Order` 标识，这样就可以实现过滤器链的效果了。

## 动态注册 Servlet


# 直接部署支持
我们可以直接在 IDEA 中直接启动 winter-Framework 项目，也可以将项目打包成 war 包放在 tomcat 的 webapp 中直接运行（实际上这是没有意义的，这个模块的开发初衷就是为了避免安装原生的 tomcat），但是 spring 还支持直接使用 java 命名在任何地方直接运行该 web 项目。

## 找到 Main

如果我们什么也不干，直接使用 java 命令是行不通的，会报错 Main 找不到等问题，具体的原因涉及到 classLoader 的机制。

由于 JVM 默认的 ClassLoader 是从 jar 的根目录开始查找的，而我们的项目的结构并不满足。JVM 能够查找到的结构为：
```text
xyz.war
	com
		yelanyanyu
			boot
				Main.class
```

而我们直接通过 maven 打包成的 war 却是这样的：
```text
xyz.war
	WEB-INF
		classes
			com
				yelanyanyu
					boot
						Main.class
```

我们并不希望直接改变 classpath，因为这会给用户很大的麻烦，程序就失去了一些透明性。

所以，我们不得不尝试把 classes 下的所有 class，放到 war 的根目录下，即同时具备 Tomcat 的目录结构，例如：
```text
xyz.war
	com
		yelanyanyu
			boot
				Main.class
	WEB-INF
		classes
		libs
```

这样，才可以加载到 `Main.class`。但是，问题还没有解决，就算有了这样的结构，还是会报错类（`WinterApplication`）找不到。
## 解压 Jar 包
我们知道一个项目可能依赖很多的 jar 包运行，而 JVM 在解析的时候，不支持直接在 libs 目录下搜索 class。所以，我们需要手动解压。并添加一个 classpath，让 JVM 能够解析。

具体的步骤，现在 Main 中，解压 jar，然后在 pom 中，声明另一个 classpath。

具体的步骤就略过了。