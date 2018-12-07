# 目录结构

> bulid—————————————项目构建webpack相关代码
> 
> config————————————配置目录
> 
> node_modules——————npm加载的项目依赖模块
> 
> src————————————————开发目录
> 
> > assets————————————图片 logo
> > 
> > components————————组件
> > 
> > App.vue———————————项目入口文件，组件可写在这里
> > 
> > main.js———————————项目的核心文件
> 
> static——————————————静态资源目录
> 
> test———————————————初始测试目录
> 
> index.html—————————首页入口文件
> 
> package.json————————项目配置文件
> 
> README.md————————————项目说明文档

## 实例化Vue

> var vm=new Vue({//选项})

## 格式

> new Vue({ el:'xxx',data:{},methods:{} })

*   el 对应DOM中的id

*   data 用于定义属性

*   methods/computed 定义函数 可通过return 返回函数值

*   {{ }} 用于输出对象属性和函数返回值

*   可用前缀$与用户自定义属性区分

    > vm.el=vm.$el=document.getElementById('vue_det')

## 模板语法

*   使用了基于 HTML 的模版语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。

*   核心是一个允许你采用简洁的模板语法来声明式的将数据渲染进 DOM 的系统。

*   结合响应系统，在应用状态改变时， Vue 能够智能地计算出重新渲染组件的最小代价并应用到 DOM 操作上

## 插值

### 1.文本

> {{}}   文本插值

### 2.HTML

> v-html   &lt;div v-html='msg'&gt;&lt;/div&gt; 输出html代码
> 
> data:{msg='&lt;h1&gt;xxx&lt;/h1&gt;'}

### 3.属性

> v-bind 绑定dom

### 4.表达式

> 提供js表达式支持  
> 
> &lt;div id="app"&gt;
>     {{5+5}}&lt;br&gt;
>     {{ ok ? 'YES' : 'NO' }}&lt;br&gt;
>     {{ message.split('').reverse().join('') }}
>     &lt;div v-bind:id="'list-' + id"&gt;菜鸟教程&lt;/div&gt;
> &lt;/div&gt;
> 
> &lt;script&gt;
> new Vue({
>   el: '#app',
>   data: {
>     ok: true,
>     message: 'RUNOOB',
>     id : 1
>   }
> })
> &lt;/script&gt;

## 指令

文件  关于
883 Words Vue(二).目录结构和基本语法.md

> &lt;script&gt;
> new Vue({
>   el: '#app',
>   data: {
>     ok: true,
>     message: 'RUNOOB',
>     id : 1
>   }
> })
> &lt;/script&gt;

## 指令

> 带有v-前缀的特殊属性
> 
> 带有v-前缀的特殊属性
> 
> 用于在表达式的值改变时，将某些行为应用到dom上
> 
> 例如 v-if='seen'  可以控制seen的值为false或者true 来控制是否插入元素

### 参数

> 在指令后一冒号指明 
> 
> v-bind 设置HTML属性 v-bind:href 缩写:href
> 
> v-on 绑定HTML事件 v-on:click 缩写 @click
> 
> 例如 v-bind:href='url' href是参数 告知v-bind指令将该元素的href属性与表达式的url的值绑定  
> 
> v-on:click=‘doSomething’ 参数是监听的事件名

### 修饰符

> 以半角句号.指明的特殊后缀 用于指出一个指令应以特殊方式绑定
> 
> 例如 .prevent修饰符告诉v-on指令对于触发的事件调用event.preventDefault()

`<form v-on:sumit.prevent="onSubmit"></form>`

## 用户输入

*   v-model

    > 用来在input、select、text、checkbox、radio等表单控件上创建双向数据绑定,用在其他控件无效果

*   v-on

    > 按钮可以使用v-on监听事件,并对用户的输入进行响应
> 
>         <span class="xml">        <span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"xml"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-tag"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<<span class="hljs-attribute">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-title"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>>div<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-attribute"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>>id<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-value"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"app"<span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"></<span class="hljs-title">span</span>></span>
>     <span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-tag"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<<span class="hljs-attribute">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-title"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>>p<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-expression"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>></span><span class="hljs-expression">{{ <<span class="hljs-variable">span</span> <<span class="hljs-variable">span</span> <span class="hljs-variable">class</span>=<span class="hljs-string">"hljs-keyword"</span>><span class="hljs-variable">class</span><<span class="hljs-end-block">/span</span>>=<<span class="hljs-variable">span</span> <span class="hljs-variable">class</span>=<span class="hljs-string">"hljs-string"</span>><span class="hljs-string">"hljs-variable"</span><<span class="hljs-end-block">/span</span>>><span class="hljs-variable">message</span><<span class="hljs-end-block">/span</span>> }}</span><span class="xml"><span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"xml"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-tag"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"></<<span class="hljs-attribute">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-title"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>>p<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"></<span class="hljs-title">span</span>></span>
>     <span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-tag"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<<span class="hljs-attribute">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-title"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>>button<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-attribute"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>>v-on:click<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-value"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"reverseMessage"<span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"></<span class="hljs-title">span</span>></span>反转字符串<span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-tag"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"></<<span class="hljs-attribute">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-title"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>>button<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"></<span class="hljs-title">span</span>></span>
>     `<span class="hljs-tag"></<span class="hljs-title">pre</span>></span>
>     <span class="hljs-tag"></<span class="hljs-title">div</span>></span>
>     <span class="hljs-tag"><<span class="hljs-title">pre</span>></span>`<span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-tag"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<<span class="hljs-attribute">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-title"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>>script<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"javascript"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"></<span class="hljs-title">span</span>></span>
>     `<span class="hljs-tag"></<span class="hljs-title">pre</span>></span>
>     <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>new<span class="hljs-tag"></<span class="hljs-title">span</span>></span> Vue({
>       el: <span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>'#app'<span class="hljs-tag"></<span class="hljs-title">span</span>></span>,
>       data: {
>     <span class="hljs-tag"><<span class="hljs-title">pre</span>></span>`<span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-keyword"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>>message<span class="hljs-tag"></<span class="hljs-title">span</span>></span>: <span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-string"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>'Runoob!'<span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"></<span class="hljs-title">span</span>></span>
>     `<span class="hljs-tag"></<span class="hljs-title">pre</span>></span>
>       },
>       methods: {
>     <span class="hljs-tag"><<span class="hljs-title">pre</span>></span>`reverseMessage: <span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-function"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-keyword"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>>function<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-params"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>>()<span class="hljs-tag"></<span class="hljs-title">span</span>></span> <span class="hljs-tag"></<span class="hljs-title">span</span>></span>{
>       <span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-keyword"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>this<span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"></<span class="hljs-title">span</span>></span>.message = <span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-keyword"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>this<span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"></<span class="hljs-title">span</span>></span>.message.split(<span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-string"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>''<span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"></<span class="hljs-title">span</span>></span>).reverse().<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>join<span class="hljs-tag"></<span class="hljs-title">span</span>></span>(<span class="hljs-tag"><<span class="hljs-title">span</span> <<span class="hljs-attribute">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-keyword"</span>></span>class<span class="hljs-tag"></<span class="hljs-title">span</span>></span>=<span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>"hljs-string"<span class="hljs-tag"></<span class="hljs-title">span</span>></span>><span class="hljs-tag"><<span class="hljs-title">span</span> <span class="hljs-attribute">class</span>=<span class="hljs-value">"hljs-string"</span>></span>''<span class="hljs-tag"></<span class="hljs-title">span</span>></span><span class="hljs-tag"></<span class="hljs-title">span</span>></span>)
>     }</span>
> 
>   }
> })
> &lt;/script&gt;

