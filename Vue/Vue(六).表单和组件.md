# 表单

> 使用v-model 双向绑定

*   1.输入框

    > 会根据控件类型自动选取正确的方法来更新元素

*   2.复选框

    > 如果是一个为逻辑值，如果是多个则绑定到同一个数组

*   3.单选按钮

    > 同上

*   4.select列表

    > 同上

### 修饰符

> .lazy
> 在默认情况下， v-model 在 input 事件中同步输入框的值与数据
> 
> > 在 "change" 而不是 "input" 事件中更新
> > 
> > > v-model.lazy="msg"
> 
> .number
> 
> > 自动将用户的输入值转为 Number 类型（如果原值的转换结果为 NaN 则返回原值）
> > 
> > > v-model.number="age" type="number"

.trim

> > 自动过滤用户输入的首尾空格
> > 
> > > v-model.trim="msg"

### 全选与取消全选

> > data: { checked: false,checkedNames: [],checkedArr: ["Runoob", "Taobao", "Google"]},
> > 
> > methods: {changeAllChecked: function() {if (this.checked) {this.checkedNames = this.checkedArr} else { this.checkedNames = []}}},
> > 
> > watch: { "checkedNames": function() {if (this.checkedNames.length == this.checkedArr.length) {
> > this.checked = true} else {this.checked = false
> > }}}

# 组件

> 组件可以扩展 HTML 元素，封装可重用的代码
> 
> 组件系统让我们可以用独立可复用的小组件来构建大型应用，几乎任意类型的应用的界面都可以抽象为一个组件树
> 
> > 注册一个全局组件语法格式:
> > 
> > > Vue.component(tagName, options)
> > > 
> > > tagName 为组件名，options 为配置选项。
> > 
> > 注册后，我们可以使用以下方式来调用组件
> > 
> > > &lt;tagName&gt;&lt;/tagName&gt;

## 全局组件

> 所有实例都可使用
> 
> > &lt;div id="app"&gt; //自定义组件&lt;runoob&gt;&lt;/runoob&gt;&lt;/div&gt;
> > 
> > Vue.component('runoob'.{template:'&lt;h1&gt;xxx&lt;/h1&gt;'}) //注册
> > 
> > //创建根实例 new Vue({el:'#app'})

## 局部组件

> 当前实例使用
> 
> > 定义 &lt;div&gt;&lt;runoob&gt;&lt;/runoob&gt;&lt;/div&gt;
> > 
> > 注册var child={template:'&lt;h1&gt;xxx&lt;/h1&gt;'}
> > 
> > 创建根目录 new Vue({el:'#app',components:{'runoob':child}})

## Prop

> 是父组件用来传递数据的一个自定义属性
> 
> 父组件的数据需要通过props把数据传给子组件,子组件需要显式地用props选项声明prop
> 
> > 定义 &lt;child msg='hello!'&gt;&lt;/child&gt;
> > 
> > 注册 Vue.component('child',{ props:['msg'],template:‘&lt;span&gt;{{msg}}&lt;/span&gt;'})
> > 
> > 创建根实例

## 动态Prop

> 类似于用 v-bind 绑定 HTML 特性到一个表达式
> 
> 也可以用 v-bind 动态绑定 props 的值到父组件的数据中
> 
> 每当父组件的数据变化时，该变化也会传导给子组件
> 
> > 定义 &lt;input v-model='parentMsg'&gt; &lt;child v-bind:msg='parentMsg'&gt;&lt;/child&gt;
> > 
> > 注册 同上
> > 
> > //创建根实例  data:{parentMsg:'xxx'}

## Prop验证

> 组件可以为 props 指定验证要求。
> 
> prop 是一个对象而不是字符串数组时，它包含验证要求：
> 
> > Vue.component('example',{ porps:{.......}})
> > 
> > 基础类型检测 null意思是任何类型都可以
> > 
> > > propA:Number
> > 
> > 多种类型
> > 
> > > propB:[String,Number]
> > 
> > 必传且是字符串
> > 
> > > propC:{type:String,required:true}
> > 
> > 数字，有默认值
> > 
> > > propD:{type:Number,default:100}
> > 
> > 数组、对象的默认值应当由一个工厂函数返回
> > 
> > > propE:{type:Object,default:function(){return {msg:'hello'}}}
> > 
> > 自定义验证函数
> > 
> > > propF:{validator:function(value){return value&gt;10}}
> > 
> > type：
> > 
> > > String、Number、Boolean、Function、Object、Array
> > > 
> > > 也可以是一个自定义构造器，使用instanceof检测

## 自定义事件

> prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来
> 
> 如果子组件要把数据传递回去，就需要使用自定义事件
> 
> > 可以使用v-on绑定自定义事件，每个实例都实现了事件接口
> > 
> > 使用$on(eventName)监听事件
> > 
> > 使用$emit(eventName)触发事件
> > 
> > 如果你想在某个组件的根元素上监听一个原生事件。可以使用 .native 修饰 v-on
> > 
> > > &lt;my-component v-on:click.native="doTheThing"&gt;&lt;/my-component&gt;