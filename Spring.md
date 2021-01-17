## Spring

> Spring对bean的管理细节
> 	1.创建bean的三种方式
> 	2.bean对象的作用范围
> 	3.bean对象的生命周期

```markdown
创建bean的三种方式

 第一种：使用默认构造函数创建
 	在Spring的配置文件中使用bean标签，配以id和class属性之后，且没有其他属性和标签时，采用的就是默认构造函数创建bean对象，此时如果类中没有默认构造函数，则对象无法创建 
 	
 第二种：使用普通工厂中的方法创建对象（使用某个类中的方法创建对象，并存入Spring容器）
 	
 第三种：使用工厂中的静态方法创建对象（使用某个类中的静态方法创建对象，并存入Spring容器）
 	
```

> Spring中的依赖注入
>  依赖注入：
>  		Dependency Injection
>  IoC的作用：
>  	降低程序间的耦合（依赖关系）
>  	依赖关系的管理：
>  		以后都交给Spring来维护
>  	在当前类需要用到其他类的对象，由Spring为我们提供，我们只需要在配置文件中说明依赖关系的维护
>  		就称之为依赖注入
>  	依赖注入：
>  		能注入的数据：有三类
>  			基本数据类型和String
>  			其他bean类型（在配置文件中或者注解配置过的bean）
>  			复杂类型/集合类型
>  		注入的方式：有三种
>  			1.使用构造函数提供
>  			2.使用set方法提供
>  			3.使用注解提供

~~~ markdown
构造器注入
	使用的标签：constructor-arg
	标签出现的位置，bean标签的内部
	标签中的位置
		type：用于指定要注入的数据的数据类型，该数据类型也是构造函数中某个或某些参数的类型
		index：用于指定要注入的数据给构造函数中指定索引位置的参数复制。索引的位置是从0开始的
		name：用于指定给构造函数中指定名称的参数赋值
		=======以上三个用于指定给构造函数中哪个参数赋值=======
		value：用于提供基本类型和String类型的数据
		ref：用于指定其他的bean类型数据，它指的就是在spring的IoC核心容器中出现过的bean对象
		
	优势：
		在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功。
	弊端：
		改变了bean对象的实例化方式，使我们在创建对象时，如果用不到这些数据，也必须提供。
~~~

~~~markdown
Set方法注入
    涉及的标签：property
    出现的位置：bean标签的内部
    标签的属性：
        name：用于指定注入时所调用的set方法名称
        value：用于提供基本类型和String类型的数据
        ref：用于指定其他的bean类型数据，它指的就是在spring的IoC核心容器中出现过的bean对象
~~~





~~~markdown
动态代理：
特点：字节码随用随创建，随用随加载
作用：不修改源码的基础上对方法增强
分类：
	基于接口的动态代理
	基于子类的动态代理
1.基于接口的动态代理：
	涉及的类：Proxy
	提供者：JDK官方
如何创建代理对象：
	使用Proxy类中的newProxyInstance方法
创建代理对象的要求：
	被代理类最少实现一个接口，如果没有则不能使用
newProxyInstance方法的参数：
	ClassLoader：类加载器
		它是用于加载代理对象字节码的，和被代理对象使用相同的类加载器，固定写法
	Class[]：字节码数组
		它是用于让代理对象和被代理对象有相同方法，固定写法
	InvocationHandler：用于提供增强的代码
		它是让我们写如何代理，我们一般都是些一个该接口的实现类，通常情况下都是匿名类，但不是必须的。
		
2.基于子类的
~~~

==高亮显示==  



### Spring由哪些模块组成？

Spring 总共大约有 20 个模块， 由 1300 多个不同的文件构成。 而这些组件被分别整合在核心容器（Core Container） 、 AOP（Aspect Oriented Programming）和设备支持（Instrmentation） 、数据访问与集成（Data Access/Integeration） 、 Web、 消息（Messaging） 、 Test等 6 个模块中。 以下是 Spring 5 的模块结构图：

![image-20200524210416963](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200524210416963.png)



- spring core：提供了框架的基本组成部分，包括控制反转（Inversion of Control，IOC）和依赖注入（Dependency Injection，DI）功能。
- spring beans：提供了BeanFactory，是工厂模式的一个经典实现，Spring将管理对象称为Bean。
- spring context：构建于 core 封装包基础上的 context 封装包，提供了一种框架式的对象访问方法。
- spring jdbc：提供了一个JDBC的抽象层，消除了烦琐的JDBC编码和数据库厂商特有的错误代码解析， 用于简化JDBC。
- spring aop：提供了面向切面的编程实现，让你可以自定义拦截器、切点等。
- spring Web：提供了针对 Web 开发的集成特性，例如文件上传，利用 servlet listeners 进行 ioc 容器初始化和针对 Web 的 ApplicationContext。
- spring test：主要为测试提供支持的，支持使用JUnit或TestNG对Spring组件进行单元测试和集成测试。

### Spring 框架中都用到了哪些设计模式？

1. 工厂模式：BeanFactory就是简单工厂模式的体现，用来创建对象的实例；
2. 单例模式：Bean默认为单例模式。
3. 代理模式：Spring的AOP功能用到了JDK的动态代理和CGLIB字节码生成技术；
4. 模板方法：用来解决代码重复的问题。比如. RestTemplate, JmsTemplate, JpaTemplate。
5. 观察者模式：定义对象键一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知被制动更新，如Spring中listener的实现–ApplicationListener。

