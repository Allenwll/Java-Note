## 过渡

> 在插入、更新或者移除 DOM 时，提供多种不同方式的应用过渡效果
> 
> 提供了内置的过渡封装组件，该组件用于包裹要实现过渡效果的组件

### 语法格式

> &lt;transition name = "nameoftransition"&gt;&lt;/transition&gt;

### 过渡其实就是一个淡入淡出的效果

> 元素显示与隐藏的过渡中，提供了6个class 来切换:
> 
> > *   v-enter：进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除
> > 
> > *   v-enter-active：进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数
> > 
> > *   v-enter-to: 进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter 被移除)，在过渡/动画完成之后移除
> > 
> > *   v-leave: 离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除
> > 
> > *   v-leave-active：离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数
> > 
> > *   v-leave-to: 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除
> 
> 使用了&lt;transition name="my-transition"&gt;，那么 v-enter 会替换为 my-transition-enter。
> 
> v-enter-active 和 v-leave-active 可以控制进入/离开过渡的不同的缓和曲线

## 动画

> CSS 动画用法类似 CSS 过渡，但是在动画中 v-enter 类名在节点插入 DOM 后不会立即删除而是在 animationend 事件触发时删除。
> 
> 自定义过渡的类名优先级高于普通的类名，这样就能很好的与第三方（如：animate.css）的动画库结合使用
> 
> > &lt;transition
> >     name="custom-classes-transition"
> >     enter-active-class="animated tada"
> >     leave-active-class="animated bounceOutRight"&gt;

## 同时使用过渡和动画

> 为了知道过渡的完成，必须设置相应的事件监听器。
> 
> 它可以是 transitionend 或 animationend
> 
> > 这取决于给元素应用的 CSS 规则
> 
> 如果你使用其中任何一种，Vue 能自动识别类型并设置监听
> 
> 在一些场景中，你需要给同一个元素同时设置两种过渡动
> 
> > 比如 animation 很快的被触发并完成了，而 transition 效果还没结束
> > 
> > 需要使用 type 特性并设置 animation 或 transition 来明确声明你需要 Vue 监听的类型

### 显性的过渡持续时间

> 可以自动得出过渡效果的完成时机。默认情况下，Vue 会等待其在过渡效果的根元素的第一个 transitionend 或 animationend 事件
> 
> 用 &lt;transition&gt; 组件上的 duration 属性定制一个显性的过渡持续时间 (以毫秒计)：
> 
> > &lt;transition :duration="1000"&gt;...&lt;/transition&gt;
> 
> 定制进入和移出的持续时间
> 
> > &lt;transition :duration="{ enter: 500, leave: 800 }"&gt;...&lt;/transition&gt;
> 
> 可以在属性中声明 JavaScript 钩子:
> 
> > 当只用 JavaScript 过渡的时候，在 enter 和 leave 中必须使用 done 进行回调。否则，它们将被同步调用，过渡会立即完成
> > 
> > 推荐对于仅使用 JavaScript 过渡的元素添加 v-bind:css="false"，Vue 会跳过 CSS 的检测。这也可以避免过渡过程中 CSS 的影响
> > 
> > > v-on:enter="enter"
> > > 
> > > enter: function (el, done) {// ...done()},

## 初始渲染的过渡

> 通过 appear 特性设置节点在初始渲染的过渡
> 
> > &lt;transition appear&gt;
> > 
> > 可以自定义 CSS 类名
> > 
> > > appear-class="custom-appear-class"
> > 
> > 自定义 JavaScript 钩子：
> > 
> > > appear v-on:before-appear="customBeforeAppearHook"

## 多个元素的过渡

> 需要注意的是当有相同标签名的元素切换时，需要通过 key 特性设置唯一的值来标记以让 Vue 区分它们，否则 Vue 为了效率只会替换相同标签内部的内容。
> 
> > &lt;button v-if="isEditing" key="save"&gt;
> >     Save
> >   &lt;/button&gt;
> >   &lt;button v-else key="edit"&gt;
> >     Edit
> >   &lt;/button&gt;
> 
> 也可以通过给同一个元素的 key 特性设置不同的状态来代替 v-if 和 v-else
> 
> > &lt;button v-bind:key="isEditing"&gt;
> >     {{ isEditing ? 'Save' : 'Edit' }}
> >   &lt;/button&gt;