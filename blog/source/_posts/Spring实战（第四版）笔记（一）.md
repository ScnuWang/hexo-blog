---
title: Spring实战（第四版）笔记（一）
date: 2017-10-18 17:02:19
tags: Spring
categories: 读书笔记
---
- 依赖注入（DI）---> 保持松散耦合有助于应用对象之间的解耦
> 按照传统的做法，每个类要负责管理与这个类相关的对象的引用，这就会导致代码高度的耦合以及难以测试。耦合必须的，如果一点耦合都没有，那也是毫无意义的，但是应该小心管理。
<!-- more -->
- 创建应用组件之间协作的行为通常称为装配（wiring）
>Spring有多种装配bean的方式:基于XML文件配置,基于Java注解的方式配置，自动装配  
Spring通过应用上下文（Application Context）装载bean的定义并把它
们组装起来。  
使用XML文件进行配置的，选择ClassPathXmlApplicationContext为应用上下文相对是比较合适的。
使用Java注解进行配置的，选择AnnotationConfigApplicationContext为应用上下文相对比较合适。注意：使用AnnotationConfigApplicationContext时，

- 面向切面编程--->实现横切关注点与它们所影响的对象之间的解耦（aspect-oriented programming， AOP）允许你把遍布应用各处的功能分离出来形成可重用的组件。面向切面编程往往被定义为促使软件系统实现关注点的分离一项技
术。系统由许多不同的组件组成，每一个组件各负责一块特定功能。除了实现自身核心的功能之外，这些组件还经常承担着额外的职责。诸如日志、事务管理和安全这样的系统服务经常融入到自身具有核心业务逻辑的组件中去，这些系统服务通常被称为横切关注点，因为它们会跨越系统的多个组件。

- 使用注解配置配置切面
> 1.新建一个普通类，使用@Aspect注解到类级别，类中包含切面的基本操作信息.  
2 新建一个配置类配置并启动刚刚新建的切面类，使用@Configuration和@EnableAspectJAutoProxy注解 
Spring的AspectJ自动代理仅仅使用@AspectJ作为创建切面的指导，切面依然是基于代理的

- 容器是Spring框架的核心
> Spring容器使用DI管理构成应用的组件，它会创建相互协作的组件之间的关联。Spring容器并不是只有一个。Spring自带了多个容器实现，可以归为两种不同的类型。bean工厂（由org.springframework. beans.factory.eanFactory接口定义）是最简单的容器，提供基本的DI支持。应用上下文（由org.springframework.context.ApplicationContext接口定义）基于BeanFactory构建，并提供应用框架级别的服务，例如从属性文件解析文本信息以及发布应用事件给感兴趣的事件监听者。虽然我们可以在bean工厂和应用上下文之间任选一种，但bean工厂对大多数应用来说往往太低级了，因此，应用上下文要比bean工厂更受欢迎。

- bean的生命周期
> 在传统的Java应用中， bean的生命周期很简单。使用Java关键字new进行bean实例化，然后该bean就可以使用了。一旦该bean不再被使用，则由Java自动进行垃圾回收。相对而言，Spring的bean周期相对复杂很多。

- bean装载到Spring应用上下文中的一个典型的生命周期过程。

- bean命名
> @Component注解默认的bean的ID为首字母小写的类名，也可以在注解中直接指定bean的ID如：@Component("person"),也可以使用@Named注解来为bean设置ID，@Named("person")。Spring支持将@Named作为@Component注解的替代方案。两者之间有一些细微的差异，但是在大多数场景中，它们是可以互相替换的。但是推荐使用@Component注解  
@ComponentScan(basePackages = "cn.geekview") //默认会扫描与配置类相同的包,basePackages返回的是一个字符串数组，所以可以同时设置多个包路径，也可以使用包中所包含的类或接口代替

- 自动装配bean
> @Autowired注解不仅能够用在构造器上,@Autowired注解可以用在类的任何方法上。如果没有匹配的bean，那么在应用上下文创建的时候，Spring会抛出一个异常。为了避免异常的出现，你可以将@Autowired的required属性设置为false  
@Inject和@Autowired，差不多，大多时候可以替换，推荐使用@Autowired  
带有@Bean注解的方法可以采用任何必要的Java功能来产生bean实例。

- XML装配的缺陷
> 我
们将bean的类型以字符串的形式设置在了class属性中。谁能保证设
置给class属性的值是真正的类呢？ Spring的XML配置并不能从编译
期的类型检查中受益。即便它所引用的是实际的类型，如果你重命名
了类，会发生什么呢？--->可以通过借助IDE检查XML的合法性使用能够感知Spring功能的IDE来检验

- 导入和混合配置
> 使用@Import注解导入其他配置类
@ImportResource注解在JavaConfig中引入XML配置文件

- 激活profile
> Spring在确定哪个profile处于激活状态时，需要依赖两个独立的属性：spring.profiles.active和spring.profiles.default。  
有多种方式来设置这两个属性：
作为DispatcherServlet的初始化参数；
作为Web应用的上下文参数；
作为JNDI条目；
作为环境变量；
作为JVM的系统属性；
在集成测试类上，使用@ActiveProfiles注解设置。  
profile使用的都是复数形式。这意味着你可以同时激活多个profile，这可以通过列出多个profile名称，并以逗号分隔来实现。  
@ActiveProfiles注解指定要激活的profile

