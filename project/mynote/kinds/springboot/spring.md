**初识spring**

[TOC]

# 1、Spring的发展

## 1.1Spring1.x时代

在Spring1.x时代，都是通过xml文件配置bean，随着项目的不断扩大，需要将xml配置分放到不同的配置文件中，需要频繁的在java类和xml配置文件中切换。

## 1.2Spring2.x时代

随着JDK  1.5带来的注解支持，Spring2.x可以使用注解对Bean进行申明和注入，大大的减少了xml配置文件，同时也大大简化了项目的开发。

那么，问题来了，究竟是应该使用xml还是注解呢？

## 1.3注解还是XML

在spring早期版本中，由于当时的JDK并不支持注解，因此只能使用XML的配置，很快，随着JDK升级到JDK5之后，它加入了注解的新特性，这样注解就被广泛地使用起来，  于是spring内部也分为两派，一派是使用XML的，一派是使用注解的，为了简化开发，在spring2.X之后的版本也引入了注解，不过是少量的注解，如@Component  @Service等，但是功能还是不强大，因此对于srping的开发，大部分情况下还是使用XML为主，随着注解的增加，尤其是Servlet3.0之后，WEB容器可以脱离web.xml的部署，使用得WEB容器完全可以基于注解开发，对于spring3和spring4的版本注解功能越来越强大。对于XML的依赖起来越少，到了4.0完全可以脱离XML，  所以在spring中使用注解开发占据了主流地位，近年来。微服务的流行，越来越多的企业要求快速开发，所以spring Boot更加兴旺了。

1、应用的基本配置用xml，比如：数据源、资源文件等；

2、业务开发用注解，比如：Service中注入bean等；

## 1.4Spring3.x到Spring4.x

从Spring3.x开始提供了Java配置方式，使用Java配置方式可以更好的理解你配置的Bean，现在我们就处于这个时代，并且Spring4.x和Springboot都推荐使用java配置的方式。

## 1.5SpringBoot的优点

1，创建独立的spring应用程序。

2，嵌入的tomcat jetty 或者undertow 不用部署WAR文件。

3，允许通过Maven来根据需要获取starter

4，尽可能的使用自动配置spring

5，提供生产就绪功能，如指标，健康检查和外部配置

6，绝对没有代码生成，对XML没有要求配置

# 2、Spring简介和微服务的介绍

## 2.1springboot简介

　Spring Boot 是由 Pivotal 团队提供的全新框架，其设计目的是用来简化新 Spring 应用的初始搭建以及开发过程。

　　该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。　　

　　通过这种方式，Spring Boot 致力于在蓬勃发展的快速应用开发领域（rapidapplication development）成为领导者。

## 2.2为什么用springboot

　　创建独立的 Spring 应用程序

　　嵌入的 Tomcat，无需部署 WAR 文件

　　简化 Maven 配置

　　自动配置 Spring

　　提供生产就绪型功能，如指标，健康检查和外部配置

　　开箱即用，没有代码生成，也无需 XML 配置。

​    与云计算天然集成

## 2.3特性理解　

　　为基于 Spring 的开发提供更快的入门体验

　　开箱即用，没有代码生成，也无需 XML 配置。同时也可以修改默认值来满足特定的需求。

　　提供了一些大型项目中常见的非功能特性，如嵌入式服务器、安全、指标，健康检测、外部配置等。

　　Spring Boot 并不是对 Spring 功能上的增强，而是提供了一种快速使用 Spring 的方式。　

## 2.4传统开发模式

所有的功能打包在一个 WAR包里，基本没有外部依赖（除了容器），部署在一个JEE容器（Tomcat，JBoss，WebLogic）里，包含了 DO/DAO，Service，UI等所有逻辑。

![image-20200223191334232](typora-user-images\image-20200223191334232.png)

优点：

> ①开发简单，集中式管理
>
> ②基本不会重复开发
>
> ③功能都在本地，没有分布式的管理和调用消耗

> 

缺点：

> 1、效率低：开发都在同一个项目改代码，相互等待，冲突不断
>
> 2、维护难：代码功功能耦合在一起，新人不知道何从下手
>
> 3、不灵活：构建时间长，任何小修改都要重构整个项目，耗时
>
> 4、稳定性差：一个微小的问题，都可能导致整个应用挂掉
>
> 5、扩展性不够：无法满足高并发下的业务需求
>
> 6、对服务器的性能要求要统一，要高

## 2.5微服务开发

微服务：架构风格(服务微化)

​    微服务是指开发一个单个小型的但有业务功能的服务，每个服务都有自己的处理和轻量通信机制，可以部署在单个或多个服务器上，微服务也指一种松耦合的，有一定有界上下文的**面向服务的架构**    

 目的：有效的拆分应用，实现敏捷开发和部署

![image-20200223191507171](.\typora-user-images\image-20200223191507171.png)

