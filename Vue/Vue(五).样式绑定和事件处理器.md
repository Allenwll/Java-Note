## 样式绑定

> class 与 style 是 HTML 元素的属性，用于设置元素的样式，我们可以用 v-bind 来设置样式属性。
> 
> Vue.js v-bind 在处理 class 和 style 时， 专门增强了它。表达式的结果类型除了字符串之外，还可以是对象或数组
> 
> 将 isActive 设置为 true 显示了一个绿色的 div 块，如果设置为 false 则不显示

## 1.v-bind:class

> > v-bind:class="{ active: isActive }"&gt;
> > 
> > data:{isActive:true}
> 
> text-danger 类背景颜色覆盖了 active 类的背景色
> 
> > v-bind:class="{ active: isActive, 'text-danger': hasError }"
> > 
> > data:{isActive:true,hasError:true}
> 
> 也可以直接绑定数据里的一个对象
> 
> > &lt;div v-bind:class="classObject"&gt;&lt;/div&gt;
> > 
> > classObject:{active:true,'text-danger':true}
> 
> 也可以在这里绑定返回对象的计算属性
> 
> > computed: {classObject: function () {return {base: true,XX}}}
> 
> 也可传入数组
> 
> > v-bind:class="[activeClass,errorClass]"
> 
> 三元表达式
> 
> > [errorClass ,isActive ? activeClass : '']

## 2.v-bind:style

> 直接设置
> 
> > v-bind:style="{ color: activeColor, fontSize:fontSize + 'px' }
> 
> 绑定一个样式对象
> 
> > v-bind:style="styleObject"
> 
> 或者使用数组将多个样式对象应用到一个元素
> 
> > v-bind:style="[baseStyles, overridingStyles]"

## 事件处理器

### 1.事件监听可以使用v-on指令

> 通常情况下，我们需要使用一个方法来调用 JavaScript 方法。
> v-on 可以接收一个定义的方法来调用。
> 
> > v-on:click="greet"
> > 
> > methods:{greet:function(event){xxxx}}
> > 
> > 也可用JS直接调用方法 vm.greet()
> 
> 也可内联JS语句
> 
> > v-on:click="say('hi')"
> > 
> > methods: {say: function (message) {alert(message)}}

## 事件修饰符

> Vue.js 为 v-on 提供了事件修饰符来处理 DOM 事件细节，
> 
> > event.preventDefault()或event.stopPropagation()。
> 
> Vue.js通过由点(.)表示的指令后缀来调用修饰符。
> 
> > 1.阻止单击事件冒泡
> > 
> > > v-on:click.stop="doThis"
> > 
> > 2.提交事件不再重载页面 
> > 
> > > v-on:submit.prevent="onSubmit"
> > 
> > 3.修饰符可以串联
> > 
> > > v-on:click.stop.prevent="doThat"
> > 
> > 4.只有修饰符
> > 
> > > v-on:submit.prevent
> > 
> > 5.添加事件侦听器时使用事件捕获模式 
> > 
> > > v-on:click.capture="doThis"
> > 
> > 只当事件在该元素本身（而不是子元素）触发时触发回调
> > 
> > > v-on:click.self="doThat"
> > 
> > click 事件只能点击一次，2.1.4版本新增
> > 
> > > v-on:click.once="doThis"

## 按键修饰符

> Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：
> 
> > 只有在 keyCode 是 13 时调用 vm.submit()
> > 
> > > v-on:keyup.13="submit"
> > 
> > 别名 13=enter  缩写 @keyup.enter
> > 
> > 全部按键别名
> > 
> > > enter
> > > 
> > > .tab
> > > 
> > > .delete (捕获 "删除" 和 "退格" 键)
> > > 
> > > .esc
> > > 
> > > .space
> > > 
> > > .up
> > > 
> > > .down
> > > 
> > > .left
> > > 
> > > .right
> > > 
> > > .ctrl
> > > 
> > > .alt
> > > 
> > > .shift
> > > 
> > > .meta

### 实例

> > Alt+C
> > 
> > > &lt;input @keyup.alt.67="clear"&gt;
> > 
> > Ctrl+Click
> > 
> > > @click.ctrl="doSomething"