# 自定义指令

> 除了默认设置的核心指令( v-model 和 v-show ), Vue 也允许注册自定义指令
> 
> 注册一个全局指令v-focus，在页面加载时，元素获得焦点
> 
> > &lt;input v-focus&gt;
> > 
> > 注册一个全局自定义指令v-focus
> > 
> > > Vue.directive('focus',{inserted:function(el){el.focus}})
> 
> 注册局部指令 
> 
> > new Vue({el='xx',directives:{focus:{inserted:function(el){el.focus()}}}})

### 钩子函数

> 指令定义函数提供了几个钩子函数
> 
> > bind:只调用一次,指令第一次绑定到元素时调用.可定义一个在绑定时执行一次的初始化动作
> > 
> > inserted:被绑定元素插入父节点时调用(父节点存在即可调用，不必存在于document中)
> > 
> > update:被绑定元素所在的模板更新时调用,而不论绑定值是否变化,通过比较更新前后的绑定值，可以忽略不必要的模板更新
> > 
> > componentUpdated:被绑定元素所在模板完成一次更新周期时调用
> > 
> > unbind:只调用一次，指令与元素解绑时调用

#### 钩子函数参数

> el:指令所绑定的元素，可以直接操作dom
> 
> binding:一个对象，包含一下属性:
> 
> > name:指令名，不包括v-前缀
> > 
> > value:指令的绑定值，例如v-my-directive='1+1' value值是2
> > 
> > oldValue:指令绑定的前一个值，仅在update和componentUpdated时可用
> > 
> > expression:绑定值的变量名或表达式 例如 上个例子 expression的值是'1+1'
> > 
> > arg:传给指令的参数 例如 v-my-directive:foo arg的值是foo
> > 
> > modifiers:一个包含修饰符的对象 例如 v-my-directive.foo.bar, 修饰符对象的值是{foo:true,bar:true}
> 
> vnode:vue编译生成的虚拟节点
> 
> oldVnode:上一个虚拟节点 仅在update和componentUpdated时可用
> 
> 格式：Vue.directive('xx',{bind:function(el,binding,vnode){binding.name.........}})
> 
> 有时不需要其他钩子函数，可以简写
> 
> > Vue.directive('runoob', function (el, binding) {// 设置指令的背景颜色el.style.backgroundColor = binding.value.color})
> 
> 可以接受所有合法的 JavaScript 表达式
> 
> > &lt;div v-runoob="{ color: 'green', text: '菜鸟教程!' }"&gt;&lt;/div&gt;
> > 
> > Vue.directive('runoob', function (el, binding) {// 简写方式设置文本及背景颜色
> > el.innerHTML = binding.value.text
> >     el.style.backgroundColor = binding.value.color
> > })

# 路由

> 允许我们通过不同的 URL 访问不同的内容。
> 
> 可以实现多视图的单页Web应用（single page web application，SPA）
> 
> > 需要载入 vue-router 库
> > 
> > 直接下载 / CDN [https://unpkg.com/vue-router/dist/vue-router.js](https://unpkg.com/vue-router/dist/vue-router.js)
> > 
> > 推荐使用淘宝镜像：cnpm install vue-router

### &lt;router-link&gt;

> 是一个组件，该组件用于设置一个导航链接,切换不同 HTML 内容。 to 属性为目标地址， 即要显示的内容
> 
> 1.定义（路由）组件。
> 
> 1.  定义路由
> 
> 2.  创建 router 实例，然后传 routes 配置
> 
> 4.创建和挂载根实例。  
> 
> > 手动挂载 $.mount() 类似于el
> > 
> > **点击过的导航链接都会加上样式 class ="router-link-exact-active router-link-active"**

#### to

> 当被点击后，内部会立刻把 to 的值传到 router.push()，所以这个值可以是一个字符串或者是描述目标位置的对象
> 
> > &lt;router-link :to="{ path: 'register', query: { plan: 'private' }}"&gt;Register&lt;/router-link&gt;

#### replace

> 点击时，会调用 router.replace() 而不是 router.push()，导航后不会留下 history 记录
> 
> > &lt;router-link :to="{ path: '/abc'}" replace&gt;&lt;/router-link&gt;

#### append

> 在当前 (相对) 路径前添加基路径
> 
> > 从/a导航到一个相对路径b 路径为/b，配置append后，则为/a/b
> > 
> > &lt;router-link :to="{ path: 'relative/path'}" append&gt;&lt;/router-link&gt;

#### tag

> &lt;router-link&gt; 渲染成某种标签，例如 &lt;li&gt;。使用tag指定何种标签，同样它还是会监听点击，触发导航
> 
> > &lt;router-link to="/foo" tag="li"&gt;foo&lt;/router-link&gt;
> > &lt;!-- 渲染结果 --&gt;&lt;li&gt;foo&lt;/li&gt;

#### active-class

> 设置链接激活时使用的 CSS 类名
> 
> > &lt;router-link v-bind:to = "{ path: '/route1'}" active-class = "_active"&gt;Router Link 1&lt;/router-link&gt;
> > 
> > > **这里 class 使用 active_class="_active"**

#### exact-active-class

> 配置当链接被精确匹配的时候应该激活的 class
> 
> > &lt;router-link v-bind:to = "{ path: '/route1'}" exact-active-class = "_active"&gt;Router Link 1&lt;/router-link&gt;

#### event

> 声明可以用来触发导航的事件。可以是一个字符串或是一个包含字符串的数组
> 
> > &lt;router-link v-bind:to = "{ path: '/route1'}" event = "mouseover"&gt;Router Link 1&lt;/router-link&gt;