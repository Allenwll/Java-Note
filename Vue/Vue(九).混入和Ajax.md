# 混入

> 混入 (mixins)定义了一部分可复用的方法或者计算属性
> 
> 混入对象可以包含任意组件选项。
> 
> 当组件使用混入对象时，所有混入对象的选项将被混入该组件本身的选项
> 
> > 定义一个混入对象
> > 
> > > var mixin={
> > > 
> > > > created:function(){this.startmixin()},
> > > > methods:{startmixin:function(xxxxx)}}
> > 
> > 实例化
> > 
> > > var Component=Vue.extend({mixins:[mixin]})
> > > 
> > > var component=new Component();

## 选项合并

> 当组件和混入对象含有同名选项时，这些选项将以恰当的方式混合
> 
> 数据对象在内部会进行浅合并 (一层属性深度)，在和组件的数据发生冲突时以组件数据优先
> 
> 如果 methods 选项中有相同的函数名，则 Vue 实例优先级会较高

## 全局混入

> 一旦使用全局混入对象，将会影响到 所有 之后创建的 Vue 实例
> 
> 谨慎使用全局混入对象，因为会影响到每个单独创建的 Vue 实例 (包括第三方模板)
> 
> > 为自定义的选项 'myOption' 注入一个处理器
> > 
> > > Vue.mixin({created:function(){
> > > 
> > > > var myOption=this.$options.myOption
> > > > 
> > > > if (myOption){console.log(myOption)}
> > > 
> > > }})
> > > 
> > > new Vue({myOption:'hello'})

# Ajax

> 要实现异步加载需要使用到 vue-resource 库
> 
> [https://cdn.staticfile.org/vue-resource/1.5.1/vue-resource.min.js](https://cdn.staticfile.org/vue-resource/1.5.1/vue-resource.min.js)

## Get请求

> this.$http.get('get.php',jsonData) 传递参数
> 
> > 第二个参数 jsonData 就是传到后端的数据
> > 
> > methods:{get:funtion(){//发送get请求}}
> > 
> > > this.$http.get('xxxx').then(function(res){document.write(res.body)},function(console.log(‘error’)));

## Post请求

> 发送数据到后端，需要第三个参数 {emulateJSON:true}
> 
> > 如果Web服务器无法处理编码为 application/json 的请求，你可以启用 emulateJSON 选项。
> > 
> > this.$http.post('xxx',{name:"xx",url:"xxx"},{emulateJSON:true}).then(function(res){...}

## 语法&amp;API

> 可以使用全局对象方式Vue.http 或者在一个Vue实例中使用this.$http发起HTTP请求
> 
> > 全局对象
> > 
> > > Vue.http.get().then(success,error);
> > > 
> > > Vue.http.post().then(success,error);
> > 
> > 实例使用
> > 
> > > this.$http.get().then(success,error);
> > > 
> > > this.$http.post().then(success,error);

### 提供了7种请求API REST风格

> > get(url,[options])
> > 
> > head(url,[options])
> > 
> > delete(url,[options])
> > 
> > jsonp(url,[options])
> > 
> > post(url,[body],[options])
> > 
> > put(url,[body],[options])
> > 
> > patch(url,[body],[options])
> 
> **options参数说明**
> 
> > url 类型:string
> > 
> > > 请求的目标URL
> > 
> > body 类型:Object,FormData,String
> > 
> > > 作为请求体发送的数据
> > 
> > headers 类型:Object
> > 
> > > 作为请求头部发送的头部对象
> > 
> > params 类型:Object
> > 
> > > 作为URL参数的参数对象
> > 
> > method 类型:string
> > 
> > > HTTP方法 (例如GET，POST，...)
> > 
> > timeout 类型:number
> > 
> > > 请求超时（单位：毫秒） (0表示永不超时)
> > 
> > before 类型:function(request)
> > 
> > > 在请求发送之前修改请求的回调函数
> > 
> > progress 类型:function(event)
> > 
> > > 用于处理上传进度的回调函数 ProgressEvent
> > 
> > credentials 类型:boolean
> > 
> > > 是否需要出示用于跨站点请求的凭据
> > 
> > emulateHTTP 类型:boolean
> > 
> > > 是否需要通过设置X-HTTP-Method-Override头部并且以传统POST方式发送PUT，PATCH和DELETE请求
> > 
> > emulateJSON 类型:boolean
> > 
> > > 设置请求体的类型为application/x-www-form-urlencoded

### 处理请求获取到的响应对象

*   属性
> url 类型:string
> 
> > 响应的URL源
> 
> body 类型:Object,Blob,String
> 
> > 响应体数据
> 
> headers 类型:Header
> 
> > 请求头部对象
> 
> ok 类型:boolean
> 
> > 当HTTP响应码为200到299之间的数值时该值为true
> 
> status 类型:number
> 
> > HTTP响应吗
> 
> statusText 类型:String
> 
> > HTTP响应状态

*   方法
> text() 类型:约定值
> 
> > 以字符串方式返回响应体
> 
> json() 类型:约定值
> 
> > 以格式化后的json对象方式返回响应体
> 
> blob() 类型:约定值
> 
> > 以二进制Blob对象方式返回响应体