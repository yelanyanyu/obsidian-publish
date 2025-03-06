---
{"dg-publish":true,"permalink":"/02-pages/Cookie/","tags":["personal/blog"]}
---

## 2.1 Cookie
### 2.1.1 基本介绍
Cookie (小甜饼) 是客户端技术，服务器把每个用户的数据以 cookie 的形式写给用户各自的浏览器。当用户使用浏览器再去访问服务器中的 web 资源时，就会带着各自的数据去。这样， web 资源处理的就是用户各自的数据了。
但是 Cookie 只是存储在浏览器本地的，他与这个会话并没有一一对应的关系，这其实就是与 Session 的主要区别。
**注意事项：**
1. Cookie 是服务器在客户端保存用户的信息，比如登录名，浏览历史等, 就可以以 cookie 方式保存. 
2. Cookie 信息就像是小甜饼 (cookie 中文) 一样，数据量并不大，服务器端在需要的时候可以从客户端/浏览器读取 (http 协议)，可以通过图来理解；
### 2.1.2 Cookie 原理解析
![[Pasted image 20221016214816.png\|Pasted image 20221016214816.png]]
**解读：**
1. 服务端接收到浏览器发来的请求；
2. 服务端执行相关的业务，假设这里创建了几个 Cookie（注意这里的 Cookie 还未打入浏览器，只存在于服务端），并放在了响应体中；
3. 之后服务端将响应打回浏览器；
4. 浏览器解析响应，并在本地创建 Cookie；
### 2.1.3 Cookie 生命周期
1. Cookie 的生命周期指的是如何管理 Cookie 什么时候被销毁（删除） ；
2. setMaxAge (秒数) ：
	1. 正数，表示在指定的秒数后过期； 
	2. 负数，表示浏览器关闭，Cookie 就会被删除（默认值是-1）。即代表这个 Cookie 是会话级的； 
	3. 0，表示马上删除 Cookie，浏览器会直接进行删除，即不在本地存储了；
**原理：** 创建 Cookie 时，请求中会自带失效的时间，当浏览器再次请求的时候，发现时间超过了规定的生命周期，就下次请求就不会带上 Cookie，并进行本地的删除。见下图：
![[Pasted image 20221016220508.png\|Pasted image 20221016220508.png]]
**解读：**
1. 当生命周期设置为 0 时，Cookie 的失效时间被设置为默认时间 1970 年；
### 2.1.4 Cookie 有效路径
#### 2.1.4.1 基本介绍
1. Cookie 有效路径 Path 的设置； 
2. Cookie 的 path 属性可以有效的**过滤哪些 Cookie 可以发送给服务器，哪些不发**。 path 属性是通过请求的地址来进行有效的过滤；
#### 2.1.4.2 基本规则
```java
cookie1.setPath = /工程路径 
cookie2.setPath = /工程路径/aaa 

请求地址url: http://ip:端口/工程路径/资源 
cookie1 会发给服务器 
cookie2 不会发给服务器 

请求地址url: http://ip:端口/工程路径/aaa/资源 
cookie1 会发给服务器 
cookie2 会发给服务
```
**解读：**
1. 如果 Path 为请求 url 子集的时候，就可以发送给服务器；
2. 不管请求资源是否存在，只要满足上一规则，就会发送这个 Cookie；
### 2.1.5 应用实例
1. 保存用户名密码，在一定时间内实现自动登录；
2. 保存上次登录时间；
3. 网页的个性化定制；
4. ...
### 2.1.6 Cookie 基本使用
Cookie 本质就是一对 k-v。
#### 2.1.6.1 创建
```java
Cookie cookie = new Cookie(String name, String val); 
cookie.setMaxAge();//保存时间 
```
#### 2.1.6.2 添加到浏览器（客户端）
```java
response.addCookie(cookie);
```
#### 2.1.6.3 读取请求中的 Cookie
```java
Cookie[] cookies = request.getCookies();
cookie.getName();
cookie.getValue();
```
### 2.1.7 注意事项
1. 一个 Cookie 只能标识一种信息，它至少含有一个标识该信息的名称（NAME）和设置值 （VALUE），就相当于一条数据，而不是键值对集合；
2. 一个 WEB 站点可以给一个浏览器发送多个 Cookie，一个浏览器也可以存储多个 WEB 站点提供的 Cookie；
3. cookie 的总数量没有限制，但是每个域名的 COOKIE 数量和每个 COOKIE 的大小是有限制的 (不同的浏览器限制不同, 知道即可) , Cookie 不适合存放数据量大的信息；
4. 注意，删除 cookie 时，path 必须一致，否则不会删；
### 2.1.8 Cookie 中文乱码解决方案
如果存放中文的 cookie, 默认报错, 可以通过 URL 编码和解码（利用 URLDecoder 和 URLEncoder 类）来解决，**不建议存放中文的 cookie 信息。**
**实例：**
#### 2.1.8.1 中文编码
```java
//以utf-8的编码方式保存“我爱你”
String urlName = URLEncoder.encode("我爱你", "utf-8");
Cookie nameCookie = new Cookie("Love", urlName); //添加 cookie[Love-我爱你] response.addCookie(nameCookie); 
Cookie cookie2 = new Cookie("key1", "key1_value"); 
response.addCookie(cookie2);
```
#### 2.1.8.2 中文解码
```java
response.setContentType("text/html;charset=utf-8"); 
PrintWriter writer = response.getWriter(); 
Cookie[] cookies = request.getCookies(); //处理的中文乱码问题 
for (Cookie cookie : cookies) {
	//以utf-8的方式解码
	writer.println("Cookie[" + cookie.getName() + "=" + 
                      URLDecoder.decode(cookie.getValue(),"utf-8") + "]  
"); 
}
```