优点

  1，每个微服务都很小，这样能聚焦一个指定的业务功能或业务需求

  2，微服务能够被小团队开发，这个小团队2-5人就可以完成了

  3，微服务是松耦合的，是有功能，有意义的服务，无论在开发阶段或部署阶段都是独立的

  4，微服务可以使用不同的语言开发

  5，微服务能部署在中低端配置的服务器上

  6，很容易和第三方集成

  7，每个服务都有自己的存储能力，单独的库，也可以有统一的库

缺点

  1，微服务会带来过多的操作

  2，可以有双倍的努力 

  4，分布式系统可能复杂难管理

  5，分布跟踪部署难

  6，当服务数量增加时，管理复杂度增加



# 3、入门程序

## 3.1环境准备

> ①JDK1.8
>
> ②maven3.x
>
> ③spring tools suite3.9
>
> ④springboot2.x版本
>
> ⑤spring5

方式一：

https://start.spring.io/  ， 其它两种方式的本质也是去这个网站创建然后返回过去的

方式二：

**通过STS创建**

![image-20200223200916458](.\typora-user-images\image-20200223200916458.png)

![image-20200223200924903](.\typora-user-images\image-20200223200924903.png)

![image-20200223200936180](.\typora-user-images\image-20200223200936180.png)

![image-20200223200947682](.\typora-user-images\image-20200223200947682.png)![image-20200223200947845](.\typora-user-images\image-20200223200947845.png)

![image-20200223201010735](.\typora-user-images\image-20200223201010735.png)

方式三：

**通过IDEA创建**

![image-20200223201102117](.\typora-user-images\image-20200223201102117.png)

![image-20200223201123505](.\typora-user-images\image-20200223201123505.png)

![image-20200223201302185](.\typora-user-images\image-20200223201302185.png)

![image-20200223201331088](.\typora-user-images\image-20200223201331088.png)

![image-20200223201347145](.\typora-user-images\image-20200223201347145.png)

## 3.2pom.xml的配置说明

![image-20200223201528009](.\typora-user-images\image-20200223201528009.png)

![image-20200223201556521](.\typora-user-images\image-20200223201556521.png)

## 3.3jar包启动测试

右键项目run

![image-20200223201640935](.\typora-user-images\image-20200223201640935.png)

查看target

![image-20200223201658623](.\typora-user-images\image-20200223201658623.png)

把jar包放到D盘，使用java -jar 01_springboot_helloworld-0.0.1-SNAPSHOT.jar来执行

![image-20200223201720684](.\typora-user-images\image-20200223201720684.png)

测试

![image-20200223201741242](.\typora-user-images\image-20200223201741242.png)

## 3.4banner的修改

spring Boot启动的时候会有一个默认的启动图案。如下图

![image-20200223201805053](.\typora-user-images\image-20200223201805053.png)

生成图案的网站 http://www.network-science.de/ascii/

也可以关闭banner

![image-20200223201840313](.\typora-user-images\image-20200223201840313.png)

# 4、常用注解

Java配置是Spring4.x推荐的配置方式，可以完全替代xml配置。

## 4.1相关注解说明

1、@Configuration作用于类上，相当于一个xml配置文件；

2、@Bean作用于方法上，相当于xml配置中的<bean>；

3、@Import注解 在创建配置文件之后可以引入其它的配置文件

4、@ComponentScan("com.sxt")配置扫描

5、@Qualifier注解了，qualifier的意思是合格者，通过这个标示，表明了哪个实现类才是我们所需要的，我们修改调用代码，添加@Qualifier注解，需要注意的是@Qualifier的参数名称必须为我们之前定义@Bean注解的名称之一

6、@Primary ： 主要的意思，在定义的时候指明使用哪一个。

# 5、什么是热部署

spring为开发者提供了一个名为spring-boot-devtools的模块来使springboot应用支持热部署，提高开发的效率，修改代码后无需重启应用

![image-20200223202128019](.\typora-user-images\image-20200223202128019.png)

# 6、springboot启动分析【难点】

**启动流程及注解分析**

## 6.1@SpringBootApplication

**@SpringBootApplication SpringBoot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动springBoot的应用**

![image-20200223205506752](.\typora-user-images\image-20200223205506752.png)

## 6.2@SpringBootConfiguration 

标记在某个类上表示是一个springboot的配置类和@Configuration 一样

![image-20200223205545049](.\typora-user-images\image-20200223205545049.png)

## **6.3@Configuration **

作用于类上，相当于一个xml配置文件

![image-20200223205720892](.\typora-user-images\image-20200223205720892.png)

## **6.4@EnableAutoConfiguration**

开启自动配置功能



以前我们要自己配置东西，现在spring boot帮我们自动配置自己动扫描，@EnableAutoConfiguration就是告诉springBoot开启自动配置功能，这样自动配置才会生效