- 条件化的bean
> @Conditional注解，它可以用到带有@Bean注解的方法上。设置给@Conditional的类可以是任意实现了Condition接口的类型。可以看出来，这个接口实现起来很简单直接，只需提供matches()方法的实现即可。如果matches()方法返回true，那么就会创建带有@Conditional注解的bean。如果matches()方法返回false，将不会创建这些bean。

- 处理自动装配的歧义性
> 仅有一个bean匹配所需的结果时，自动装配才是有效的。如果
不仅有一个bean能够匹配结果的话，这种歧义性会阻碍Spring自动装
配属性、构造器参数或方法参数。

- 解决自动装配的歧义性
> 标示首选的bean: @Primary  
限定自动装配的bean: @Qualifier注解.它可以与@Autowired和@Inject协同使用，在注入的时候指定想要注入进去的是哪个bean。当通过Java配置显式定义bean的时候，@Qualifier也可以与@Bean注解一起使用  
使用自定义的限定符注解: 自定义限定符注解

- bean的作用域
> 在默认情况下， Spring应用上下文中所有bean都是作为以单例（singleton）的形式创建的。也就是说，不管给定的一个bean被注入到其他bean多少次，每次所注入的都是同一个实例  
单例（Singleton）：在整个应用中，只创建bean的一个实例。在Spring应用上下文加载的时候创建  
原型（Prototype）：每次注入或者通过Spring应用上下文获取的时候，都会创建一个新的bean实例。  
会话（Session）：在Web应用中，为每个会话创建一个bean实例。  
请求（Rquest）：在Web应用中，为每个请求创建一个bean实例  
选择其他的作用域，要使用@Scope注解，它可以
与@Component或@Bean一起使用。

- 注入外部的值
> 在Spring中，处理外部值的最简单方式就是声明属性源并通过Spring的Environment来检索属性。使用@PropertySource注解和Environment

- 深入学习Spring的Environment


- 解析属性占位符
> 在Spring装配中，占位符的形式为使用“${...}”包装的属性名称,这种主要用在XML配置方式，如果是JavaConfig方式的话，可使用@Value注解  
为了使用占位符，我们必须要配置一个PropertyPlaceholderConfigurerbean或PropertySourcesPlaceholderConfigurer bean。从Spring3.1开始，推荐使用PropertySourcesPlaceholderConfigurer，因为它能够基于Spring Environment及其属性源来解析占位符。
- SpEL
> 使用bean的ID来引用bean；
调用方法和访问对象的属性；
对值进行算术、关系和逻辑运算；
正则表达式匹配；
集合操作;
SpEL能够用在依赖注入以外的其他地方  
SpEL表达式要放到“#{ ... }”之中,而属性占位符需要放到“${ ... }”之中

- AOP
> 在AOP术语中，切面的工作被称为通知,通知定义了切面是什么以及何时使用。  
前置通知（Before）：在目标方法被调用之前调用通知功能；  
后置通知（After）：在目标方法完成之后调用通知，此时不会关心方法的输出是什么；  
返回通知（After-returning）：在目标方法成功执行之后调用通知；  
异常通知（After-throwing）：在目标方法抛出异常后调用通知；  
环绕通知（Around）：通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为。

> 连接点（Join point）是在应用执行过程中能够插入切面的一个点,这个点可以是调用方法时、抛出异常时、甚至修改一个字段时。切面代码可以利用这些点插入到应用的正常流程之中，并添加新的行为。

> 切点（Poincut）  如果说通知定义了切面的“什么”和“何时”的话，那么切点就定义
了“何处”。切点的定义会匹配通知所要织入的一个或多个连接点。用于准确定位应该在什么地方应用切面的通
知。

> 切面（Aspect）切面是通知和切点的结合。通知和切点共同定义了切面的全部内容
——它是什么，在何时和何处完成其功能。

> 引入（Introduction）引入允许我们向现有的类添加新方法或属性。

> 织入（Weaving）织入是把切面应用到目标对象并创建新的代理对象的过程。切面在指
定的连接点被织入到目标对象中。在目标对象的生命周期里有多个点
可以进行织入：  
编译期：切面在目标类编译时被织入。这种方式需要特殊的编译器。AspectJ的织入编译器就是以这种方式织入切面的。  
类加载期：切面在目标类加载到JVM时被织入。这种方式需要特殊的类加载器（ClassLoader），它可以在目标类被引入应用之前增强该目标类的字节码。 AspectJ 5的加载时织入（load-timeweaving， LTW）就支持以这种方式织入切面。  
运行期：切面在应用运行的某个时刻被织入。一般情况下，在织入切面时，AOP容器会为目标对象动态地创建一个代理对象。Spring AOP就是以这种方式织入切面的。

- Spring AOP
> Spring对AOP的支持局限于方法拦截。  
Spring提供了4种类型的AOP支持：  
基于代理的经典Spring AOP；  
纯POJO切面；  
@AspectJ注解驱动的切面；  
注入式AspectJ切面（适用于Spring各版本）  
在Spring中，切面只是实现了它们所包装bean相同**接口**的代理。注意是接口

> Spring支持的指示器，只有execution指示器是实际执行匹配的，而其他的指示器都是用来限制匹配的。这说明execution指示器是我们在编写切点定义时最主要使用的指示器。在此基础上，我们使用其他指示器来限制所匹配的切点。

> 为对象拥有的方法添加了新功能 @DeclareParents  Value值后面的加号必须要有，标记符后面的加号表示
是Performance的所有子类型，而不是Performance本
身。


