---
{"dg-publish":true,"permalink":"/04-project/winter-framework/004-webmvc/","tags":["blog"]}
---


# 核心组件
+ `DispatcherServlet`；
+ `@Controller`；
+ `@RestController`。这里跟 springmvc 的逻辑有所不同，为了体现 Rest 请求的风格，这个含义变为：
	+ 如果有 controller 被改注解注释就意味着不进行视图渲染，直接以 Http body 的形式返回；
	+ 该注解不再同时具有 `@ResponseBody` 的功能。即若 handler 的返回值为 String 或 byte，那么你需要额外使用注解 `@ResponseBody` 才能够直接返回（不走视图）； 
+ `ViewResolver`；

# 启动 ioc 容器
## 加载 PropertyResolver
我们有两种类型的文件（`.properties` 和 `.yml` ）需要支持。优先支持 yml，如果 yml 没有再读取 `properties` 文件。但是由于实际环境中，可能会有空的配置文件的存在的情况，所以我们不能粗暴的直接根据文件是否为 Empty 来读取。

并且，我们会发现如果再写一个获取 ClassLoader 的代码，会造成一定的冗余，我们期望的是一种更为强大的工具，使得我们给出一个 inputStream，可以得到我们想要的各种形式的资源，例如在 Resource ，这里的 prop 文件。这就是**函数式接口**的应用场景了。



# 实现 MVC
## 加载 DispatcherServlet
我们通过监听器来创建 DispatcherServlet。当监听到 Servlet 容器创建的时候，来创建 ioc 容器，同时注册 DispatcherServlet。

## DispatcherServlet 细节
### 成员属性
+ 维护两个 list，getDispatchers 和 postDispatchers 分别用来存储 get 请求和 post 请求；
+ 维护 ioc 容器的引用，用来得到容器中的组件进行调用；
+ 维护 ViewResovler，用来视图解析，返回信息给前端；

### init
这里主要进行初始化工作，当 servlet 被创建的时候，我们需要初始化所有的 Controller 和 urlPattern，将 urlPattern 与 Handler 进行绑定，以至于在之后接受 get 和 post 请求的时候，可以做到路由转发。

+ 我们将各个请求抽象成类 `Dispatcher`，该类不仅仅存储了各个请求的信息，还为请求规定了应该调用的 Handler；


### Handler 路径匹配
根据对应的 uri 找到应该调用的 Handler。

我们首先知道 uri 的格式为：`../[...]/[...]`，而要与之进行匹配的 Handler 的 URL 的格式为 `/[...]/{...}`，我们还要拿出 path variable。所以，这就需要用上正则匹配了。


### 请求类型分发处理
这两个方法主要分别用于处理 get 和 post 请求。对于 get 请求，可能有两种情况：
 + 获取静态资源，例如 icon 或者 static 目录下的资源（html）；
 + 获取动态资源，例如 Servlet，controller；
故而，对于 doGet 方法，应该分而治之。

对于 post 请求，只有可能请求动态资源。

### 调用 controller 方法
分而治之后，我们应该想办法调用 controller 中的方法，为了调用这个方法，我们需要先填充方法的参数（体现在方法 `Dispatcher.process()`）。
 + 参数有几种类型，不同类型的参数需要不同的处理方法，例如被 `@RequestBody` 修饰的参数就说明 Request 传输过来的是 json 格式的字符流，需要调用 `reader.getReader()`，然后进行 json 解析；如果参数类型为 `@PathVariable` 修饰的，那么就需要对 uri 进行正则匹配，然后进行填充...
 + 参数总共有四种，每种都需要进行特别的处理；
***

当参数填充完毕后，我们要用参数数组来 invoke 相关的方法。最后将结果返回。

### 将结果进行返回
假如我们能够正确调用方法，并返回方法返回值，那么就要对返回值进行进一步的处理。

返回值的有几种类型：
 + `void` 或 `null`：表示内部已处理完毕；
 - `String`：如果以 `redirect:` 开头，则表示一个重定向；
 - `String` 或 `byte[]`：如果配合 `@ResponseBody`，则表示返回值直接写入响应；
 - `ModelAndView`：表示这是一个 MVC 响应，包含 Model 和 View 名称，后续用模板引擎处理后写入响应；不论是 String 还是 Object，本质上都会封装成 ModelAndView 对象，然后调用视图解析器渲染或者返回相关数据；
 - 其它类型：如果是 `@RestController`，则序列化为 JSON 后写入响应。

不符合上述要求的返回类型则报500错误。






## 视图解析
> 当我们从 controller 成功的调用了相应的方法，我们就要尝试将数据返回给前端了，如何根据 controller 中方法的返回类型来规定响应的内容呢？这就需要进行视图解析了。



### 确定 content-type
>为了简单起见，我们仅仅支持几种最常用的 content-type 返回。并且将这些代码写死，也就是说，暂时无法根据请求头来动态确定返回的响应头中的 header。

支持的 content-type：
 + `appllication/json`。
	 + 对于 Rest 类型的 Dispatcher，默认的 content-type 就为 `application/json`。除了，被其修饰的 `byte[]` 类型；
	 + 对于非 Rest 类型的 Dispathcer，如果被 `@ResponseBody` 修饰，那么默认返回该类型；
 + `text/html`。非 Rest 类型的 Dispatcher 的默认值。
 + `application/octet-stream`。如果返回的数据为 `byte[]`，那么就默认为该类型；

### 视图解析流程
springmvc 的视图的流程可以表示为：
	![004-webmvc-20231109](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/004-webmvc-20231109.png)

我们对其进行一定的简化，如下图所示：
![DispatcherServlet_doService 1](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/DispatcherServlet_doService%201.svg)


上图中有两个很重要的组件，担当了视图解析的核心功能。一个是 `ViewResovler`，一个是 `View`。

其中 `ViewResovler` 的作用大致为：根据 handler 返回的类型确定视图 `View`。简单来说，就是确定返回的后端数据类型。

View 就是我们返回数据 body 的抽象，为了简化流程（在 Springmvc 中，还需要经过 HttpOutputMessage 的处理），就在 View 中通过方法 `renderMergedOutputModel` 直接向浏览器返回数据了。


### 对 jsp 的处理
当我们想要请求转发或者重定向到 jsp file 的时候，我们就需要对 jsp 进行一些必要的准备：
 + 设置 request 域的一些参数，这些一方面是默认的 model，一方面是人为添加的 model。以便我们在 controller 中随时使用；
 + jsp 视图类名为 `JstlView`；



# 开发 Web 应用
>至此，框架的基本功能已经完成。故而，这一章节，主要专注于框架的测试方案。

# Bug 以及解决
## tomcat 报错 ClassNotFound
如下图所示：
![004-webmvc-20231107](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/004-webmvc-20231107.png)
**解决：**
 + 原因是 tomcat 加载路径的外部包中，并没有引入我们开发的 `yelanyanyu-mvc`，导致找不到；
### 解决方法
![004-webmvc-20231107_1](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/004-webmvc-20231107_1.png)
在 mvc 的类加载路径下的 lib 中加入即可解决这个问题。