![image-20200223205818953](.\typora-user-images\image-20200223205818953.png)

## 6.5@AutoConfigurationPackage

自动扫包的配置



![image-20200223210119485](.\typora-user-images\image-20200223210119485.png)

**6.6@Import(AutoConfigurationPackages.Registrar.class)对包进行注册**

**AutoConfigurationPackages.Registrar.class**

![image-20200223210239748](.\typora-user-images\image-20200223210239748.png)

![image-20200223210315268](.\typora-user-images\image-20200223210315268.png)

![image-20200223210339344](.\typora-user-images\image-20200223210339344.png)

**先加载所有的自动导包。后面再过滤项目中没有用到的包**

![image-20200223210717412](.\typora-user-images\image-20200223210717412.png)

## 6.6springboot提供了哪些starter

https://docs.spring.io/spring-boot/docs/2.1.0.RELEASE/reference/htmlsingle/#using-boot-starter 

![image-20200223210840581](.\typora-user-images\image-20200223210840581.png)

```
<servlet>
	<servlet-name>springmvc</servlet-name>
<servlet-class> org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>springmvc</servlet-name>
<url-patten>/ </url-patten>
</servlet-mapping>

分析：<servlet>：相当于配置到容器中，
<servlet-mapping>：对外暴露访问的方式
```

# 7、springboot的两种配置文件语法

解决提示问题

![image-20200223211407661](.\typora-user-images\image-20200223211407661.png)

创建Student

@ConfigurationProperties(prefix=”student”)

Springboot标记了IOC容器里面一个对象

当IOC里面的对象初始化完成之后，再去扫描ConfigurationProperties

然后把配置文件里面是student前缀开头的配置注入到IOC这个对象的相应的set方法里面

**类是被动的注入**

![image-20200223211526362](.\typora-user-images\image-20200223211526362.png)

## 7.1application.properties

使用application.properties的文件注入

![image-20200223211550537](.\typora-user-images\image-20200223211550537.png)

## 7.2application.yml

使用application.yml的文件注入

![image-20200223211624558](.\typora-user-images\image-20200223211624558.png)

## 7.3配置文件占位符

![image-20200223211642805](.\typora-user-images\image-20200223211642805.png)

![image-20200223211654510](.\typora-user-images\image-20200223211654510.png)

## 7.4两种方法的说明

1，如果properties里面配置了就不会去yml里面去取值，如果没有配置就会去yml里面去取

2，**两种配置方法是互补的**

3，**properties的优先级高于yml**

## 7.5@Value

通过@Value，将配置文件的内容注入到对应的类中

**类是主动的去注入，需要一个一个配置**

![image-20200228212519796](.\typora-user-images\image-20200228212519796.png)

![image-20200228212531045](.\typora-user-images\image-20200228212531045.png)



# 8、验证处理

注解验证：通过**@Validated、@Email**

![image-20200224173953246](.\typora-user-images\image-20200224173953246.png)

# 9、**@PropertySource和@ImportResource**

**1、为使用要使用@PropertySource？**

上面的注入，所有的配置都是写在appliaction.properties或application.yml文件里，那么如果不想写在这里面怎么处理呢使用@PropertySource可以解决

![image-20200228233045711](.\typora-user-images\image-20200228233045711.png)

**2、注入优先级的问题**

  所在的配置都是优先注入appliaction.properties或application.yml里面的数据

  如果要不一样，必须修改配置文件引入的前缀

**3、为什么要使用@ImportResource**

从上面所有的配置中可以看出我们没有使用以前的spring的xml的配置方法，如果还是要使用spring里面的xml的配置方式怎么办理，使用@ImportResource

![image-20200224174411791](.\typora-user-images\image-20200224174411791.png)

![image-20200224174550392](.\typora-user-images\image-20200224174550392.png)

# 10、profiles配置详解

**为什么要使用profiles**

 在开发中，一般有两种环境

​    1，生产环境  [项目上线，客户在使用中，就是生产环境]

​    2，开发环境[就是开发环境，不解释]

  有时候开发环境和生产环境的配置方法是不一样的，那么如何快速的切换呢，这里就要使用profiles文件

![image-20200224174724409](.\typora-user-images\image-20200224174724409.png)

**去掉application.properties的jar包运行方式**

![image-20200224174752209](.\typora-user-images\image-20200224174752209.png)

2、java -jar运行

![image-20200224174823924](.\typora-user-images\image-20200224174823924.png)

# 11、配置文件加载优先级

**项目内部配置文件**

spring boot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件

**其中同一目标下的properties文件的优先级大于yml文件**

![image-20200224175907638](.\typora-user-images\image-20200224175907638.png)

![image-20200224175932865](.\typora-user-images\image-20200224175932865.png)

以上是按照优先级从高到低的顺序，所有位置的文件都会被加载，高优先级配置内容会覆盖低优先级配置内容。

