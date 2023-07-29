

[TOC]

# 前言

“Spring”在不同的上下文中表示不同的事物。它可以用来引用 Spring Framework 项目本身，而这一切都是从那里开始的。随着时间的流逝，其他 Spring 项目已经构建在 Spring Framework 之上。通常，当人们说“Spring”时，它们表示整个项目系列。

Spring 框架分为多个模块。应用程序可以选择所需的模块。核心容器的模块是核心，包括配置模型和依赖项注入机制。除此之外，Spring 框架为不同的应用程序体系结构提供了基础支持，包括消息传递，事务性数据和持久性以及 Web。它还包括基于 Servlet 的 Spring MVC Web 框架，以及并行的 Spring WebFlux 反应式 Web 框架。

——摘自[**Spring Framework 中文文档：https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/spring-framework-reference/core.html#spring-core**](https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/spring-framework-reference/core.html#spring-core)

Spring中最重要的是 Spring 框架的控制反转(IoC)容器。对 Spring 框架的 IoC 容器进行彻底处理之后，将全面介绍 Spring 的面向方面编程(AOP)技术。 Spring 框架具有自己的 AOP 框架，该框架在概念上易于理解，并且成功解决了 Java 企业编程中 AOP 要求的 80％的难题。

# Spring

Spring是 Java EE 编程领域的一个轻量级开源框架，该框架由一个叫 Rod Johnson 的程序员在 2002 年最早提出并随后创建，是为了解决企业级编程开发中的复杂性，实现敏捷开发的应用型框架 。 Spring是一个开源容器框架，它集成各类型的工具，通过核心的 Bean factory 实现了底层的类的实例化和生命周期的管理。在整个框架中，各类型的功能被抽象成一个个的 Bean，这样就可以实现各种功能的管理，包括动态加载和切面编程。

# Spring IOC 简介

## Bean

在 Spring 中，构成应用程序主干并由Spring IoC容器管理的对象称为bean。bean是一个由 Spring IoC 容器实例化、组装和管理的对象。

简单来说，它在 Spring 中定义为“普通、简单的类对象”

## IOC 概述

**控制反转**（Inversion of Control，缩写为**IoC**），是面向对象编程中的一种设计原则，可以用来减低计算机代码之间的耦合度。其中最常见的方式叫做**依赖注入**（Dependency Injection，简称**DI**），还有一种方式叫“依赖查找”（Dependency Lookup）。通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体将其所依赖的对象的引用传递给它。也可以说，依赖被注入到对象中。



## IOC 本质理解

IOC 控制反转实现了根本性的变化，我们以前是在 Dao 层程序控制调用什么，后续还会增加许多业务，但都是基于接口实现的，然后就需要在接口层去设配接口后续的对象。

![](C:\Users\86137\Desktop\截图\Spring\微信截图_20230601113825.png)

+ 以前，程序是主动创建对象，控制权在程序员手上；
+ 使用自动注入后，程序不再具有主动性，变成被动的接受对象。

控制反转 loC(Inversion of Control)，是一种设计思想，DI (依赖注入)是实现 loC 的一种方法，也有人认为 DI 只是 loC 的另一种说法。没有 loC 的程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是:获得依赖对象的方式反转了。

采用XML方式配置 Bean 的时候，Bean 的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean 的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在 Spring 中实现控制反转的是 loC 容器，其实现方法是依赖注入(Dependency Injection,Dl)。**

# Spring IOC 应用

在 Spring 中有三种装配方式：

+ 在 xml 中显示配置；
+ 在 java 中显示配置；
+ 注解的方式：隐式的自动装配 Bean；

## IOC xml装配

+ **IOC 程序试验（Hello）**

  + 在 Maven 项目下，在 pom.xml 中导入maven依赖：

    ```xml
    	<dependencies>
            <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-webmvc</artifactId>
                <version>5.3.20</version>
            </dependency>
        </dependencies>
    ```


  + 创建 pojo 包，新建实体类 Hello

    ```java
    public class Hello {
        private String str;
    
        ……getter、setter（set方法必须存在）
    
        @Override
        public String toString() {
            return "Hello{" +
                    "str='" + str + '\'' +
                    '}';
        }
    }
    ```


  + 在 resources 下新建 ApplicationContext.xml（官方名字），这里为了理解 IOC 本质，暂时将名字设为 beans.xml：

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            https://www.springframework.org/schema/beans/spring-beans.xsd">
        <!--使用Spring来创建对象，这些对象都称为 Bean
            类型 变量名 = new 类型();
            Hello hello = new Hello();
            Spring xml配置文件中：
            id ：变量名
            class ：所要new的对象
        -->
        <beans>
            <bean id="Hello" class="com.hb.pojo.Hello">
                <property name="str" value="HB"></property>
                <!--
    			
    			-->
            </bean>
        </beans>
    </beans>
    ```


  + 创建测试类：

    ```java
    public class HelloTest {
        public static void main(String[] args) {、
            /**
            *提供给ApplicationContext构造函数的位置路径是资源字符串
            *这些资源字符串使容器可以从各种外部资源(例如本地文件系统，Java CLASSPATH等)加载配置元数据。
            */
            //这句话是使用xml文件配置的固定语句，用来获取 Spring 的上下文对象
            ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    
            Hello hello = (Hello) context.getBean("Hello");
            System.out.println(hello.toString());
        }
    }
    ```
    
  + 运行结果为：

    ```java
    Hello{str='HB'}
    ```

    这个过程并没有 new 对象，对象是由Spring容器创建的，这就是控制反转；

  + **总结：**

    控制：谁来控制对象的创建，传统应用程序的对象是由程序本身控制创建的，使用Spring后，对象是由Spring来创建的。
    
    反转：程序本身不创建对象，而变成被动的接收对象。
    
    依赖注入：就是利用set方法来进行注入的。
    
    IOC是一种编程思想，由主动的编程变成被动的接收，可以通过newClassPathXmlApplicationContext去浏览一下底层源码，可以发现Spring帮我们实现了很多东西。
    
    那么从现在开始，我们如果要实现不同的操作，只需要在 xml 配置文件中进行修改，所谓的 loC，就是对象由 Spring 来创建，管理和装配。


+ **IOC 创建对象方式**

  + **默认的创建方式：使用无参构造创建对象；**

  + **在使用有参构造创建对象：**

    + 第一种：通过下标赋值

      ```xml
      <bean id="user" class="com.hb.pojo.User">
          <constructor-arg index="0" value="HB"/>
          <constructor-arg index="1" value="42"/>
      </bean>
      ```

    + 第二种：通过类型赋值

      ```xml
      <!--通过类型创建，不推荐使用-->
      <bean id="user" class="com.hb.pojo.User">
          <constructor-arg type="java.lang.String" value="黄博"/>
      </bean>
      ```

    + 第三种：通过参数名来设置

      ```xml
      <!--直接通过name赋值-->
      <bean id="user" class="com.hb.pojo.User">
          <constructor-arg name="name" value="黄博"/>
      </bean>
      ```

      ```xml
      <!--ref参数引用其他的id-->
      <beans>
          <bean id="beanOne" class="x.y.ThingOne">
              <constructor-arg ref="beanTwo"/>
              <constructor-arg ref="beanThree"/>
          </bean>
      
          <bean id="beanTwo" class="x.y.ThingTwo"/>
      
          <bean id="beanThree" class="x.y.ThingThree"/>
      </beans>
      ```

+ **IoC 配置说明**

  + **别名：**

    ```xml
    <!--添加了别名，也可以使用别名获取到这个对象-->
    <alias name="user" alias="hb"></alias>
    ```

  + **Bean 的配置：**

    ```xml
    <!--
    id：bean的唯一标识符 => 对象名
    class：bean 对象所对应的全限定名（包名+类名）
    name：也是别名，同时可以起多个
    …………等等
    -->
    <bean id="user" class="com.hb.pojo.User" name="u1,u2">
    	<property name="name" value="hb"></property>
    </bean>
    ```

  + **import：**

    用于团队开发，将多个配置文件`ApplicationContext.xml`导入合并为一个。

    ```xml
    <import resource="ApplicationContext1.xml"></import>
    <import resource="ApplicationContext2.xml"></import>
    <import resource="ApplicationContext3.xml"></import>
    ```

    使用的是时候，直接使用总配置。

## IOC 依赖注入

+ **依赖注入：set 注入**
  + 依赖：bean 对象的创建依赖于容器；
  + 注入：bean 对象中的所有属性，由容器来注入（完成初始化）；

+ **映射键或值的值或设定值可以是以下任何元素：**

  ```xml
  bean | ref | idref | list | set | map | props | value | null
  ```

+ **试验所有注入：**

  + 复杂类型创建：

    ```java
    package com.hb.pojo;
    
    public class Address {
        private String address;
    
    	…………一系列getter、setter、toString方法
    }
    
    ```

  + 真实测试对象：

    ```java
    package com.hb.pojo;
    
    import java.util.*;
    
    public class University {
        private String name;
        private Address address;
        private String[] colleges;
        private List<String> majors;
        private Map<String,String> classes;
        private Set<String> student;
        private Properties info;
        private String doctoralProgram;
    
    	…………一系列getter、setter
       	
    	@Override
        public String toString() {
            return "name = " + name + '\n' +
                    "address = " + address + '\n' +
                    "colleges = " + Arrays.toString(colleges) + '\n' +
                    "majors = " + majors + '\n' +
                    "classes = " + classes + '\n' +
                    "student = " + student + '\n' +
                    "info = " + info + '\n' +
                    "doctoralProgram = " + doctoralProgram;
        }
    }
    ```

  + `ApplicationContext.xml`：

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            https://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <bean id="add" class="com.hb.pojo.Address">
            <property name="address" value="陕西省西安市"></property>
        </bean>
    
        <bean id="university" class="com.hb.pojo.University">
            <!--常用！第1种，普通值注入，value-->
            <property name="name" value="西安邮电大学"></property>
            <!--常用！第2种，Bean注入，ref-->
            <property name="address" ref="add"></property>
            <!--第3种，数组注入-->
            <property name="colleges">
                <array>
                    <value>通院</value>
                    <value>计院</value>
                </array>
            </property>
            <!--第4种，List注入-->
            <property name="majors">
                <list>
                    <value>通工</value>
                    <value>物联网</value>
                </list>
            </property>
            <!--第5种，map注入-->
            <property name="classes">
                <map>
                    <entry key="1" value="通工1班"></entry>
                </map>
            </property>
            <!--第6种，set注入-->
            <property name="student">
                <set>
                    <value>张三</value>
                </set>
            </property>
            <!--第7种，NULL注入-->
            <property name="doctoralProgram">
                <null/>
            </property>
            <!--第8种，Properties注入-->
            <property name="info">
                <props>
                    <prop key="编号">11664</prop>
                </props>
            </property>
        </bean>
    </beans>
    ```

  + 测试类：

    ```java
    public class Test {
        public static void main(String[] args) {
            ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
            University university = (University) context.getBean("university");
            System.out.println(university.toString());
        }
    }
    ```

  + 测试结果：

    ![](C:\Users\86137\Desktop\截图\Spring\微信截图_20230602193143.png)

+ **p 命名和 c 命名空间注入：**

  + p 命名空间注入——对应 Set 方式注入

    p 名称空间允许您使用bean元素的属性（而不是嵌套的＜property/＞元素）来描述协作 bean 的属性值，或者两者兼而有之。

    Spring 支持具有命名空间的可扩展配置格式，命名空间基于 XMLSchema 定义。而 bean 配置格式是在 XMLSchema 文档中定义的。但是，p 名称空间没有在 XSD 文件中定义，只存在于 Spring 的核心中。

    示例：

    ```xml
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:p="http://www.springframework.org/schema/p"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            https://www.springframework.org/schema/beans/spring-beans.xsd">
            
            <!--p命名空间注入，可以直接注入属性的值：property-->
            <bean id="user" class="com.hb.pojo.User" p:name="HB" p:age="21"></bean>
    </beans>
    ```

  + c 命名空间注入——对应构造器注入

    与带有p名称空间的XML快捷方式类似，Spring 3.1 中引入的 c 名称空间允许内联属性用于配置构造函数参数，而不是嵌套的构造函数 arg 元素。

    ```xml
    <!--必须有有参构造方法-->
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:c="http://www.springframework.org/schema/c"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            https://www.springframework.org/schema/beans/spring-beans.xsd">
    
            <!--c命名空间注入，通过构造器注入：constructor-arg-->
            <bean id="user" class="com.hb.pojo.User" c:name="HB" c:age="21"></bean>
    </beans>
    ```

  注意：p 命名和 c 命名空间注入不能直接使用，需要导入约束（`xmlns:p="http://www.springframework.org/schema/p"`）；

## IOC Bean的作用域

| 作用域      | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| singleton   | 在spring IoC容器仅存在一个Bean实例，Bean以单例方式存在，bean作用域范围的默认值。 |
| prototype   | 每次从容器中调用Bean时，都返回一个新的实例。                 |
| request     | 每次HTTP请求都会创建一个新的Bean，该作用域仅适用于 web 的 Spring WebApplicationContext 环境。 |
| session     | 将单个bean定义范围界定为HTTP会话的生命周期。仅在web感知的Spring ApplicationContext的上下文中有效. |
| application | 限定一个Bean的作用域为`ServletContext`的生命周期。该作用域仅适用于web的Spring WebApplicationContext环境。 |
| websocket   | 将单个bean定义范围界定为WebSocket的生命周期。仅在web感知的Spring ApplicationContext的上下文中有效。 |

+ 单例模式（Spring 默认机制）

  如果 bean 的作用域的属性被声明为 singleton ，那么 Spring Ioc 容器只会创建一个共享的 bean 实例。对于所有的 bean 请求，只要 id 与该 bean 定义的相匹配，那么 Spring 在每次需要时都返回同一个 bean 实例。

  ```xml
  <bean id="user" class="com.hb.pojo.User" scope="singleton">
  	<property name="name" value="hb"></property>
  </bean>
  ```

+ 原型模式：每次从容器中 get 的时候，都会产生一个新对象

  当一个 bean 的作用域为 prototype，表示一个 bean 定义对应多个对象实例。声明为 prototype 作用域的 bean 会导致在每次对该 bean 请求时都会创建一个新的 bean 实例，每次获得的对象都是不同的。

  ```xml
  <bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
  ```

+ 其余的`request`,`session` 和 `application`这三个作用域都是基于 web 的 Spring Web `ApplicationContext`实现的，只有在 web 环境下中会使用到。 

# IoC 自动装配

## Bean 的自动装配

+ 自动装配是 Spring 满足 Bean 依赖的一种方式；
+ Spring 会在上下文中自动寻找，并自动给 Bean 装配属性；

byName 自动装配：

```xml
	<!--byName：会自动在上下文中查找，和自己对象set方法后面的值对应的 beanid;-->
	<bean id="user" class="com.hb.pojo.User" autowire="byName">
        <property name="name" value="hb"></property>
    </bean>
```

注意：需要保证所有的 bean 的 id 唯一，并且这个 bean 需要和自动注入的属性的 set 方法的值一致；

byType 自动装配：

```xml
	<!--byType：会自动在上下文中查找，和自己对象属性类型相同的 bean;-->
	<bean id="user" class="com.hb.pojo.User" autowire="byType">
        <property name="name" value="hb"></property>
    </bean>
```

注意：需要保证所有的 bean 的 class 唯一，并且这个 bean 需要和自动注入的属性类型一致；                                              

## 注解实现自动装配

**使用注解前需要：**

1、导入约束：context 约束。

2、配置注解的支持：`<context:annotation-config/>`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
</beans>
```

+ **@Autowired 注解（最常用）** 

  @Autowired 注解直接在属性上使用即可，通过 byType 的方式实现，例如：

  ```java
  public class User {
      @Autowired
      private String name;
      @Autowired
      private int age;
  
  	…………一系列getter、setter、toString方法
  }
  ```

  拓展：

  `@Autowired（required = false）`：如果显示定义了 Autowired 的 required 的属性为 false ，说明这个对象可以为 null，否则不允许为空。

  `@Nullable`：字段标记了这个注解，说明这个字段可以为 null；

+ **@Qualifier(value=“xxx”)**

  `@Qualifier(value=“xxx”)`：用于指定唯一的 Bean 对象注入。

  如果 @Autowired 自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们可以使用`@Qualifier(value=“xxx”)` 来配合 @Autowired。

  ```java
  public class User {
      @Autowired
      @Qualifier(value="xxx")
      private String name;
      @Autowired
      @Qualifier(value="xxx")
      private int age;
  
  	…………一系列getter、setter、toString方法
  }
  ```

+ **@Resource注解**

  ```java
  public class User {
  	@Resource
      private String name;
  	@Resource
      private int age;
  
  	…………一系列getter、setter、toString方法
  }
  ```

  @Resource 默认通过byName的方式实现，如果找不到名字，则通过byType的方式实现，如果两个都找不到，就报错。

# IoC 使用注解开发

首先，需要导入 context 约束，来增加注解的支持：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.hb.pojo"/>
    <context:annotation-config/>
</beans>
```

+ **@Component**

  @Component 组件，放在类上，说明这个类被 Spring 管理了，就是Bean，例如：

  ```java
  //@Component：组件，等价于<bean id="user" class="com.hb.pojo.User"/>
  @Component
  public class User {
      public String name = "HB";
  }
  ```

+ **@Value("xxx")**

  ```java
  @Component
  public class User {
      //等价于<property name="name" value="HB"></property>
      @Value("HB")
      public String name;
  }
  ```

+ **衍生的注解**

  @Component有几个衍生的注解，在 web 开发中，会按照 mvc 三层架构分层；

  + dao【@Repository】
  + service【@Service】
  + controller【@Controller】

  这四个注解功能都是一样的，都是代表将某个类注册到 Spring 中，装配 Bean；

+ **作用域**

  `@Scope(“prototype”)`：表示 Bean 的作用域。

**总结：**

xml 与注解：

+ xml ：更加万能，适用于任何场合，维护简单方便；
+ 注解：不是自己的类用不了，维护相对复杂；

xml 与注解结合使用，xml 用来管理 bean，注解只需完成属性的注入，卫门在使用的过程中直须注意让注解生效，开启注解的支持。

# 模拟实现Spring IoC

请见[HB个人博客：模拟实现 Spring IOC（）]()