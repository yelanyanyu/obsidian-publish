---
{"dg-publish":true,"permalink":"/01/04-java/02-java-web/017-ajax/","tags":["blog","ajax","front-end"]}
---


# 基本介绍
## 案例引入
假设我们有一个需求，要求在一个页面的局部进行更新数据，例如，以日历翻页的效果显然全网在线的人数实时变动，这个变动只是嵌套在该页面的一个标签里的，例如 div。
我们之前学过的所有技术都无法实现，因为以前的请求都是将一整个表单的数据提交给服务端的，服务端返回响应后，会对整个页面进行刷新，倘若只用该技术实现以上的效果的话，会造成一直刷新页面的严重后果。并且，假如服务端一直都没有响应，那么前端程序就会一直挂起，就是我们经常见到的转圈圈。
于是 ajax 技术就应运而生。
## 基本说明
Ajax 是一种浏览器异步发起请求 (指定发哪些数据)，局部更新页面的技术。
## 经典应用场景
1. 搜索引擎根据用户输入关键字，自动提示检索关键字；
2. 动态加载数据，按需取得数据【树形菜单、联动菜单...】；
3. 改善用户体验。【输入内容前提示、带进度条文件上传...】；
4. 电子商务应用。 【购物车、邮件订阅...】；
5. 访问第三方服务。【访问搜索服务、rss 阅读器】；
6. 页面局部刷新；
# ajax 原理
## 传统的 web 应用
![[Pasted image 20221123221217.png\|Pasted image 20221123221217.png]]
## ajax
![[Pasted image 20221123221234.png\|Pasted image 20221123221234.png]]
**解读：**
 + XMLHttpRequest 对象是浏览器创建的；
 + 若不用 JQuery，则我们需要亲自创建 XMLHttpRequest 对象来进行异步操作；