SpringBoot会从这四个位置全部加载主配置文件，如果高优先级中配置文件属性与低优先级配置文件不冲突的属性，则会共同存在—互补配置。

我们可以从ConfigFileApplicationListener这类便可看出，其中DEFAULT_SEARCH_LOCATIONS属性设置了加载的目录：

![image-20200224180017131](.\typora-user-images\image-20200224180017131.png)

# 12、**外部的配置文件**

**1、使用配置文件的路径**

我们也可以通过配置spring.config.location来改变默认配置。

![image-20200224182256364](.\typora-user-images\image-20200224182256364.png)

项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置。

指定配置文件和默认加载的这些配置文件共同起作用形成互补配置。

**2、使用命令行参数**

所有的配置都可以在命令行上进行指定；

多个配置用空格分开； –配置项=值

![image-20200224182427976](.\typora-user-images\image-20200224182427976.png)

# 13、自动配置原理及@Conditional派生注解

如何写配置文件

[https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#common-application-properties](#common-application-properties)

**自动配置的原理**

**SpringBoot启动的时候加载主配置类，开启了自动配置功能 @EnableAutoConfiguration**

![image-20200224182745454](.\typora-user-images\image-20200224182745454.png)

![image-20200224182756391](.\typora-user-images\image-20200224182756391.png)

**@EnableAutoConfiguration 作用：**

利用AutoConfigurationImportSelector给容器中导入一些组件

![image-20200224182826284](.\typora-user-images\image-20200224182826284.png)

可以查看AutoConfigurationImportSelector.java里的selectImports()方法的内容得知获取了哪些组件；

![image-20200224182912025](.\typora-user-images\image-20200224182912025.png)

![image-20200224183506221](.\typora-user-images\image-20200224183506221.png)

![image-20200224183513704](.\typora-user-images\image-20200224183513704.png)

![image-20200224183532539](.\typora-user-images\image-20200224183532539.png)

![image-20200224183624881](.\typora-user-images\image-20200224183624881.png)

![image-20200224183647337](.\typora-user-images\image-20200224183647337.png)

将类路径下  META-INF/spring.factories 里面配置的所有EnableAutoConfiguration的值加入到了容器中；

每一个这样的  xxxAutoConfiguration类都是容器中的一个组件，都加入到容器中；用他们来做自动配置；

![image-20200224183718184](.\typora-user-images\image-20200224183718184.png)

所有在配置文件中能配置的属性都是在xxxxProperties类中封装着；配置文件能配置什么就可以参照某个功能对应的这个属性类

![image-20200224183930492](.\typora-user-images\image-20200224183930492.png)

**总结：**

![image-20200224184139342](.\typora-user-images\image-20200224184139342.png)

## @Conditional派生注解

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置里面的所有内容才生效；

```
@ConditionalOnJava	系统的java版本是否符合要求(*)
@ConditionalOnBean	容器中存在指定Bean；(*)
@ConditionalOnMissingBean	容器中不存在指定Bean；
@ConditionalOnExpression	满足SpEL表达式指定
@ConditionalOnClass	系统中有指定的类(*)
@ConditionalOnMissingClass	系统中没有指定的类
@ConditionalOnSingleCandidate	容器中只有一个指定的Bean，或者这个Bean是首选Bean
@ConditionalOnProperty	系统中指定的属性是否有指定的值
@ConditionalOnResource	类路径下是否存在指定资源文件
@ConditionalOnWebApplication	当前是web环境(*)
@ConditionalOnNotWebApplication	当前不是web环境
@ConditionalOnJndi	JNDI存在指定项
```

# 14、WEB静态资源访问规则

**1、springboot访问静态资源的几种方式** 

![image-20200228172323473](.\typora-user-images\image-20200228172323473.png)

![image-20200228172441480](.\typora-user-images\image-20200228172441480.png)

如果每个目录下面都有相同的文件，那么访问的优先级为

META-INF>resources>static>public

**2、自定义静态文件配置的方式**

1、**创建JAVA类**

![image-20200228172523843](.\typora-user-images\image-20200228172523843.png)

**3，看完自定义访问静态资源不知道大家有没有猜到为什么springboot可以访问/META-INF/resources，resources，static，public这4个文件夹下的静态资源，并且直接访问文件名称即可。**

下面我们来看一下springboot中的源码： 

(1)打开WebMvcAutoConfiguration$WebMvcAutoConfigurationAdapter类找到addResourceHandlers方法

![image-20200228172900042](.\typora-user-images\image-20200228172900042.png)

![image-20200228172929456](.\typora-user-images\image-20200228172929456.png)

![image-20200228172959506](.\typora-user-images\image-20200228172959506.png)

**4、webjars的访问配置**

4.1**什么是webjars**

WebJars是打包到JAR（Java Archive）文件中的客户端Web库（例如jQuery和Bootstrap）。

在基于JVM的Web应用程序中显式轻松地管理客户端依赖项

使用基于JVM的构建工具（例如Maven，Gradle，sbt，...）来下载客户端依赖项

了解您正在使用的客户端依赖项

传递依赖关系会自动解析，并可选择通过RequireJS加载

4.2**springboot集成webjars**

配置pom.xml

```xml
<dependency>
    <groupId>org.webjars.bower</groupId>
    <artifactId>jquery</artifactId>
    <version>3.4.1</version>
</dependency>
```

**4.3查看jar包**

![image-20200228173212102](.\typora-user-images\image-20200228173212102.png)

**4.4服务测试**

![image-20200228173342211](.\typora-user-images\image-20200228173342211.png)

**springboot是如何配置webjars的**

![image-20200228173447905](.\typora-user-images\image-20200228173447905.png)

# 15、AOP

1、概述

aop是spring的两大功能模块之一,功能非常强大,为解耦提供了非常优秀的解决方案。SpringBoot集成aop是非常方便的，下面使用aop来拦截业务组件的方法

**通俗来讲：AOP就是，在不修改源代码的前提下，对类里面的方法进行增强**

```
前置
后置
环绕
异常
```

2、**添加maven依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

**3、创建Man类**

```java
package com.hjh.domain;

import lombok.Data;
import org.springframework.stereotype.Component;

/**
 * @author HJH
 * @date 2020-02-28 17:43
 */
@Data
@Component
public class Man {


    public void eat(){
        System.out.println("人吃饭");
        int a = 10 / 0;
    }

    public String sleep(String arg){
        System.out.println("sleep");
        return "sleep";
    }

}
```

**4、创建切面类**

```java
package com.hjh.aspect;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Around;

/**
 * @author HJH
 * @date 2020-02-28 17:47
 */
@Component
@Aspect
//@EnableAspectJAutoProxy // 这个默认启动，可以不加
public class ManAspect {

    /**
     * 声明切面
     */
    public static final String POINTCUT1 = "execution(* com.hjh.domain.Man.eat(..))";


    /*@Pointcut(POINTCUT1)
    public void mycut(){

    }*/

    //@Before(POINTCUT1)
    public void before(){
        System.out.println("喝汤");
    }

    //@After(POINTCUT1)
    public void after(){
        System.out.println("玩电脑");
    }

    @Around(POINTCUT1)
    public void around(ProceedingJoinPoint pjp) throws Throwable {
        before();
        pjp.proceed();
        after();
    }

    @AfterThrowing(value = POINTCUT1,throwing = "tw")
    public void exp(Throwable tw){
        System.out.println("出现异常：" + tw.getMessage());
    }
}
```

**切面类的另一种配置方法**

```java

@Aspect
@Component
//@EnableAspectJAutoProxy  //true
public class MyAspect {
	
	
	
	/**
	 * 声明切面
	 */
	@Pointcut(value="execution(* com.sxt.domain.*.*(..))")
	public void pc() {
		
	}
	
	
//	@Before(value="pc()")
	public void before() {
		System.out.println("吃点水果");
	}
	
	
	
//	@After(value="pc()")
	public void after() {
		System.out.println("搞一根");
	}

	
	@Around(value="pc()")
	public void around(ProceedingJoinPoint joinPoint) {
		try {
			Object[] args = joinPoint.getArgs();
			for (Object object : args) {
				System.out.println(object+"-----");
			}
			before();
			Object proceed = joinPoint.proceed();//执行目标方法
			System.out.println(proceed+"......");
			after();
		} catch (Throwable e) {
			e.printStackTrace();
		}
	}
	
	//@AfterThrowing(pointcut="pc()",throwing="tw")
	public void error(Throwable tw) {
		System.out.println("目标出现异常");
	}
	
}
```



## 切面的高级用法一

**当被增强的方法有返回值的切面类**

```java
package com.hjh.aspect;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.context.annotation.EnableAspectJAutoProxy;
import org.springframework.stereotype.Component;

/**
 * @author HJH
 * @date 2020-02-28 17:47
 */
@Component
@Aspect
@EnableAspectJAutoProxy // 这个默认启动，可以不加
public class ManAspect2 {

    /**
     * 声明切面
     */
    public static final String POINTCUT1 = "execution(* com.hjh.domain.Man.sleep(..))";


    /*@Pointcut(POINTCUT1)
    public void mycut(){

    }*/

    //@Before(POINTCUT1)
    public void before(){
        System.out.println("喝汤");
    }

    //@After(POINTCUT1)
    public void after(){
        System.out.println("玩电脑");
    }

    @Around(POINTCUT1)
    public Object around(ProceedingJoinPoint pjp) throws Throwable {

        Object[] args = pjp.getArgs();
        System.out.println(args[0]);


        before();
        Object result = pjp.proceed();
        after();
        return result;
    }

}
```

## 切面的高级用法二

**切面可以进行缓存操作**

```
什么是缓存？
有三个部分：应用程序，缓存，服务器，
当应用程序向服务器请求资源的时候，在服务器中查到数据再进行拦截，放到缓存里面，当
应用程序再次向服务器请求数据的时候，首先去缓存中找该数据据，如果有直接返回，这样可以大大
提高访问效率。
```

**切面类的配置**

```java
@Around(POINTCUT1)
public Object cache(ProceedingJoinPoint pjp) throws Throwable {

	Object[] args = pjp.getArgs();
	String id = (String)args[0];
	System.out.println(id);
	
	// 使用id从缓存里面查数据
	Object cacheObj = null;

	if(cacheObj == null){
		Object proceed = pjp.proceed(); // 执行目标方法
		
		// 放入缓存
		
		//返回
		return proceed;
	}else{
		return cacheObj;
	}
}
```

# 16、thymeleaf模板引擎

简单说， Thymeleaf 是一个跟 Velocity、FreeMarker 类似的模板引擎，它可以完全替代 JSP 。相较与其他的模板引擎，它有如下三个极吸引人的特点：

1、Thymeleaf 在有网络和无网络的环境下皆可运行，即它可以让美工在浏览器查看页面的静态效果，也可以让程序员在服务器查看带数据的动态页面效果。这是由于它支持 html 原型，然后在 html 标签里增加额外的属性来达到模板+数据的展示方式。浏览器解释 html 时会忽略未定义的标签属性，所以 thymeleaf 的模板可以静态地运行；当有数据返回到页面时，Thymeleaf 标签会动态地替换掉静态内容，使页面动态显示。

2、Thymeleaf 开箱即用的特性。它提供标准和spring标准两种方言，可以直接套用模板实现JSTL、 OGNL表达式效果，避免每天套模板、该jstl、改标签的困扰。同时开发人员也可以扩展和创建自定义的方言。

3、Thymeleaf 提供spring标准方言和一个与 SpringMVC 完美集成的可选模块，可以快速的实现表单绑定、属性编辑器、国际化等功能。

**Spring Boot项目Thymeleaf模板页面存放位置**

![image-20200228224138407](.\typora-user-images\image-20200228224138407.png)

**通过Controller跳转到Thymeleaf的页面**

![image-20200228224227170](.\typora-user-images\image-20200228224227170.png)

![image-20200228224302235](.\typora-user-images\image-20200228224302235.png)

![image-20200228224336034](.\typora-user-images\image-20200228224336034.png)

**测试**

![image-20200228224528174](.\typora-user-images\image-20200228224528174.png)

## Thymeleaf相关语法【了解】

WEB三大作用域对象：request(最小的作用域对象)、session(会话)、application(上下文)

```
1，简单表达式   
　　1、变量的表达式：${...}   取request作用域里面的值、ModelAndView
　　2、选择变量表达式：*{...}
　　3、信息表达式：#{...}		取IOC容器里面的值
　　4、链接URL表达式：@{...}   <a href="user/query.action">       <a th:href="@{user/query.action}"   href、Action、src
2，字面值  th:text
　　1、文本文字：'one text', 'Another one!',…
　　2、文字数量：0, 34, 3.0, 12.3,…
　　3、布尔型常量：true, false
　　4、空的文字：null
　　5、文字标记：one, sometext, main,…
3，文本处理
　　1、字符串并置：+
　　2、文字替换：|The name is ${name}|
4，表达式基本对象
　　1、#ctx：上下文对象
　　2、#vars：上下文变量
　　3、#locale：上下文语言环境
　　4、#httpServletRequest：（只有在Web上下文）HttpServletRequest对象   
　　5、#httpSession:（只有在Web上下文）HttpSession对象。              
用法：<span th:text="${#locale.country}">US</span>.
5，实用工具对象　
#dates： java.util的实用方法。对象:日期格式、组件提取等.
#calendars：类似于#日期,但对于java.util。日历对象
#numbers：格式化数字对象的实用方法。
#strings：字符串对象的实用方法:包含startsWith,将/附加等。
#objects：实用方法的对象。
#bools：布尔评价的实用方法。
#arrays：数组的实用方法。
#lists：list集合。
#sets：set集合。
#maps：map集合。
#aggregates：实用程序方法用于创建聚集在数组或集合.
#messages：实用程序方法获取外部信息内部变量表达式,以同样的方式,因为它们将获得使用# {…}语法
#ids：实用程序方法来处理可能重复的id属性(例如,由于迭代)。
```

**存在问题**

![image-20200228224718047](.\typora-user-images\image-20200228224718047.png)

出现以上的问题是没有正确读取xxx.properties文件里定义的学生信息。

**解决上面的问题，创建I18NConfig**

```java
package com.hjh.thymeleaf.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.support.ResourceBundleMessageSource;

/**
 * @author HJH
 * @date 2020-02-28 21:54
 */
@Configuration
public class I18NConfig {


    @Bean
    public ResourceBundleMessageSource messageSource() {
        //创建个资源绑定的信息源
        ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
        //设置可以使用编码访问
        messageSource.setUseCodeAsDefaultMessage(true);
        //禁用系统本地环境
        messageSource.setFallbackToSystemLocale(false);
        //设置资源文件有前缀
        messageSource.setBasename("app");
        messageSource.setDefaultEncoding("UTF-8");
        // 设置缓存时间
        messageSource.setCacheSeconds(2);
        return messageSource;
    }
}
```

**国际化创建app_zh_CN.properties**(学生信息的中文版，到时会根据系统语言环境读取响应版本的属性文件。)

案例一：

![image-20200229001138044](.\typora-user-images\image-20200229001138044.png)

![image-20200229001150866](.\typora-user-images\image-20200229001150866.png)

![image-20200229001258110](.\typora-user-images\image-20200229001258110.png)

案例二：

![image-20200229001325921](.\typora-user-images\image-20200229001325921.png)

![image-20200229001418081](.\typora-user-images\image-20200229001418081.png)

## script里面取值

![image-20200229121823207](.\typora-user-images\image-20200229121823207.png)

注意：**取的是作用域里面的值**

![image-20200229123329983](.\typora-user-images\image-20200229123329983.png)

![image-20200229123355431](.\typora-user-images\image-20200229123355431.png)

## 取三大作用域对象的值

![image-20200229124203697](.\typora-user-images\image-20200229124203697.png)

![image-20200229124210891](.\typora-user-images\image-20200229124210891.png)

**注意：model也是存在于request作用域**

session取值三种方法：

![image-20200229124356667](.\typora-user-images\image-20200229124356667.png)

![image-20200229124406238](.\typora-user-images\image-20200229124406238.png)

**链接传值**

![image-20200229140523140](.\typora-user-images\image-20200229140523140.png)

![image-20200229140534523](.\typora-user-images\image-20200229140534523.png)

![image-20200229141018081](.\typora-user-images\image-20200229141018081.png)

![image-20200229141838310](.\typora-user-images\image-20200229141838310.png)

# 17、管理及扩展springmvc组件

## 17.1前端控制器的自动管理

找到WebMvcAutoConfiguration

![image-20200229160351827](.\typora-user-images\image-20200229160351827.png)

找到DispatcherServletAutoConfiguration类

![image-20200229160532651](.\typora-user-images\image-20200229160532651.png)

以上的配置是创建前端控制器对象

![image-20200229160556673](.\typora-user-images\image-20200229160556673.png)

以上的配置是进行注册

## 17.2控制器的自动管理

直接扫描 @CompmentScan(basepackages={})

## 17.3视图解析器的自动管理

查看WebMvcAutoConfiguration

![image-20200229160712691](.\typora-user-images\image-20200229160712691.png)

![image-20200229160717396](.\typora-user-images\image-20200229160717396.png)

contentNegotiationViewResolver是一个管理所有视图解析器的对象

![image-20200229160725293](.\typora-user-images\image-20200229160725293.png)

## 17.4自定义前缀和后缀

![image-20200229160737499](.\typora-user-images\image-20200229160737499.png)

## 17.5文件上传和下载的视图解析器

```
@Configuration(proxyBeanMethods = false)
//必须有这些字节码存在
@ConditionalOnClass({ Servlet.class, StandardServletMultipartResolver.class, MultipartConfigElement.class })
//如果条件都成立，默认开启文件上传的支持
@ConditionalOnProperty(prefix = "spring.servlet.multipart", name = "enabled", matchIfMissing = true)
//必须是一个web应用程序
@ConditionalOnWebApplication(type = Type.SERVLET)
@EnableConfigurationProperties(MultipartProperties.class)
public class MultipartAutoConfiguration {
```

查看MultipartProperties

![image-20200229160854181](.\typora-user-images\image-20200229160854181.png)

配置

![image-20200229160902157](.\typora-user-images\image-20200229160902157.png)

注意文件大的单位必须为MB[大写]

## 17.6消息转化【接收页面参数并转化】

![image-20200229161014750](.\typora-user-images\image-20200229161014750.png)

![image-20200229161017248](.\typora-user-images\image-20200229161017248.png)

## 17.7格式化【接收页面参数并按某种格式格式化--如日期】

![image-20200229161047425](.\typora-user-images\image-20200229161047425.png)

## 17.8欢迎页面的自动配置

**曾经的配置方式；**

![image-20200229161146942](.\typora-user-images\image-20200229161146942.png)

**现在默认为static目录下的index.html**

![image-20200229161158038](.\typora-user-images\image-20200229161158038.png)

## 17.9扩展springmvc的组件

**在容器中注册视图控制器**

当页面跳转时，我们需要在Controller里面创建一个空方法去跳转，那么有没有别的配置方法呢

**创建一个MvcConfig的配置类实现WebMvcConfigurer重写addViewControllers方法**

![image-20200229163052018](.\typora-user-images\image-20200229163052018.png)

对比

![image-20200229163111007](.\typora-user-images\image-20200229163111007.png)

注意：**如果转发的同时还要携带数据，就不能使用前者**

## 17.10注册格式化器【了解】

之前采用：spring.mvc.date-format=yyyy-MM-dd  

![image-20200229201735480](.\typora-user-images\image-20200229201735480.png)

![image-20200229201750736](.\typora-user-images\image-20200229201750736.png)

\*注意：**通过@DateTimeFormat配置的优先级高于配置文件里面的spring.mvc.date-format**

**注册格式化器**

```java
/**
     * 自定义格式化器
     * @param registry
     */
    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addFormatter(new Formatter<Date>() {
            /**
             *
             * @param s ： 为页面传来的值
             * @param locale
             * @return
             * @throws ParseException
             */
            @Override
            public Date parse(String s, Locale locale) throws ParseException {
                SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
                return sdf.parse(s);
            }

            @Override
            public String print(Date date, Locale locale) {
                return null;
            }
        });
    }
```

**17.11消息转化器扩展fastjson**

**第一种@JsonProperty**

Person类

![image-20200229205444171](.\typora-user-images\image-20200229205444171.png)

![image-20200229205450511](.\typora-user-images\image-20200229205450511.png)

测试：

![image-20200229205510679](.\typora-user-images\image-20200229205510679.png)

**扩展fastjson的写法：**

1、加入fastjson的依赖：

2、配置类

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
/**
     * 自定义扩展消息转化器，平时不配，因为有jackson的转化器
     * @param converters
     */
    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        FastJsonHttpMessageConverter fc=new FastJsonHttpMessageConverter();
        FastJsonConfig config=new FastJsonConfig();
        config.setSerializerFeatures(SerializerFeature.PrettyFormat);
        fc.setFastJsonConfig(config);
        converters.add(fc);
    }	
}
```

**注意：fastjson会覆盖jackson的注解**

![image-20200229210851636](.\typora-user-images\image-20200229210851636.png)

![image-20200229210910385](.\typora-user-images\image-20200229210910385.png)

## 17.11注册拦截器【掌握】

**创建拦截器MyInterceptor**

```java
public class MyInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        System.out.println("preHandle");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion");
    }
}
```

**注册拦截器**

以前的配置方法：

```
<interceptors>
	<interceptor></interceptor>
