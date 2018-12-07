## 条件语句

1.v-if 

> 普通的直接类似 {{}} 在 new Vue({dataL{xx:true/false}})控制
> 
> 字符串模板中需要写条件块 eg.Handlebars模板 
> `{{#if ok}} <h1>Yes</h1> {{/if}}`

2.v-else 

> 给v-if添加一个else块

3.v-else-if 

> 用作v-if的else-if块。可以链式的多次使用

4.v-show 

> 根据条件展示元素

5.v-if和v-show的区别

> v-if 有更高的切换消耗 运行时条件不大 v-if较好
> 
> v-show有更高的初始渲染消耗 频繁切换v-show较好
> 
> v-if是动态添加，当值为false时，是完全移除该元素,即dom树中不存在钙元素
> 
> v-show仅是隐藏/显示,值为false时，dom树中有，若其原有样式设置了display:done则会导致其无法正常显示

## 循环语句

*   v-for> 需要以site in sites形式的特殊语法
> 
> > sites是源数据数组并且site是数据元素迭代的别名
> > 可以绑定数据到数组来渲染一个列表
> > 
> > > &lt;li v-for="site in sites"&gt;{{site.name}}&lt;/li&gt;
> > > 
> > > data:{sites:[{name:'TaoBao'},{},{}]}
> > 
> > 也可在模板中使用
> > 
> > > &lt;template v-for="site in sites"&gt;&lt;li&gt;{{site.name}}&lt;/li&gt;&lt;/template&gt;
> > 
> > 可以通过一个对象的属性来迭代数据
> > 
> > > v-for="value in object"  {{value}}
> > > 
> > > data:{object:{name:'xx',url:'xx',slogan:'xx'}}
> > 
> > 可以提供第二个的参数为键名
> > 
> > > v-for="(value,key) in object"
> > > 
> > > {{key}}:{{value}}
> > 
> > 第三个参数为索引
> > 
> > > v-for= "(value,key,index) in object"
> > > 
> > > {{index}}.{{key}}:{{value}}
> > 
> > 也可循环整数,数组 
> > 
> > > v-for='n in 10' {{n}}
> > > 
> > > v-for 'n in [1,3,5]' {{n}}

_在迭代属性输出前,会对属性进行升序排序即 object:{2:'xxx',1:'xx',0:'x'} 会自动排序_

> > 也可嵌套