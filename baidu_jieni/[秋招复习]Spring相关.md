## Spring相关

### 1.SpringIOC原理

**IOC**（控制反转），指的是Java类放弃对其他类的创建权，将其交由SpringIOC容器来控制，使得开发人员更加关注于业务逻辑的开发，类A需要类B时Spring会创建一个B的实例对象注入到A中供其使用。IOC的设计目的是为了降低面向对象编程中类与类之间的耦合度。

### 2.Spring中Bean的作用域

* **singleton:**唯一bean实例，Spring中默认都是单例的。
* **prototype：**每次请求都会创建一个新的bean实例。
* **request：**每一次Http请求都会创建一个新的bean实例，该实例只在当前Http request内有效。
* **session：**每一次Http请求都会创建一个新的bean实例，该bean仅在当前Http session内有效。
* **global-session：**全局session作用域，仅仅在基于portel的web应用中才有意义，Spring5已移除。

#### Spring中的单例Bean的线程安全问题

单例bean是存在线程安全问题的，主要体现在多线程操作同一对象时该对象的非静态成员变量的写操作上。

常见的解决办法：

* 在Bean中尽量不定义可变的成员变量（不太现实）
* 在类中定义一个ThreadLocal成员，将需要的成员变量保存至该ThreadLocal中。

### 3.Spring中Bean的生命周期

* Bean容器的初始化。
* 扫描配置文件，通过反射进行实例化。
* 如果设置了属性，通过set方法进行属性值的设置。
* 如果实现了BeanNameAware接口，调用setBeanName()方法传入一个name。
* 如果实现了BeanClassLoaderAware接口，调用setBeanClassLoader()方法传入一个类加载器的实例。
* 如果事先了BeanFactoryAware接口，调用setBeanFactory()方法传入一个BeanFactory实例。
* 如果实现了其他类似的*Aware接口，调用相应的方法。
* 如果加载该Bean的Spring容器有相关的BeanPostProcessor对象，执行postProcessorBeforeInitialization()方法。
* 如果Bean实现了InitializingBean接口，执行afterPropertiesSet()方法。
* 如果该Bean在配置文件中定义了init-method属性，调用指定的方法。
* 如果加载该Bean的Spring容器有相关的BeanPostProcessor对象，执行postProcessorAfterInitialization()方法。
* 销毁Bean时，如果实现了DisposableBean接口的话，执行destory()方法。
* 最后，如果配置文件中该Bean定义了destory-method属性，执行指定的方法。

### 4.Spring的事务隔离级别（结合数据库的事务隔离级别）

* **ISOLATION_DEFAULT:**

  使用数据库默认的隔离级别，MySQL就是REPEATABLE_READ。

* **ISOLATION_READ_UNCOMMITTED:**

  最低的隔离级别，允许读取尚未提交的事务的数据，可能导致脏读、幻读、不可重复读。

* **ISOLATION_READ_COMMITTED:**

  允许读取并发事务已经提交的数据，防止了脏读，但幻读和不可重复读仍然可能发生。

* **ISOLATION_REPEATABLE_READ:**

  保证对同一个字段的多次读取结果是一致的，可以阻止脏读、不可重复读，但幻读仍然可能发生。

* **ISOLATION_SERIALIZABLE:**

  最高的事务隔离级别，所有的事务串行处理，效率较低，但完全服从事务的ACID原则，可以防止脏读、幻读、不可重复读。

### 5.Spring的事务传播行为

分三种情况

**支持当前事务的话：**

* **PROPAGATION_REQUIRED** 如果当前存在事务，则加入该事务；如果当前没有事务，就创建一个事务。
* **PROPAGATION_SUPPORTS** 如果当前存在事务，则加入该事务；如果当前没有事务，就按非事务的方式运行。
* **PROPAGATION_MANDATORY** 如果当前存在事务，则加入该事务；否则抛出异常。（强制性）

**不支持当前事务：**

* **PROPAGATION_REQUIRES_NEW** 创建一个新的事务，如果当前存在事务，就把当前事务挂起。
* **PROPAGATION_NOT_SUPPORTED** 以非事务的方式运行，如果当前存在事务，就把当前事务挂起。
* **PROPAGATION_NEVER** 以非事务方式运行，如果当前存在事务，则抛出异常。

**其他：**

* **PROPAGATION_NESTED** 如果当前存在事务，创建一个事务作为当前事务的嵌套事务来运行，否则就创建一个新事务。

### 6.CGlib和JDK动态代理

**区别：**

jdk动态代理需要实现接口来完成反射。

cglib只需要一个非抽象的类即可对其增强，不需要实现接口，实现原理是生成该类的子类，所以不能增强final类。



### 7.Spring中事务失效的原因

1. 数据库不支持事务。MyiSAM
2. 没有交由Spring管理类。缺少@Service
3. **方法不是public的。**
4. **没有抛出异常。** catch了
5. **调用了另一个事务方法。** 可以注入一个本类。
6. 传播行为为**NOT_SUPPORTED**。
7. 异常类型错误，非**RuntimeExecption**。
8. 数据源没有配置事务管理器。

### 8.BeanFactory和FactoryBean的区别？

两者没什么明显的联系，就是容易混淆所以经常会考察到。

共同点：都是`org.springframework.beans.factory`包下的接口。

**区别：**

1. **BeanFactory**

   * 定义了Spring IOC容器的基本操作，并提供了IOC容器应遵守的基本接口，是Spring IOC所遵守的最底层和最基本的编程规范。
   * 综上，BeanFactory是一个接口，并不是IOC容器的实现，但Spring基于实现BeanFactory接口提供了多个IOC容器的具体实现，比如ApplicationContext、XmlBeanFactory，在Spring中，所有的Bean都是由BeanFactory的实现类管理的。

2. **FactoryBean**

   * FactoryBean是一个Bean，为Spring提供实例化Bean的一些基本逻辑，隐藏了实例化Bean的许多细节。用户自定义实例化Bean的逻辑时可实现此接口。

   * FactoryBean中定义了三个方法：

     ```java
     T getObject();		//返回由FactoryBean创建的实例，如果isSingleton()返回true，则该实例会放入Spring容器中的单例缓存池。
     boolean isSingleton();	//返回由FactoryBean创建的实例作用域是否为单例。
     Class<T> getObjectType();	//返回由FactoryBean创建的实例类型。
     ```

     