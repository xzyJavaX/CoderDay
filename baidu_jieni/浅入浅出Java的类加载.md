## 1.什么是类加载

虚拟机把class文件加载到内存，并进行验证、解析、初始化等一系列步骤生成虚拟机可用的Java类型的整个过程称为虚拟机的类加载。

## 2.Java的类加载器和类加载机制

### 2.1 类加载器

JVM 中内置了三个类加载器，除了`BootStrapClassLoader`，其他类加载器均由 Java 实现且全部继承自`Java.lang.ClassLoader`；

1. **`BootStrapClassLoader`（启动类加载器）**：最顶层的类加载器，由 C++ 实现，负责加载`%JAVA_HOME%/lib`目录下的 jar 包或类，或被`-Xbootclasspath`参数指定的路径中的所有类。
2. **`ExtensionClassLoader（拓展类加载器）：`**主要负责加载目录`%JRE_HOME%/lib/ext`目录下的jar包和类，或被`java.ext.dirs`系统变量指定的路径下的jar包。
3. **`AppClassLoader（应用程序类加载器）：`**面向用户的加载器，负责加载当前classpath下的所有jar包和类。

### 2.2 双亲委派模型

简单总结：**自底向上检查是否已被加载，自顶向下尝试加载类**。

虚拟机在执行类的加载时，会首先判断是否被加载过，没有的话会将加载请求委托给当前类加载器的**父类**的loadClass（）方法执行，因此所有的类加载请求都会传送到最顶层的类加载器**BootStrapClassLoade**r中，只有当父类加载器无法处理时才会由自己来加载，若父类加载器为 null ，则委托BootStrapClassLoader来处理。

例如：AppClassLoader的父类加载器为ExtClassLoader，后者的父类加载器为NULL。

**双亲委派模型的好处：**保证了Java程序的稳定运行，可以避免类的重复加载，（JVM不仅仅根据类名区分类，相同类名但不同类加载器加载的或被认为是不同的两个类），也保证了Java的核心API不会被覆盖（自己创建一个String类）。

**如何不使用双亲委派模型：**自定义类加载器，并重写`loadClass（）`方法。

### 2.3类加载器的设计模式

源于一道有些奇怪的阿里面试题：

![](https://raw.githubusercontent.com/qingshui3000/pic_bed/master/notes/20200710164344.png)

为此在中文搜索引擎上搜了一通，众说纷纭，有说模板方法模式，有说单例、工厂的，为此去google了一下，锁定到了一个词：`delegation model（委派模式）`，[来源点此]( "https://www.geeksforgeeks.org/classloader-in-java/") 。

以委派模式在百度搜索倒是发现不少关联到Java类加载的文章，但无论是委派模式还是双亲委派`parent delegation model`都不属于常见的23种设计模式，所以这个问题这么问显然是有点问题的，不过回答时引导到Java类加载的双亲委派模型就问题不大。

![](https://raw.githubusercontent.com/qingshui3000/pic_bed/master/notes/20200710165151.png)

## 3.拓展-Tomcat和OSGI的类加载s

### 3.1 Tomcat的类加载

Tomcat作为一个Java实现的web服务器因为其特殊的业务需求，并没有使用Java默认的类加载模型，我们一般讲：Tomcat破坏了Java类加载的双亲委派模式。

Tomcat作为web服务器，其主要有以下几点不同于传统Java应用的需求：

1. 同一个服务器下可能会有多个web应用，这些web应用之间依赖的Java类库需要**隔离**，因为不同应用可能会依赖同一个第三方类库的不同版本，所以不能使同一个第三方类库在服务器中只有一份（一个版本）。
2. 同一个服务器下可能会有多个web应用，这些web应用之间依赖的Java类库需要**共享**，这与上一条并不矛盾，因为不同的应用也可能会有许多版本相同的第三方类库，如果每个应用下都需要一份的话就很造成很大的资源浪费。
3. Tomcat本身也是Java实现的，自身也需要依赖于一些Java类库，那么为了保证服务器能够安全稳定的运行，其自身依赖的类库是需要和应用程序的类库完全独立的。
4. 对于jsp格式的网页，服务器需要实现热部署，即修改jsp文件后只需要重新编译该jsp文件即可实现页面的更新，无需重启。

由于上述几个原因，单一的一个classpath以及默认的类加载器是无法满足这些需求的。所以Tomcat中有额外的一些classpath，例如加载jsp文件的路径位于/WEB-INF/；此外，Tomcat还实现了几个特有的类加载器用于加载jsp文件、应用程序等。

双亲委派模式的特点就是 **自底向上检查是否已被加载，自顶向下进行类加载** ，Tomcat中的类加载起在接到类加载请求时，检查是否已被加载后不会向上委派父类加载器加载，而是自己先尝试加载，因此说Tomcat在类加载的实现上破坏了双亲委派模式。

![](https://raw.githubusercontent.com/qingshui3000/pic_bed/master/notes/20200710153251.png)

### 3.2 关于OSGI

OSGI是一个Java的动态模块化规范，在类加载方面，其主要的特点就是 **灵活**，OSGI的类加载器只有规则而没有强制和固定的委派关系。这使得OSGI标准下的类加载模型不同于Java默认的双亲委派模型和Tomcat等web服务器的类加载器之间的有明显上下级的树形模型，而是一种平级的模型。这个类似网状的模型更为复杂、具体运行时才能确定。