### Spring控制反转(IOC)

**什么是Spring IOC 容器？**
控制反转即IoC (Inversion of Control)，它把传统上由程序代码直接操控的对象的调用权交给容器，通过容器来实现对象组件的装配和管理。所谓的“控制反转”概念就是对组件对象控制权的转移，从程序代码本身转移到了外部容器。

Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

### Spring IoC 的实现机制

Spring 中的 IoC 的实现原理就是工厂模式加反射机制。

~~~java
interface Fruit {
   public abstract void eat();
 }

class Apple implements Fruit {
    public void eat(){
        System.out.println("Apple");
    }
}

class Orange implements Fruit {
    public void eat(){
        System.out.println("Orange");
    }
}

class Factory {
    public static Fruit getInstance(String ClassName) {
        Fruit f=null;
        try {
            f=(Fruit)Class.forName(ClassName).newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return f;
    }
}

class Client {
    public static void main(String[] a) {
        Fruit f=Factory.getInstance("io.github.dunwu.spring.Apple");
        if(f!=null){
            f.eat();
        }
    }
}

~~~

### Spring 的 IoC支持哪些功能

**Spring 的 IoC 设计支持以下功能**：

- 依赖注入
- 依赖检查
- 自动装配
- 支持集合
- 指定初始化方法和销毁方法
- 支持回调某些方法（但是需要实现 Spring 接口，略有侵入）

其中，最重要的就是依赖注入，从 XML 的配置上说，即 ref 标签。对应 Spring RuntimeBeanReference 对象。

对于 IoC 来说，最重要的就是容器。容器管理着 Bean 的生命周期，控制着 Bean 的依赖注入。

### BeanFactory 和 ApplicationContext有什么区别？

BeanFactory和ApplicationContext是Spring的两大核心接口，都可以当做Spring的容器。其中ApplicationContext是BeanFactory的子接口。

**依赖关系**

BeanFactory：是Spring里面最底层的接口，包含了各种Bean的定义，读取bean配置文档，管理bean的加载、实例化，控制bean的生命周期，维护bean之间的依赖关系。

ApplicationContext接口作为BeanFactory的派生，除了提供BeanFactory所具有的功能外，还提供了更完整的框架功能：

- 继承MessageSource，因此支持国际化。

- 统一的资源文件访问方式。

- 提供在监听器中注册bean的事件。

- 同时加载多个配置文件。

- 载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层。

**加载方式**

BeanFactroy:writing_hand:使用才加载

​		:point_right:采用的是延迟加载形式来注入Bean的，即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化。这样，我们就不能发现一些存在的Spring的配置问题。如果Bean的某一个属性没有注入，BeanFacotry加载后，直至第一次使用调用getBean方法才会抛出异常。

ApplicationContext，它是在容器启动时，一次性创建了所有的Bean。这样，在容器启动时，我们就可以发现Spring中存在的配置错误，这样有利于检查所依赖属性是否注入。 ApplicationContext启动后预载入所有的单实例Bean，通过预载入单实例bean ,确保当你需要的时候，你就不用等待，因为它们已经创建好了。

相对于基本的BeanFactory，ApplicationContext 唯一的不足是占用内存空间。当应用程序配置Bean较多时，程序启动较慢。

创建方式

BeanFactory通常以编程的方式被创建，ApplicationContext还能以声明的方式创建，如使用ContextLoader。

注册方式

BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，但两者之间的区别是：BeanFactory需要手动注册，而ApplicationContext则是自动注册。

### ApplicationContext通常的实现是什么？

**FileSystemXmlApplicationContext** ：此容器从一个XML文件中加载beans的定义，XML Bean 配置文件的全路径名必须提供给它的构造函数。

**ClassPathXmlApplicationContext**：此容器也从一个XML文件中加载beans的定义，这里，你需要正确设置classpath因为这个容器将在classpath里找bean配置。

**WebXmlApplicationContext**：此容器加载一个XML文件，此文件定义了一个WEB应用的所有bean。

### 怎样开启注解装配？:question:

注解装配在默认情况下是不开启的，为了使用注解装配，我们必须在Spring配置文件中配置 `<context:annotation-config/>`元素。

### @Component, @Controller, @Repository, @Service 有何区别？

**@Component**：这将 java 类标记为 bean。它是任何 Spring 管理组件的通用构造型。spring 的组件扫描机制现在可以将其拾取并将其拉入应用程序环境中。

**@Controller**：这将一个类标记为 Spring Web MVC 控制器。标有它的 Bean 会自动导入到 IoC 容器中。

**@Service**：此注解是组件注解的特化。它不会对 @Component 注解提供任何其他行为。您可以在服务层类中使用 @Service 而不是 @Component，因为它以更好的方式指定了意图。

**@Repository**：这个注解是具有类似用途和功能的 @Component 注解的特化。它为 DAO 提供了额外的好处。它将 DAO 导入 IoC 容器，并使未经检查的异常有资格转换为 Spring DataAccessException。

### @Required 注解有什么作用

这个注解表明bean的属性必须在配置的时候设置，通过一个bean定义的显式的属性值或通过自动装配，若@Required注解的bean属性未被设置，容器将抛出BeanInitializationException。示例：