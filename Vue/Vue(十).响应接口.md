## 数据动态响应接口

> 不允许在已经创建的实例上动态添加新的根级响应式属性
> 
> 不能检测到对象属性的添加或删除
> 
> > 最好的方式就是在初始化实例前声明根级响应式属性，哪怕只是一个空值
> 
> 全局 Vue，Vue.set 和 Vue.delete 方法在运行过程中实现属性的添加或删除

### Vue.set

> 用于设置对象的属性，它可以解决 Vue 无法检测添加属性的限制
> 
> > Vue.set(target,key,value)
> > 
> > target 可以是对象或数组 key 可以是字符串或数字 value 任意

### Vue.delete

> 用于删除动态添加的属性
> 
> > Vue.delete( target, key )
> > 
> > target 可以是对象或数组  key 可以是字符串或数字