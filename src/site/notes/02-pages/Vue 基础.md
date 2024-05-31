---
{"dg-publish":true,"permalink":"/02-pages/Vue 基础/","tags":["personal/blog","program/frontend/vue"]}
---

# 基本介绍
官网文档：[介绍 — Vue.js (vuejs.org)](https://v2.cn.vuejs.org/v2/guide/index.html)
全面教程：[简介 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/introduction.html)
Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://v2.cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。
# MVVM 思想
## 引入
在 MVVM 之前，开发人员从后端获取需要的数据模型，然后要通过 DOM 操作 Model 渲染到 View 中。而后当用户操作视图，我们还需要通过 DOM 获取 View 中的数据，然后同步到 Model 中。
例如在 Jsp 开发中我们用的各种 el 表达式，jstl 表达式就是例子，我需要**手动动态的渲染**数据，这就给我们带来了很大的麻烦。而且 DOM 操作十分的繁杂，就算有了 JQuery 这种第三库的帮助也不简单。
## 正式介绍
+ M：即 Model，模型，包括数据和一些基本操作；
+ V：即 View，视图，页面渲染结果；
+ VM：即 View-Model，模型与视图间的双向操作 (**无需开发人员干涉**)。这就意味着，我们不需要再进行手动的渲染，VM 会自动的帮我同步数据；
+ 而 MVVM 中的 VM 要做的事情就是把 DOM 操作**完全封装**起来，开发人员不用再关心 Model 和 View 之间是如何互相影响的；
+ 只要我们 Model 发生了改变，View 上自然就会表现出来；当用户修改了 View，Model 中的数据也会跟着改变；
如下图，就是 MVVM 的示意图：
![[01 Vue_基础.png\|01 Vue_基础.png]]
**解读：**
 + View 通过 VM（ViewModel） 与数据模型相绑定；
 + 当 View 发生了变化，VM 中的 Data Listeners 会监听到这种变化，然后改变数据模型中的数据；
 + 当 Model 发生了变化，就会通过 Data Bindings 改变 View 的数据；
# 快速入门
其实这里讲到的 Vue 就是一个大型 js 库，所以根本不用什么其他的环境，只用引入 `Vue.js` 即可。
## 示例代码
```html
<body>
<!--解读
1. div元素不是必须的，也可以是其它元素，比如span,但是约定都是将vue实例挂载到div
2. 因为div更加适合做布局
3. id 不是必须为app , 是程序员指定,一般我们就使用app
-->
<div id="app">
    <h1>欢迎你{{message}}-{{name}}</h1>
</div>
<!--引入vue.js-->
<script src="vue.js"></script>
<script>
    //创建Vue对象实例
    let vm = new Vue({
        el: "#app", //创建的vue实例挂载到 id=app的div
        data: { //data{} 表示数据池(model的有了数据), 有很多数据 ,以k-v形式设置(根据业务需要来设置)
            message: "Hello-Vue!",
            name: "你好"
        }
    })
</script>
</body>
```
**解读：**
 + `{{message}}` : **插值表达式**；
 + message 来源于 model 的 data 数据池；
 + 当我们的代码执行时，会到 `data{}` 数据池中去匹配数据, 如果匹配上, 就进行**替换**，如果没有匹配上, 就是输出空；
 + 当我们在数据池中设置了对应的属性，并且在 html 中使用插值表达式，就意味着打通了 Data Bindings ；
## 注意事项
+ 注意代码顺序，**要求 body 在前，script 在后**，否则无法绑定数据。原因如下：
	+ 这与**浏览器的解析顺序**（从文件上到下）有关。倘若浏览器先解析 `new Vue` 那么，就会导致 Vue 实例挂载失败（`id='#app'` 的 dom 元素还没有被解析）；
# Vue 的绑定机制
## 数据绑定
### 单向渲染
我们通常在 html 中使用**插值表达式** `{{msg}}` 来实现单向渲染**非属性的内容**，若数据池中存在 msg 属性，就会将 msg 替换成数据池中的数据。

由于无法用插值表达式渲染标签属性，于是就要借用 v-bind 指令来渲染。

利用 **v-bind 指令**来渲染标签的属性 attr。
```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>单向数据渲染</title>
</head>
<body>
<div id="app">
    <h1>{{message}}</h1>
    <!--解读
    1. 使用插值表达式引用 data数据池数据是在标签体内
    2. 如果是在标签/元素 的属性上去引用data数据池数据时，不能使用插值表达式
    3. 需要使用v-bind, 因为v-bind是vue来解析, 默认报红,但是不影响解析
    4. 如果不希望看到报红, 直接 alt+enter 引入 xmlns:v-bind
    -->
    <!--<img src="{{img_src}}">-->
    <img v-bind:src="img_src" v-bind:width="img_width">
    <img :src="img_src" :width="img_width">
</div>
<script src="vue.js"></script>
<script>
    let vm = new Vue({
        el: "#app", 
        data: { 
            message: "hello, 耗子精",
            img_src: "1.jpg",
            img_width: "200px"
        }
    })
</script>
</body>
</html>
```
**再次强调：**
 + 使用插值表达式引用 data 数据池数据是在**标签体内**；
 + 如果给**标签属性**绑定值，则使用 v-bind 指令；
### 双向绑定
**v-model** 可以完成双向数据绑定，即：
 + data 数据池绑定的数据变化，会影响 view 【底层的机制是 Data Bindings】；
 + view **关联的的元素值**变化, 会影响到 data 数据池的数据【底层机制是 Dom Listeners】；
简单来说，就是就是 View 可以影响数据池中的数据，从而影响其他的 View。不过，并不推荐使用这个双向绑定，这极**容易导致被攻击**。
```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>双向数据渲染</title>
</head>
<body>
<div id="app">
    <h1>{{message}}</h1>
    <input type="text" v-model="hobby.val"><br/><br/>
    <input type="text" :value="hobby.val"><br/><br/>
    <p>你输入的爱好是: {{hobby.val}}</p>
</div>
<script src="vue.js"></script>
<script>
    let vm = new Vue({
        el: "#app", //创建的vue实例挂载到 id=app的div
        data: { //data{} 表示数据池(model的有了数据), 有很多数据 ,以k-v形式设置(根据业务需要来设置)
            message: "hi, 输入你的爱好",
            hobby: {
                val: "购物"
            }
        }
    })</script>
</body>
</html>
```
## 其他绑定
### 事件绑定
+ 我们可以使用 **v-on** 来进行事件绑定。比如: `v-on:click` 表示处理鼠标点击事件。
+ 事件调用的方法定义在 vue 对象声明的 methods 节点中；
+ 具体的**语法**是`v-on:事件名`；
```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>事件处理</title>
</head>
<body>
<div id="app">
    <h1>{{message}}</h1>
    <!--解读
    1. v-on:click 表示我们要给button元素绑定一个click的事件
    2. sayHi() 表示绑定的方法, 在方法池 methods{} 定义的
    3. 底层仍然是dom处理
    4. 如果方法不需要传递参数，可以省略()
    5. v-on:click可以简写@, 但是需要浏览器支持
    -->
    <button v-on:click="sayHi()">点击输出</button>
    <button v-on:click="sayOk()">点击输出</button>
    <button v-on:click="sayHi">点击输出</button>
    <button @click="sayOk">点击输出</button>
</div>
<script src="vue.js"></script>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            message: "Vue事件处理的案例",
            name: "韩顺平教育"
        },
        //解读:
        // 1. 是一个methods属性, 对应的值是对象{}
        // 2. 在{} 中, 可以写很多的方法, 你可以这里理解是一个方法池
        // 3. 这里需要小伙伴有js的基础=>java web第4章
        methods: {
            sayHi() {
                console.log("hi, 银角大王~");
            },
            sayOk() {
                console.log("ok, 金角大王~");
            }
        }
    })
</script>
</body>
</html>
```
**注意事项：**
 + v-on 指令的**简写**形式 @ ，需要浏览器支持；
 + 如果**方法没有参数**，可以省略 (需要浏览器支持)；
### 修饰符
修饰符 (modifier) 是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：
```
<form v-on:submit.prevent="onSubmit">...</form>
```
具体实例，略，见[事件处理 — Vue.js (vuejs.org)](https://v2.cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6)
### 条件渲染
类似于 jstl 表达式中的 `<c:if>`。
+ 相关的语法为：`v-if="表达式"`，若“表达式”为真，就输出，为假就不输出。
+ 还有 `v-else`，`v-else-if` 用来是实现各种条件的；
+ 若表达式是个变量，就会到 data 数据池中寻找，如果没有就默认为 false；
+ 若表达式是个 js 代码段，就会直接判断；
```html
<div v-if="Math.random() > 0.5">  
Now you see me  
</div>  
<div v-else>  
Now you don't  
</div> 
<!--                              -->
<div v-if="type === 'A'">  
A  
</div>  
<div v-else-if="type === 'B'">  
B  
</div>  
<div v-else-if="type === 'C'">  
C  
</div>  
<div v-else>  
Not A/B/C  
</div>
```
### 列表渲染
相当于 jstl 中的 `<c:foreach>`。
+ 我们可以用 `v-for` 指令基于一个数组来渲染一个列表。`v-for` 指令需要使用 `item in items` 形式的特殊语法，其中 `items` 是源数据数组，而 `item` 则是被迭代的数组元素的**别名**。
```html
<ul id="example-1">  
<li v-for="item in items" :key="item.message">  
{{ item.message }}  
</li>  
</ul>
```
```js
var example1 = new Vue({
	el: '#example-1',  
	data: {    
		items: [      
			{ message: 'Foo' },
			{ message: 'Bar' }    
		]  
	}
})
```
**解读：**
 + 实际上，我们是取出了对象数组 items 中的对象，然后进行遍历，所以我们就可以利用类似于对象解构的方法遍历单个对象的属性；
***
+ 在 `v-for` 块中，我们可以访问所有**父作用域的 property**。`v-for` 还支持一个可选的第二个参数，即当前项的索引。
```html
<ul id="example-2">  
	<li v-for="(item, index) in items">  
		{{ parentMessage }} - {{ index }} - {{ item.message }}  
	</li>  
</ul>
```

```js
var example2 = new Vue({  
	el: '#example-2',  
	data: {  
		parentMessage: 'Parent',  
		items: [  
			{ message: 'Foo' },  
			{ message: 'Bar' }  
		]  
	}  
})
```
***
**用 v-for 来遍历一个对象的 property：**
```html
<ul id="v-for-object" class="demo">  
	<li v-for="value in object">  
		{{ value }}  
	</li>  
</ul>
```
```js
new Vue({  
	el: '#v-for-object',  
	data: {  
		object: {  
			title: 'How to do lists in Vue',  
			author: 'Jane Doe',  
			publishedAt: '2016-04-10'  
		}  
	}  
})
```
你也可以提供第二个（或者更多也行）的参数为 property 名称 (也就是键名)：
```html
<div v-for="(value, name) in object">  
	{{ name }}: {{ value }}  
</div>
```
# 组件化编程
## 引入
+ 在大型应用开发的时候，页面可以划分成很多部分，往往不同的页面，也会有相同的部分。例如可能会有**相同**的头部导航；
+ 但是如果每个页面都独自开发，这无疑增加了我们开发的成本。所以我们会把页面的不同部分拆分成独立的组件，然后在不同页面就可以**共享这些组件**，避免重复开发（如图）；
![[3_01 Vue_基础.png\|3_01 Vue_基础.png]]
## 正式介绍
+ 组件 (Component) 是 Vue. js 最强大的功能之一 (可以提高复用性（1. 界面 2. 业务处理）)；
+ 组件也是一个 Vue 实例，也包括∶ data、methods、生命周期函数等；
+ 组件渲染需要 html 模板，所以增加了 template 属性，值就是 HTML 模板；
+ 对于全局组件，任何 vue 实例都可以直接在 HTML 中通过组件名称来使用组件；
+ data 是一个函数，不再是一个对象，这样每次引用组件都是独立的对象/数据；
## 全局组件
类似于 Java 的静态类，一旦注册了全局组件，就可以在所有的 Vue 实例中使用。
+ 要把组件视为一个 Vue 实例，也有自己的数据池和 methods；
+ 对于组件，我们的数据池的数据，是使用函数/方法返回（目的是为了保证每个组件的数据是独立）, 不能使用原来的方式；
+ 界面通过 template 实现共享, 业务处理也复用；
```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>组件化编程-全局组件</title>
</head>
<body>
<div id="app">
    <h1>组件化编程-全局组件</h1>
    <!--使用全局组件-->
    <counter></counter>
    <br/>
    <counter></counter>
    <counter></counter>
    <counter></counter>
    <counter></counter>
</div>

<div id="app2">
    <h1>组件化编程-全局组件-app2</h1>
    <!--使用全局组件-->
    <counter></counter>
    <counter></counter>
</div>
<script src="vue.js"></script>
<script>
    //1、定义一个全局组件, 名称为 counter
    //2. {} 表示就是我们的组件相关的内容
    //3. template 指定该组件的界面, 因为会引用到数据池的数据，所以需要是模板字符串
    Vue.component("counter", {
        template: `<button v-on:click="click()">点击次数= {{count}} 次【全局组件化】</button>`,
        data() {//这里需要注意，和原来的方式不一样!!!!
            return {
                count: 10
            }
        },
        methods: {
            click() {
                this.count++;
            }
        }
    })
    let vm = new Vue({
        el: "#app"
    })

    let vm2 =  new Vue({
        el: "#app2"
    })
</script>
</body>
</html>
```
## 局部组件
以通过一个普通的 JavaScript 对象来定义组件。把常用的组件放入到某个 js 文件中，类似于 Java 的工具包。
```js
//定义一个组件, 组件的名称为 buttonCounter
    //扩展：
    //1. 可以把常用的组件，定义在某个commons.js中 export
    //2. 如果某个页面需要使用， 直接import
    const buttonCounter = {
        template: `<button v-on:click="click()">点击次数= {{count}} 次【局部组件化】</button>`,
        data() {//这里需要注意，和原来的方式不一样!!!!
            return {
                count: 10
            }
        },
        methods: {
            click() {
                this.count++;
            }
        }
    }
    //创建Vue实例，必须有
    let vm = new Vue({
        el: "#app",
        components: { //引入/注册某个组件, 此时my_counter就是一个组件, 是一个局部组件,他的使用范围在当前vue
            'my_counter': buttonCounter
        }
    })
    let vm2 = new Vue({
        el: "#app2",
        components :{//引入/注册组件buttonCounter
            'hsp_counter': buttonCounter
        }
    })
```

```html
<div id="app">
    <h1>组件化编程-局部组件</h1>
    <!--使用局部组件 ,该组件是从挂载到app的vue中的-->
    <my_counter></my_counter><br/>
    <my_counter></my_counter><br/>
    <my_counter></my_counter><br/>
</div>

<div id="app2">
    <h1>组件化编程-局部组件-app2</h1>
    <!--使用局部组件 -->
    <hsp_counter></hsp_counter><br/>
    <hsp_counter></hsp_counter><br/>
</div>
```

## 总结
+ 组件也是一个 Vue 实例，因此它的定义是也存在∶ data、methods、生命周期函数等；
+ data 是一个函数，不再是一个对象，这样每次引用组件都是独立的对象/数据；
+ 组件渲染需要 html 模板，所以增加了 template 属性，值就是 HTML 模板；

这是深入内容：
[[02-pages/Vue-深入组件-插槽\|Vue-深入组件-插槽]]


更多关于组建的内容请查看官方文档：
[组件基础 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/component-basics.html)


# 生命周期和监听函数（钩子函数）
## 介绍
+ Vue 实例有一个完整的生命周期，也就是说从开始创建、初始化数据、编译模板、挂载 DOM、渲染-更新-渲染、卸载等一系列过程，我们称为 Vue 实例的生命周期；
+ **钩子函数 (监听函数)**: Vue 实例在完整的生命周期过程中 (比如设置数据监听、编译模板、将实例挂载到 DOM 、在数据变化时更新 DOM 等), 也会运行叫做生命周期钩子的函数；
+ **钩子函数的作用**就是在某个阶段, 给程序员一个做某些处理的机会；
如下图，就表示 Vue 的完整生命周期：
![[未命名文件(10).png\|未命名文件(10).png]]
**解读：**
 + 我们将重点关注前三个阶段，在 Vue 实例销毁的阶段我们不去深究；
 + **new Vue ()**。new 了一个 Vue 的实例对象，此时就会进入组件的创建过程。
 + Init Events & Lifecycle。初始化组件的事件和生命周期函数
 + **beforeCreate**。组件创建之后遇到的第一个生命周期函数，这个阶段 data 和 methods 以及 dom 结构都未被初始化，也就是获取不到 data 的值，不能调用 methods 中的函数
 + Init injections & reactivity。这个阶段中, 正在初始化 data 和 methods 中的方法
 + **created**。这个阶段组件的 data 和 methods 中的方法已初始化结束，可以访问，但是 dom 结构未初始化，页面未渲染；在这个阶段，**经常会发起 Ajax 请求**；
 + **beforeMount**。当模板在内存中编译完成，此时内存中的模板结构还未渲染至页面上，看不到真实的数据；
 +  Create vm.$el and replace ‘el’ with it。这一步，再在把内存中渲染好的模板结构替换至真实的 dom 结构也就是页面上；
 +  **mounted**。此时，页面渲染好，用户看到的是真实的页面数据，生命周期创建阶段完毕，进入到了运行中的阶段；
 +  **beforeUpdate**。当执行此函数，数据池的数据新的，但是页面是旧的
 +  Virtual DOM re-render and patch。根据最新的 data 数据，重新渲染内存中的模板结构，并把渲染好的模板结构，替换至页面上；
 + **updated**。页面已经完成了更新，此时，data 数据和页面的数据都是新的
 + **beforeDestroy**。当执行此函数时，组件即将被销毁，但是还没有真正开始销毁，此时组件的 data、methods。数据或方法还可被调用；
 + Teardown……注销组件和事件监听；
 + **destroyed**。组件已经完成了销毁；