</interceptors>
```

**现在的配置：**

```java
/**
     * 配置拦截器
     * @param registry
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        MyInterceptor myInterceptor = new MyInterceptor(); // <bean
        registry.addInterceptor(myInterceptor)
                .addPathPatterns("/**") // 拦截的路径，可以配置多个
                .excludePathPatterns("/index/get","/index/getDate*"); // 放行的路径，也可以配置多个
    }
```

**测试**

没放行的结果

![image-20200229213401227](.\typora-user-images\image-20200229213401227.png)

放行的结果

![image-20200229213615235](.\typora-user-images\image-20200229213615235.png)

注意：如果放行了还执行了拦截器里面的方法的话，就是发生了异常报500错误，然后再发一次请求，就会执行拦截器里面的方法了，此时需要放行/error

```java
/**
     * 配置拦截器
     * @param registry
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        MyInterceptor myInterceptor = new MyInterceptor(); // <bean
        registry.addInterceptor(myInterceptor)
                .addPathPatterns("/**") // 拦截的路径，可以配置多个
                .excludePathPatterns("/index/get","/index/getDate*","/error"); // 放行的路径，也可以配置多个
    }
```

**程序先执行拦截器的preHandle(),再执行方法，再执行拦截器的postHandle（）、afterCompletion（）**

# 18、(熟悉)内嵌WEB服务器加载原理【难点】

**服务器的相关配置**

![image-20200229233650836](.\typora-user-images\image-20200229233650836.png)

## 原理

查看ServletWebServerFactoryAutoConfiguration 配置类

![image-20200229233926072](.\typora-user-images\image-20200229233926072.png)

![image-20200229233940059](.\typora-user-images\image-20200229233940059.png)

![image-20200229233951599](.\typora-user-images\image-20200229233951599.png)

![image-20200229234042694](.\typora-user-images\image-20200229234042694.png)

# 19、【熟悉】启动内嵌Jetty服务器

![image-20200229234419108](.\typora-user-images\image-20200229234419108.png)

排除tomcat

```xml
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
            <!--排除tomcat-->
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
		</dependency>
```

