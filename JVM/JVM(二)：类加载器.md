# 类加载器

`ClassLoader loader=Thread.currentThread().getContextClassLoader()`

**类层次**

> 启动类加载器Bootstrap ClassLoader---&gt;扩展类加载器ExtClassLoader--&gt;应用类加载器AppClassLoader--&gt;自定义类加载器User ClassLoader

_父类加载器不是通过继承实现，而是采用组合实现_

A.Java虚拟机：

> > 启动类加载器 
> > 
> > > C++实现(仅限于Hotspot)
> > 
> > 其他类加载器 
> > 
> > > 由Java语言实现，独立于虚拟机之外，并且全部继承自抽象类`java.lang.ClassLoader`，这些类需要启动类加载器加载到内存之后才能加载其他的类

B.Java开发人员: 

1.启动类加载器:`Boostrap ClassLoader`

> > 负责加载存在`JDK\jre\lib`下,或被`-Xbootclasspath`参数指定的路径中的，并且能被虚拟机识别的类库(rt.jar,所有java.开头的类均被BoostrapClassLoader加载). 启动类加载器是无法被Java程序直接引用的

2.扩展类加载器:`Extension ClassLoader`

> > 该加载器由`sum.misc.Lancher$ExtClassLoader`实现，负责加载`JDK\jre\lib\ext`或由`java.ext.dirs`系统变量指定的路径下的所有类库(javax.开头的),开发者可以直接使用

3.应用程序类加载器：`Application ClassLoader`

> > 该加载器由`sun.misc.Launcher$AppClassLoader`实现.负责加载用户类路径(ClassPath)所指定的类,开发者可以直接使用 若未自定义类加载器，则一般用的它

**自定义加载器** 

> > 因为JVM自带的ClassLoader只能从本地文件系统加载标准的Java Class文件，因此若有自己的类加载器可以:
> > 1.在执行非置信代码之前，自动验证数字签名
> > 2.动态创建符合用户特定需要的定制化构建类
> > 3.从特定的场所所取得Java Class 例数据库和网络中

**JVM类加载机制**

1.全盘负责

> > 当一个类加载器负责加载某个Class时，该Class所依赖的和引用的其他Class也将由该类加载器负责载入，除非显示使用另外一个类加载器来载入

2.父类委托

> > 先让父类加载器试图加载该类，只有在父类加载器无法加载该类时才尝试从自己的类路径中加载该类

3.缓存机制

> > 缓存机制将会保证所有加载过的Class都会被缓存，当程序中需要使用某个Class时，类加载器先从缓存区寻找该Class，只有缓存区不存在，系统才会读取该类对应的二进制数据，并将其转换成Class对象，存入缓存区。这就是为什么修改了Class后，必须重启JVM，程序的修改才会生效

**类加载方式**

1、命令行启动应用时候由JVM初始化加载

2、通过Class.forName()方法动态加载

3、通过ClassLoader.loadClass()方法动态加载

**Class.forName()和ClassLoader.loadClass()区别**

> > Class.forName()：将类的.class文件加载到jvm中之外，还会对类进行解释，执行类中的static块；
> > 
> > ClassLoader.loadClass()：只干一件事情，就是将.class文件加载到jvm中，不会执行static中的内容,只有在newInstance才会去执行static块。
> > 
> > Class.forName(name, initialize, loader)带参函数也可控制是否加载static块。并且只有调用了newInstance()方法采用调用构造函数，创建类的对象 。

**双亲委派机制:**
如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把请求委托给父加载器去完成，依次向上，因此，所有的类加载请求最终都应该被传递到顶层的启动类加载器中，只有当父加载器在它的搜索范围中没有找到所需的类时，即无法完成该加载，子加载器才会尝试自己去加载该类

> > 1、当AppClassLoader加载一个class时，它首先不会自己去尝试加载这个类，而是把类加载请求委派给父类加载器ExtClassLoader去完成。
> > 
> > 2、当ExtClassLoader加载一个class时，它首先也不会自己去尝试加载这个类，而是把类加载请求委派给BootStrapClassLoader```去完成。
> > 
> > 3、如果BootStrapClassLoader加载失败（例如在$JAVA_HOME/jre/lib里未查找到该class），会使用ExtClassLoader来尝试加载；
> > 
> > 4、若ExtClassLoader也加载失败，则会使用AppClassLoader来加载，如果AppClassLoader也加载失败，则会报出异常ClassNotFoundException。

_系统类防止内存中出现多份同样的字节码
保证Java程序安全稳定运行_

**自定义类加载器**

> > 应用是通过网络来传输 Java类的字节码，为保证安全性，这些字节码经过了加密处理，这时系统类加载器就无法对其进行加载，这样则需要自定义类加载器来实现。自定义类加载器一般都是继承自ClassLoader类，只需要重写 findClass 方法即可

注意点:

1、这里传递的文件名需要是类的全限定性名称

2、最好不要重写loadClass方法，因为这样容易破坏双亲委托模式 

3、这类Test 类本身可以被 AppClassLoader类加载，因此我们不能把com/paddx/test/classloading/Test.class放在类路径下。否则，由于双亲委托机制的存在，会直接导致该类由AppClassLoader加载，而不会通过我们自定义类加载器来加载。