# 应用
## JS 原生 ajax 使用
参考文档：
	[AJAX 简介 (w3school.com.cn)](https://www.w3school.com.cn/js/js_ajax_intro.asp)
我们以一段实际的案例来看看具体的使用细节：
```JS
//1. 创建XMLHttpRequest对象(!!!) [ajax引擎对象]  
var xhr = new XMLHttpRequest();  
//   获取用户填写的用户名  
var uname = document.getElementById("uname").value;  
  
//2. 准备发送指定数据 open, send//老韩解读  
//(1)"GET" 请求方式可以 GET/POST//(2)"/ajax/checkUserServlet?username=" + uname 就是 url//(3)true , 表示异步发送  
xhr.open("GET", "/ajax/checkUserServlet?uname=" + uname, true);  
  
//在send函数调用前，给XMLHttpRequest 绑定一个事件onreadystatechange  
//该事件表示，可以去指定一个函数，当数据变化，会触发onreadystatechange  
//每当 xhr对象readyState 改变时，就会触发 onreadystatechange 事件  
xhr.onreadystatechange = function () {  
    //如果请求已完成，且响应已就绪, 并且状态码是200  
    if (xhr.readyState == 4 && xhr.status == 200) {  
        //把返回的jon数据,显示在div  
        document.getElementById("div1").innerHTML = xhr.responseText;  
        //console.log("xhr=", xhr)  
        var responseText = xhr.responseText;  
        //console.log("返回的信息=" + responseText);  
        if (responseText != "") {  
            document.getElementById("myres").value = "用户名不可用"  
        } else {  
            document.getElementById("myres").value = "用户名可用"  
        }  
    }  
}  
  
//3. 真正的发送ajax请求[http请求]  
//如果你POST 请求，再send("发送的数据")  
xhr.send();
```
对于 ajax 基本的使用就是这些了。
## JQuery ajax 使用 
之前，我们利用 [[01-计算机笔记/04-Java工程师/02-JavaWeb/015 JQuery基础使用\|JQuery]] 这个 JS 库大大的简化了 DOM 对象的操作。同样，JQuery 也支持 ajax，简化了发送 ajax 异步请求的过程。
参考文档：
	[jQuery AJAX 简介 (w3school.com.cn)](https://www.w3school.com.cn/jquery/jquery_ajax_intro.asp)
### ajax 操作函数一览
划红线的是极其重要的函数，务必留个印象。
![[Pasted image 20221123222554.png\|Pasted image 20221123222554.png]]
### $. ajax 方法
详细说明：
	[jQuery ajax - ajax() 方法 (w3school.com.cn)](https://www.w3school.com.cn/jquery/ajax_ajax.asp)
`$. ajax` **常用参数**：
 +  url： 请求的地址；
 +  type : 请求的方式 get 或 post；
 +  data : 发送到服务器的数据。将自动转换为请求字符串格式；
 +  success: 成功的回调函数，当接受到服务端正常的响应后触发的函数，不是程序员触发的。
 +  error: 失败后的回调函数；
 +  dataType: 返回的数据类型（常用 json 或 text）；
### $. get () 和 $. post () 请求
这两个方法是 `$.ajax({...})` 方法的进一步简化。
**常用参数：**
 + url: 请求的 URL 地址；
 + data: 请求发送到服务器的数据；
 + success: 成功时回调函数；
 + type: 返回内容格式，xml, html, script, json, text；
### $. getJSON ()
这是限定了 type 只能为 json，对 `$.get()` 和 `$.post()` 的简化。
**常用参数：**
 + url: 请求发送的哪个 URL；
 + data: 请求发送到服务器的数据；
 + success: 请求成功时运行的函数；
### 应用实例
```js
//发出ajax  
/**  
 * 解读  
 * 1. 指定参数时，需要在{}  
 * 2. 给参数时，前面需要指定参数名  
 * 3. dataType: "json" 要求服务器返回的数据格式是json  
 */    
 $.ajax({  
        url: "/ajax/checkUserServlet2",  
        type: "POST",  
        data: { //这里我们直接给json, 为啥我要传日期, 为了浏览器缓存  
            username: $("#uname").val(),  
            date: new Date()  
        },  
        error: function () { //失败后的回调函数  
            console.log("失败~")  
        },  
        success: function (data, status, xhr) {  
            console.log("成功");  
            console.log("data=", data);  
            console.log("status=", status);  
            console.log("xhr=", xhr);  
            //data是json对象-> 显示转成json的字符串  
            $("#div1").html(JSON.stringify(data));  
            //对返回的结果进行处理  
            if ("" == data.username) {  
                $("#myres").val("该用户名可用");  
            } else {  
                $("#myres").val("该用户名不可用");  
            }  
        },  
        dataType: "json"  
    })
```
```js
/**  
 * 说明  
 * 1.$.get() 默认是get请求，不需要指定 请求方式  
 * 2.不需要指定参数名  
 * 3.填写的实参，是顺序 url, data, success回调函数, 返回的数据格式  
 */  
  
//讲解.get() 使用  
$.get(  
    "/ajax/checkUserServlet2",  
    {  
        username: $("#uname").val(),  
        date: new Date()  
    },  
    function (data, status, xhr) {  
        console.log("$.get() 成功");  
        console.log("data=", data);  
        console.log("status=", status);  
        console.log("xhr=", xhr);  
        //data是json对象-> 显示转成json的字符串  
        $("#div1").html(JSON.stringify(data));
        //对返回的结果进行处理  
        if ("" == data.username) {
            $("#myres").val("该用户名可用");
        } else {  
            $("#myres").val("该用户名不可用");
        }  
    },  
    "json"  
)

//老师说明$.post() 和 $.get() 的方式一样  
//只是这时，是按照post方式发送ajax请求  
$.post(  
    "/ajax/checkUserServlet2",  
    {  
        username: $("#uname").val(),  
        date: new Date()  
    },  
    function (data, status, xhr) {  
        console.log("$.post() 成功");  
        console.log("data=", data);  
        console.log("status=", status);  
        console.log("xhr=", xhr);  
        //data是json对象-> 显示转成json的字符串  
        $("#div1").html(JSON.stringify(data));  
        //对返回的结果进行处理  
        if ("" == data.username) {  
            $("#myres").val("该用户名可用");  
        } else {  
            $("#myres").val("该用户名不可用");  
        }  
    },  
    "json"  
)
```
```js
//说明  
//1. 如果你通过jquery发出的ajax请求是get 并且 返回的数据格式是json  
//2. 可以直接使用getJSON() 函数，就很简洁  
$.getJSON(  
    "/ajax/checkUserServlet2",  
    {  
        username: $("#uname").val(),  
        date: new Date()  
    },  
    function (data, status, xhr) {//成功后的回调函数  
        console.log("$.getJSON 成功");  
        console.log("data=", data);  
        console.log("status=", status);  
        console.log("xhr=", xhr);  
        //data是json对象-> 显示转成json的字符串  
        $("#div1").html(JSON.stringify(data));  
        //对返回的结果进行处理  
        if ("" == data.username) {  
            $("#myres").val("该用户名可用");  
        } else {  
            $("#myres").val("该用户名不可用");  
        }  
    }  
)
```