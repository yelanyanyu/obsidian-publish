---
{"dg-publish":true,"permalink":"/02-pages/JSP 服务器渲染技术/","tags":["personal/blog","program/backend/framework/jsp","java","program/backend/framework/tomcat"]}
---

# 1. 基本介绍
## 1.1 现状
虽然说，JSP 正在逐渐被取代（目前主流的技术是前后端分离 (比如 : Spring Boot + Vue/React），但是现在仍有不少的使用，而且维护一些的老的项目也需要用到 JSP 技术。我们学习 JSP 技术并不是要学习 JSP 本身，而是为了体会其设计思想，发掘其实现原理。这样我们才会不惧技术的更新换代。
## 1.2 服务器渲染技术
JSP 是一种服务器渲染技术（CSR），所以在正式讲 JSP 之前，我们先简要了解了解什么是服务器渲染技术。
CSR 是 Client Side Render 简称。页面上的内容是我们加载的 js 文件渲染出来的，js 文件运行在浏览器上面，服务端只返回一个 html 模板。看不懂没有关系，我们看下一节。
## 1.3 引出-为什么会有 JSP 技术（CSR 技术）
在没有 CSR 之前，假如我们要返回一个页面只能返回一个静态页面（例如，html 等），浏览器解析的静态资源是已经被固定好了的资源，不会更改，**不会提供动态的数据**。我们期望在静态页面中嵌入一些 java（其他语言同理）代码来**向用户提供动态的数据**。
最开始，我们是通过 servlet 的输出流直接返回 HTML 的，但是有一个问题就是这样返回动态页面会**使得代码的可读性极差，很难进行排版，维护起来会很麻烦**。（如下图）
![Pasted image 20221023214418.png](/img/user/99-Resource/media/Pasted%20image%2020221023214418.png)
**解读：** 上图中的 username 和 pwd 都是需要动态获取的。
为了解决以上问题，就有了 **CSR，其中一个大名鼎鼎的技术就叫做 JSP**。
## 1.4 基本介绍
1. JSP 全称是 Java Server Pages，Java 的服务器页面；
2. JSP 这门技术的最大的特点在于，写 JSP 就像在写 HTML（）JSP 技术的对应的有一个文件叫做 JSP 文件）；
3. jsp 技术基于 Servlet, 你可以理解成 JSP 就是对 Servlet 的包装.；
4. 会使用 JSP 的程序员, 再使用 thymeleaf 是非常容易的事情, 几乎是无缝接；
5. jsp=html+java 片段+标签+javascript+css；
# 2. 基本使用
1. 使用 `<%...%>` 来嵌套 java 代码；
2. 使用 `<%!...%>` 来声明该 jsp 需要使用的属性，方法，静态代码块, 内部类；
具体的细节看代码和总结！（sum. jsp）
```jsp
<%@ page import="java.io.PrintWriter" %>  //这里将相当于导入包import操作
<%@ page import="org.apache.jasper.runtime.HttpJspBase" %>
//这里相当于resp.setContentType("...")操作
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>jsp的简单的求和计算器</title>  
</head>  
<body>  
<h1>jsp的简单的求和计算器</h1>  
<%!  
    //这里我们可以声明该jsp需要使用的属性，方法，静态代码块, 内部类  
    //也就是给 statement.jsp 对应的 statement_jsp 类定义成员  
    //1. 属性  
    private String name = "jack";  
    private int age;  
    private static String company;  
  
    //2 方法  
    public String getName() {  
        return name;  
    }  
    //3. 静态代码块  
    static {  
        company = "字节跳动";  
    }  
%>
<%  
    //解读  
    //1. 在jsp的 该标签中, 可以写java代码  
  
    int i = 10;  
    int j = 20;  
    int res = i + j;  
  
    //2. jsp 中内置对象，可以直接使用, 比如 out
    //在java片段中,仍然是java的注释  
    String email = "xx@qq.com";  
    /*  
        多行注释  
     */  
%>  
<%--email: <%=email%>--%>  
<!--html注释 -->  
</body>  
</html>
```
**解读：**
1. 从这里可以看出，书写 JSP 其实就是在写 servlet；
# 3. 运行原理
1. **jsp 页面本质是一个 Servlet 程序**, 其性能是和 java 关联的, 只是长得丑. 
2. 第 1 次访问 jsp 页面的时候。Tomcat 服务器会把 jsp 页面解析成为一个 java 源文件。并且对它进行编译成为 . class 字节码程序。sum. jsp 对应的 sum_jsp. java 和 sum_jsp. class 文件；
我们直接看 JSP 对应的 java 文件。我们直接从控制台去找（或者用 everything 直接搜也是可以的）；
![Pasted image 20221023215726.png](/img/user/99-Resource/media/Pasted%20image%2020221023215726.png)
![Pasted image 20221023220151.png](/img/user/99-Resource/media/Pasted%20image%2020221023220151.png)
![Pasted image 20221023220243.png](/img/user/99-Resource/media/Pasted%20image%2020221023220243.png)
这就是 tomcat“翻译的页面了”。
![Pasted image 20221023222525.png](/img/user/99-Resource/media/Pasted%20image%2020221023222525.png)
**解读：**
1. 注意到 sum_jsp. java 继承了一个 HttpJspBase 类，我们打开类图看看是什么东西；
![Pasted image 20221023220737.png](/img/user/99-Resource/media/Pasted%20image%2020221023220737.png)
**解读：**
1. 这就一目了然了，HttpJspBase 继承了 HttpServlet，实现了 HttpJspPage，所以 JSP 就是一个 Servlet，而且功能比原生的 Servlet 还要全面很多；
# 4. page 指令
## 4.1 基本介绍
page 指令用来给改 JSP 页面设置一些基本的属性，属于最常用的指令了。
就是上面的这些语句。
![Pasted image 20221023221153.png](/img/user/99-Resource/media/Pasted%20image%2020221023221153.png)
1. language 表示 jsp 翻译后是什么语言文件, 只支持 java ；
2. contentType 表示 jsp 返回的数据类型，对应源码中 response. setContentType () 参数值；
3. pageEncoding 属性表示当前 jsp 页面文件本身的字符集；
4. import 属性跟跟 java 源代码中一样。用于导包，导类；
# 5. JSP 常用脚本
## 5.1 基本介绍
JSP脚本就是Java代码片段，它分为三种：  
1.  `<%...%>` ：Java 语句，该段内容在_jsp. java 文件中直接替换为 java 语句，可以在 jsp 页面中，编写我们需要的功能（使用 java ）。可以由多个代码脚本块组合完成一个完整的 java 语句。代码脚本还可以和表达式脚本一起组合使用，在 jsp 页面上输出数据；
2.  `<%=…%>` ：Java 表达式，相当于 `out.print()`，在 jsp 页面上输出数据；
3.  `<%!...%>` ：用于定义 jsp 的需要属性、方法、静态代码块和内部类等。注意与 page 指令进行区分，page 指令设置的整个文件的，而 JSP 脚本只是设置的是 xxx_jsp. java 类的成员的；
## 5.2 应用实例
```jsp
<%@ page import="java.util.ArrayList" %>  
<%@ page import="com.hspedu.entity.Monster" %>   
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>演示代码脚本</title>  
</head>  
<body>  
<h1>演示代码脚本</h1>  
<%  
    //创建ArrayList ,放入两个monster对象  
    ArrayList<Monster> monsterList = new ArrayList<>();  
    monsterList.add(new Monster(1, "牛魔王", "芭蕉扇"));  
    monsterList.add(new Monster(2, "蜘蛛精", "吐口水"));  
%>  
<table bgcolor="#f0f8ff" border="1px" width="300px">  
    <tr>  
        <th>id</th>  
        <th>名字</th>  
        <th>技能</th>  
    </tr>  
    <% //1号脚本  
        for (int i = 0; i < monsterList.size(); i++) {  
            //先取出monster对象  
            Monster monster = monsterList.get(i);  
    %>  
    <tr>  
        <td><%=monster.getId()%>  
        </td>  
        <td><%=monster.getName()%>  
        </td>  
        <td><%=monster.getSkill()%>  
        </td>  
    </tr>  
    <% //2号脚本  
        }  
    %>  
  
</table>  
</body>  
</html>
```
**解读：**
1. 1 号脚本与 2 号脚本共同组成了一个 for 循环，用于循环输出表格的内容；
# 6. JSP 注释
一共有三种注释。
1. `<%-- --%>`；
2. `//java形式`；
3. `<!-- html形式-->`
# 7. JSP 内置对象
## 7.1 基本介绍
1、JSP 内置对象 (已经创建好的对象, 直接使用 inbuild)，是指 Tomcat 在翻译 jsp 页面成为 Servlet 后，内部提供的九大对象，叫内置对象；
2、内置对象，可以直接使用，不需要手动定义；
## 7.2 对象一览
1. out 向客户端输出数据，out. println ("");
2. request 客户端的 http 请求；
3. response 响应对象；
4. session 会话对象；
5. application 对应 ServletContext ；
6. pageContext jsp 页面的上下文，是一个域对象，可以 setAttribue (), 作用范围只是本页面；
7. exception 异常对象 , getMessage () ；
8. page 代表 jsp 这个实例本身；
9. config 对应 ServletConfig；
# 8. JSP 域对象
## 8.1 基本介绍
### 8.1.1 pageContext
域对象，存放的数据只能在当前页面使用。
![Pasted image 20221023223816.png](/img/user/99-Resource/media/Pasted%20image%2020221023223816.png)
### 8.1.2 request
存放的数据在一次 request 请求有效。
![Pasted image 20221023223848.png](/img/user/99-Resource/media/Pasted%20image%2020221023223848.png)
### 8.1.3 session
存放的数据在一次会话有效。
![Pasted image 20221023223911.png](/img/user/99-Resource/media/Pasted%20image%2020221023223911.png)
### 8.1.4 application
存放的数据在整个 web 应用运行期间有效, 范围更大，只有当服务器重新加载或者重启的时候，才会失效。
![Pasted image 20221023223941.png](/img/user/99-Resource/media/Pasted%20image%2020221023223941.png)
## 8.2 应用实例
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>scope文件</title>  
</head>  
<body>  
<%  
    //在不同的域对象中，放入数据  
    //1. 因为四个域对象，是不同的对象，因此name(key) 相同时，并不会冲突  
    pageContext.setAttribute("k1", "pageContext数据(k1)");  
    request.setAttribute("k1", "request数据(k1)");  
    session.setAttribute("k1", "session数据(k1)");  
    application.setAttribute("k1", "application数据(k1)");  
   
  
    //做一个重定向  
    String contextPath = request.getContextPath();//返回的就是 web路径=>/jsp  
    //response.sendRedirect("/jsp/scope2.jsp");    response.sendRedirect(contextPath + "/scope2.jsp");  
%>  
<h1>四个域对象，在本页面获取数据的情况</h1>  
pageContext-k1: <%=pageContext.getAttribute("k1")%><br/>  
request-k1: <%=request.getAttribute("k1")%><br/>  
session-k1: <%=session.getAttribute("k1")%><br/>  
application-k1: <%=application.getAttribute("k1")%><br/>  
</body>  
</html>
```
## 8.3 注意事项
1. 域对象是可以像 Map 一样存取数据的对象。四个域对象功能一样。不同的是它们对数据的存储范围；
2. 从存储范围 (作用域范围看) pageContext < request < session < application；
# 9. 请求转发标签
`<jsp:forward page="/bb.jsp"></jsp:forward>`，该标签表示把请求转发到 bb. jsp。等价于 `request. getRequestDispatcher ("/bb. jsp"). for...`；
# 10. EL 表达式
## 10.1 基本介绍
1. EL 表达式全称：Expression Language，是表达式语言；
2. EL 表达式主要是**代替 jsp 页面的表达式脚本** `<%=request.getAttribute("xx")%>` ；
3. EL 表达式输出数据的时，比 jsp 的表达式脚本**简洁**；
4. EL 表达式基本语法： ${key1}, 你可以理解就是一个语法糖，直接通过 key1 **取得域对象中的数据；**
## 10.2 EL 常用输出形式
EL 表达式常用输出 Bean 的普通属性、数组属性、List 集合属性和 map 集合属性。我们用具体的案例来说明具体细节。
```jsp
<%@ page import="com.yelanyanyu.bean.Book" %>  
<%@ page import="java.util.List" %>  
<%@ page import="java.util.Map" %>  
<%@ page import="java.util.ArrayList" %>  
<%@ page import="java.util.HashMap" %>  
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>el 表达式输出数据演示</title>  
</head>  
<body>  
<h1>el 表达式输出数据演示</h1>  
<%  
    //创建Book 对象，放入相关的属性  
    //private String name;//书名  
    //private String[] writer;//作者  
    //private List<String> reader;//读者  
    //private Map<String, Object> topics;//评讲  
    Book book = new Book();  
    book.setName("昆虫总动员");  
    book.setWriter(new String[]{"jack", "tom"});  
    ArrayList<String> readers = new ArrayList<>();  
    readers.add("老韩");  
    readers.add("老李");  
    book.setReader(readers);//放入readers  
  
    //创建topics  
    HashMap<String, String> topics = new HashMap<>();  
    topics.put("topic1", "这是我看过的最好的动画片");  
    topics.put("topic2", "不错的电影~~");  
    book.setTopics(topics);  
  
    //把book 放入到request域对象  
    request.setAttribute("bookkey", book);  
  
%>  
book对象: ${bookkey}<br/>  
book.name= ${bookkey.name}<br/>  
book.writer= ${bookkey.writer}<br/>  
book.writer[0]= ${bookkey.writer[0]}<br/>  
  
book.readers= ${bookkey.reader}<br/>  
book.readers第2个= ${bookkey.reader.get(1)}<br/>  
book.readers第2个= ${bookkey.reader[1]}<br/>  
<%-- map 特殊字符 key 可以用[]方式来读取,比如就 book.topics['1'] --%>
book.topics= ${bookkey.topics}<br/>  
book.topics第一个评论= ${bookkey.topics["topic1"]}<br/>  
  
</body>  
</html>
```
## 10.3 EL 运算操作
与 java 几乎全部相同，故略。
## 10.4 EL 的 empty 运算
1. empty 运算可以判断一个数据是否为空，如果为空，返回 true，否则返回 false ；
2. 以下几种情况为空：
	1. 值为 null；
	2. 值为空串的时；
	3. 值是 Object 类型数组，长度为零；
	4. list 集合，元素个数为零；
	5. map 集合，元素个数为零；
## 10.5 EL 的 11 个隐含对象
### 10.5.1 总览
![Pasted image 20221024120103.png](/img/user/99-Resource/media/Pasted%20image%2020221024120103.png)
### 10.5.2  EL 获取四个特定域中的属性
其中特别重要的是以下四个，代表了四个域对象：
![Pasted image 20221024120150.png](/img/user/99-Resource/media/Pasted%20image%2020221024120150.png)
## 10.6 pageContext 对象的使用
我们可以通过 pageContext **获取 http 协议相关的信息**。
协议： ${ pageContext.request.scheme }  
服务器 ip：${ pageContext.request.serverName }  
服务器端口：${ pageContext.request.serverPort }  
工程路径：${ pageContext.request.contextPath }  
请求方法：${ pageContext.request.method }  
客户端 ip 地址：${ pageContext.request.remoteHost }  
会话 id ：${ pageContext.session.id }  
# 11. JSTL 标签库
## 11.1 基本介绍
1. JSTL 标签库是指 JSP Standard Tag Library JSP 标准标签库；
2. EL 表达式是为了替换 jsp 中的表达式脚本，JSTL 是为了替换代码脚本。这样 jsp 页面变得更佳简洁，也可以理解为一种语法糖；
3. **在使用 JSTL 之前**，应该先导入相关包，并将标签 `<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>  ` 写在页首；
4. JSTL 由五个标签库组成，我们这里只简要介绍第一个**核心标签库**：
![Pasted image 20221024162409.png](/img/user/99-Resource/media/Pasted%20image%2020221024162409.png)
## 11.2 核心库常用标签
### <c:set/> 标签
**基本语法**： `<c:set scope="request" var="username" value="yelanyanyu"/>`
set 标签可以往域中保存数据：
	1. 等价域对象. setAttribute (key, value); 
	2. scope 属性设置保存到哪个域 page 表示 PageContext 域（默认值） request 表示 Request 域 session 表示 Session 域 application 表示 ServletContext 域；
	3. var 属性设置 key 是什么；
	4. value 属性设置值；
### <c:if /> 标签
**基本语法**：`<c:if test="${a}">hello</c:if>`；
	1. if 标签用来做 if 判断；
	2. test 属性表示判断的条件（用 EL 表达式输出）；
所以上式用来判断 a 条件，若 a 条件合法且为真，则输出 hello。
### <c:choose> <c:when> <c:otherwise> 标签
这可以直接类比 `switch ... case... default`。这里直接给出实例，立马就可以明白。
```jsp
<%--  
1. 如果${requestScope.score} 那么就明确的指定从request域对象取出数据  
2. 如果${score}, 这是就按照从小到大的域范围去获取 pageContext->request->session->application3.  
--%>
<c:choose>  
    <c:when test="${requestScope.score > 80}">  
        <h1>${score}-成绩优秀</h1>  
    </c:when>  
    <c:when test="${requestScope.score >= 60}">  
        <h1>${score}-成绩一般, 及格了</h1>  
    </c:when>  
    <c:otherwise>  
        <h1>${score}-没有及格，下次努力~</h1>  
    </c:otherwise>  
</c:choose>
```
### <c:forEach/> 标签
这是最为重要的标签之一，我们在浏览器上看到了表格，很多都是利用到了这个语句循环输出。
该标签有四种遍历形式：
	1. 普通遍历输出 i 到 j ；
	2. 遍历数组；
	3. 遍历 Map；
	4. 遍历 List；
我们还是通过实例来理解。
```jsp
<%@ page import="java.util.Map" %>  
<%@ page import="java.util.HashMap" %>  
<%@ page import="java.util.ArrayList" %>  
<%@ page import="java.util.List" %>  
<%@ page import="com.hspedu.entity.Monster" %>  
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>  
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>c:forEach 标签</title>  
</head>  
<body>  
<h1>c:forEach 标签</h1>  
<hr/>  

<%--=========================第1种遍历方式从i到j============================--%>
<h1>第1种遍历方式从i到j</h1>  
<ul>  
    <%--  
    1.遍历 1 到 5，  
    2. 输出 begin 属性设置开始的索引 end 属性设置结束的索引  
    3. var 属性表示循环的变量(也是当前正在遍历到的数据)  
    4. 等价 for (int i = 1; i <= 5; i++) {}    5. 在默认情况下, i 每次会递增1  
    --%>    
    <c:forEach begin="1" end="5" var="i">  
        <li>排名=${i}</li>  
    </c:forEach>  
</ul>  
<hr/>  


<%--=============================第2种遍历方式：遍历数组=======================--%>
<h1>第2种遍历方式：遍历数组</h1>  
<%  
    request.setAttribute("sports", new String[]{"打篮球", "乒乓球"});  
%>  
<%--  
    <c:forEach items="${ requestScope.sports }" var="item"/>    1. items 遍历的集合/数组  
    2. var 遍历到的数据  
    3. 等价 for (Object item: arr) {}--%>  
<c:forEach items="${requestScope.sports}" var="sport">  
    运动名称= ${sport}<br/>  
</c:forEach>  
<hr/> 


<%--==========================第3种遍历方式：遍历Map========================--%>
<h1>第3种遍历方式：遍历Map</h1>  
<%  
    Map<String, Object> map = new HashMap<>();  
    map.put("key1", "北京");  
    map.put("key2", "上海");  
    map.put("key3", "天津");  
    request.setAttribute("cities", map);  
%>  
<%--  
    1. items 遍历的map集合  
    2. var 遍历到的数据  
    3. entry.key 取出key  
    4. entry.value 取出值  
--%>  
<c:forEach items="${requestScope.cities}" var="city">  
    城市信息: ${city.key}--${city.value}<br/>  
</c:forEach>  
<hr/>  


<%--==========================第4种遍历方式：遍历List==================--%>
<h1>第4种遍历方式：遍历List</h1>  
<%  
    List<Monster> monsters = new ArrayList<>();  
    monsters.add(new Monster(100, "小妖怪", "巡山的"));  
    monsters.add(new Monster(200, "大妖怪", "做饭的"));  
    monsters.add(new Monster(300, "老妖怪", "打扫位置的"));  
    request.setAttribute("monsters", monsters);  
%>  
<%--  
    items 表示遍历的集合  
    var 表示遍历到的数据  
    begin 表示遍历的开始索引值 ,从0开始计算  
    end 表示结束的索引值  
    step 属性表示遍历的步长值  
    varStatus 属性表示当前遍历到的数据的状态,可以得到step,begin,end等属性值   
--%>  
<c:forEach items="${requestScope.monsters}" var="monster">  
    妖怪的信息: ${monster.id}-${monster.name}-${monster.skill}<br/>  
</c:forEach>  
</body>  
</html>
```
# JSP 常见问题汇总（持续更新）
1. 为什么在运行 JSP 代码的时候，会出现如下报错？![9f5ad8877286efabd10ace94ee6bde9.jpg](/img/user/99-Resource/media/9f5ad8877286efabd10ace94ee6bde9.jpg)
   这一般是 tomcat 和 jdk 不匹配导致的，建议将 jdk 切换为低版本的。
2. 为什么我的 jstl 或者 el 表达式无法在前端显示？首先，请检查你的包引全了没有。如果都引全了，那么可能是你的 webapp 的版本过高导致无法显示。请添加如下的代码：
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
```

# JSP 与 Tomcat 的兼容性问题
详细见：
[[02-pages/Tomcat10.x-jsp-jstl-el 问题\|Tomcat10.x-jsp-jstl-el 问题]]