## 过滤器

> 允许自定义过滤器，被用作一些常见的文本格式化。由"管道符"指示
> 
> 在两个大括号中 {{msg | capitalize}}
> 
> 在v-bind指令中 v-bind:id='rawId | formatId'
> 
> 过滤器函数接受表达式的值作为第一个参数
> 
> 例如字符串第一个字母转为大写
> 
> filters: {
>     capitalize: function (value) {
>       if (!value) return ''
>       value = value.toString()
>       return value.charAt(0).toUpperCase() + value.slice(1)
>     }
>   }
> 
> 可以串联和接受参数
> 
> {{ message | filterA('arg1', arg2) | filterB }}
> 
> message 是第一个参数，字符串 'arg1' 将传给过滤器作为第二个参数， arg2 表达式的值将被求值然后传给过滤器作为第三个参数。

## 缩写

1.v-bind

> 完整写法: &lt;a v-bind:href="url"&gt;&lt;/a&gt;
> 
> 缩写 &lt;a :href="url"&gt;&lt;/a&gt;

2.v-on

> 完整写法 &lt;a v-on:click="doSomething"&gt;&lt;/a&gt;
> 
> 缩写 &lt;a @click="doSomething"&gt;&lt;/a&gt;

## 新增属性和打开新页面

> 打开新页面 target
> 
> data:{url:'xx',target:'_blank'}
> 
> 给对象添加一个新属性 Vue.set
> 
> Vue.set(this.对象,'新增属性',属性值)