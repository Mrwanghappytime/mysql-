# spring 框架

### 1.整体架构

![image-20210223145255397](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210223145255397.png)

- ​	**spring** **核心容器**

  ​		容器是Spring框架最核心的部分，它管理着Spring应用中bean的创建、 配置和管理。在该模块中，包了Spring bean工厂，它为Spring提供 了DI的功能。基于bean工厂，我们还会发现有多种Spring应用上下文 

  的实现，每一种都提供了配置Spring的不同方式。

- **spring 的AOP模块**

  ​		在AOP模块中，Spring对面向切面编程提供了丰富的支持。这个模块是Spring应用系统中开发切面的基础。与DI一样，AOP可以帮助应用 49对象解耦。借助于AOP，可以将遍布系统的关注点（例如事务和安全）从它们所应用的对象中解耦出来。 

- **数据访问与集成**

  ​		使用JDBC编写代码通常会导致大量的样板式代码，例如获得数据库连接、创建语句、处理结果集到最后关闭数据库连接。Spring的JDBC和DAO（Data Access Object）模块抽象了这些样板式代码，使我们的数据库代码变得简单明了，还可以避免因为关闭数据库资源失败而引发的问题。该模块在多种数据库服务的错误信息之上构建了一个语义丰富的异常层，以后我们再也不需要解释那些隐晦专有的SQL错误信息了！ 

### 2.spring核心容器

```properties
#在XML中进行显式配置。 在Java中进行显式配置。 隐式的bean发现机制和自动装配。
1.Autowired 注解 ，自动装配注解
使用方式：①直接在属性上使用，会装配名字相同bean ②在构造方法上使用 ③在Setter方法上使用

2.Configuration注解 用于标识此类为配置类，用于配置bean

3. @Profile 用于标识使用起作用于那种环境，可使用@ActiveProfile用于启动环境注解 或者在xml文件的beans中加入profile=""
4.@Conditional注解标识某个类存在时才使@Bean生效，其中@Profile由这个注解实现
5.spring bean的歧义性，  @Primary用于标识首选的bean @Qualifier注解用于标识bean注入的id，用于与@Autowired注解一起使用，标识注入的bean id为指定id，例@Qualifier("id")
6.

```

### 3.spring中bean的配置方式

```properties
1.使用@Component注解，此时需要开启注解扫描，扫描方式可以是在xml文件使用
<Context:component-scan base-package="com.haofeng.spring"/>
也可以使用@ComponentScan(basePackages = "com.haofeng.spring")注解
2.直接在xml中进行配置，配置方式为
<bean class="" id="">,配置属性可以通过构造器注入和setter方法进行注入
3.在config进行配置，配置方式如下
@Bean
public Student getStudent(){
	return new Student();
}
4.当config和xml方式需要同时使用时，可以在config中通过@ImportResource注解进行注入，也可以在xml对config类进行配置即可
```

### 4.spring AOP

```
1.AOP中的相关概念
```

### 5.springMVC

```
1.dispactcher springmvc的控制servlet
	流程为映射控制器，控制器，视图解析器，视图之后响应结果
2.在不使用web.xml时可以使用继承AbstractAnnotationConfigDispatcherServletInitializer
	此时需要实现三个接口，接口用于指定Spring的配置类，在spring配置类中需要使用@EnableWebMvc进行配置使能springmvc功能，然后使能扫描包即可
3.@Controller用于注解bean使其成为c层，@RequestMapping用于指定映射的路径和请求的方式
4.@ReponseBody用于标记回复的字符串
5.



```

