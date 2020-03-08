[TOC]

# 1、Spring的概述

## 1.1名称说明

**spring** ：春天   程序员的春天

​	|--springmvc 

​	|--springboot

​	|--springclould

​	|--spring data

## 1.2spring的核心

**spring就是一个容器**

​	IOC  /DI   

​	|-- IOC全称为：Inverse of Control

​	|--DI 依赖注入）：全称为Dependency Injection

​	AOP

​		|-- AOP为Aspect Oriented Programming的缩写  面向切面编程

AOP：面向切面编程。扩展功能而不修改源代码。

IOC：控制反转，降低耦合度。如果调用一个类中的方法，需要new对象然后才可以调用；在spring的IoC种，可以将代码new对象的操作交给spring的配置文件来完成。



l Spring在JavaWeb三层结果中，每一层都提供了不同的解决技术。

l Web层：SpringMVC

l Service层：Spring ioc

l Dao层：Spring jdbc Template ==> mybatis



## 1.3spring框架图

![图片1](img\图片1.png)

# 2、传统的javaEE的三层写法

数据访问层   

​		|--jdbc  hibernate  com.sxt.dao.impl

​		|--mybtais    com.sxt.mapper  com.sxt.mapping[可以和mapper合并]

业务层 

​		|--service  

​				|--com.sxt.service.impl 

​				|--com.sxt.biz.impl

WEB层

​		|--web层   com.sxt.servlet

​					 com.sxt.action

​					 com.sxt.controller

实体类

​		|-- jdbc  hibernate  com.sxt.bean  com.sxt.entity  com.sxt.pojo

​		|--mybtais  com.sxt.pojo  com.sxt.domain





# 5、Spring属性注入配置详解

创建Person

```java
package com.hjh.domain;

import java.util.List;

public class Person {

	private List<String> strList;
	private List<User> userList;

	public List<String> getStrList() {
		return strList;
	}

	public void setStrList(List<String> strList) {
		this.strList = strList;
	}

	public List<User> getUserList() {
		return userList;
	}

	public void setUserList(List<User> userList) {
		this.userList = userList;
	}

}
```

# 2.2cglib动态代理

特点：目标对象没有实现接口也可以进行代理

```
上面的静态代理和动态代理模式都是要求目标对象是实现一个接口的目标对象,但是有时候目标对象只是一个单独的对象,并没有实现任何的接口,这个时候就可以使用以目标对象子类的方式类实现代理,这种方法就叫做:Cglib代理

Cglib代理,也叫作子类代理,它是在内存中构建一个子类对象从而实现对目标对象功能的扩展.

JDK的动态代理有一个限制,就是使用动态代理的对象必须实现一个或多个接口,如果想目标没有实现接口的类,就可以使用Cglib实现.


Cglib是一个强大的高性能的代码生成包,它可以在运行期扩展java类与实现java接口.它广泛的被许多AOP的框架使用,例如Spring AOP和synaop,为他们提供方法的interception(拦截)


Cglib包的底层是通过使用一个小而块的字节码处理框架ASM来转换字节码并生成新的类.不鼓励直接使用ASM,因为它要求你必须对JVM内部结构包括class文件的格式和指令集都很熟悉.


Cglib子类代理实现方法:


1.需要引入cglib的jar文件,但是Spring的核心包中已经包括了Cglib功能,所以直接引入spring-core-3.2.5.jar即可.


2.引入功能包后,就可以在内存中动态构建子类


3.代理的类不能为final,否则报错


4.目标对象的方法如果为final/static,那么就不会被拦截,即不会执行目标对象额外的业务方法.
```

mybatis

```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder.build("/mybatis.cfg.xml");

SqlSession sqlSession = factory.openSession();

//通过cglib获取到代理对象
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
```

cglib的实现原理(继承)：在内存当中构造一个目标对象的子类对象，返回一个目标对象的子类对象代理对象

案例

1、创建项目并导包

asm-4.2.jar

cglib-3.1.jar

2、创建目标类

```java
package com.hjh.dao.impl;


public class PersonDaoImpl {

	public void update() {
		System.out.println("正在更新中");
	}

}

```

```java
package com.hjh.dao.impl;


public class UserDaoImpl {

	public void save() {
		System.out.println("保存过程中~~~");
	}

	public void delete() {
		System.out.println("删除过程");
	}

	public void update() {
		System.out.println("正在更新中");
	}
}
```

3、创建代理工厂

```java
package com.hjh.proxy;

import java.lang.reflect.Method;

import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

/**
 * 代理工厂
 * @author hjh
 *
 */
public class ProxyFactory implements MethodInterceptor {
	/**
	 * 目标对象
	 */
	private Object target;
	
	/**
	 * 初始化目标对象
	 * @param target
	 */
	public ProxyFactory(Object target) {
		this.target = target;
	}

	/**
	 * 获取代理对象的方法
	 */
	public Object getProxyInstance() {
		// 1、创建一个子类对象的构造器
		Enhancer enhancer = new Enhancer();
		
		// 2、设置父类
		enhancer.setSuperclass(target.getClass());
		
		// 3、设置回调，就是当前的工厂对象      -- 去调用拦截的方法     -- this => new ProxyFactory();
		enhancer.setCallback(this);
		
		// 4 、在内存里面生成代理对象
		return enhancer.create();
		
	}

	/**
	 *  拦截的方法
	 * arg0 : 目标对象
	 * arg1: 目标对象的方法
	 * arg2:参数
	 * arg3:代理的方法(cglib本质是继承，继承了的方法)
	 */
	@Override
	public Object intercept(Object arg0, Method arg1, Object[] arg2, MethodProxy arg3) throws Throwable {
		startTransaction();
		Object invoke = arg1.invoke(target, arg2);
		endTransaction();
		return invoke;
	}
	
	/*
	 * 开启前置增强
	 */
	public void startTransaction() {
		System.out.println("开启事务");
	}
	
	/*
	 * 开启后置增强
	 */
	public void endTransaction() {
		System.out.println("提交事务");
		System.out.println("关闭资源");
	}
}

```

4、测视类

```java
package com.hjh.test;

import com.hjh.dao.impl.UserDaoImpl;
import com.hjh.proxy.ProxyFactory;

public class TestUser {
	public static void main(String[] args) {
		// 1、创建目标对象
		UserDaoImpl userDaoImpl = new UserDaoImpl();
		
		// 2、创建代理工厂
		ProxyFactory proxyFactory = new ProxyFactory(userDaoImpl);
		
	    // 3、创建代理对象
		//Object proxyInstance = proxyFactory.getProxyInstance();
		UserDaoImpl impl = (UserDaoImpl) proxyFactory.getProxyInstance();
		
		//System.out.println(proxyInstance.getClass().getSimpleName()); // UserDaoImpl$$EnhancerByCGLIB$$1758e3be
		impl.delete();
	}
}

```

总结

```
在Spring的AOP编程中:

如果加入容器的目标对象有实现接口,用JDK代理

如果目标对象没有实现接口,用Cglib代理
```

# 3、普通AOP开发(XML方式)

AOP目的：对类里面的方法进行加强

如果要对com.hjh.service.iml进行增强，那么这个包就是切面

![aop](E:\chm\img\aop.png)

案例

1、创建项目并导包

![image-20200306203410451](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200306203410451.png)

2、创建Man

```java
package com.hjh.domain;
/**
 *  目标类
 * @author hjh
 *
 */
public class Man {
	
	public void eat() {
		System.out.println("人吃饭");
	}
	
}
```

3、配置ApplicationContext.xml

导入头文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
	
	
</beans>
```

4、前置通知

4.1创建MyBeforeAdvice

```java
package com.hjh.advice;

import java.lang.reflect.Method;

import org.springframework.aop.BeforeAdvice;
import org.springframework.aop.MethodBeforeAdvice;
/**
 * 	前置增强
 * @author hjh
 *
 */
public class MyBeforeAdvice implements MethodBeforeAdvice {

	/**
	 * arg0：目标方法
	 * arg1：方法参数
	 * arg2：目标对象
	 */
	@Override
	public void before(Method arg0, Object[] arg1, Object arg2) throws Throwable {
		before();
	}
	
	/*
	 * 开启前置增强
	 */
	public void before() {
		System.out.println("前置增强");
	}
	
	/*
	 * 开启后置增强
	 */
	/*
	 * public void endTransaction() { System.out.println("提交事务");
	 * System.out.println("关闭资源"); }
	 */
	
}

```

4.2配置ApplicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

	<!-- 声明目标对象 -->
	<bean id="man" class="com.hjh.domain.Man"></bean>

	<!-- 声明通知对象 -->
	<bean id="myBeforeAdvice" class="com.hjh.advice.MyBeforeAdvice"></bean>
	
	<!-- 进行AOP的配置 
	 		expression：表达式 
	 -->
	<aop:config>
		<!-- 声明一个切面 -->
		<aop:pointcut expression="execution(* com.hjh.domain.*.*(..))" id="pc1"/>
		
		<!-- 织入操作 
			advice-ref:通知对象
			pointcut-ref：织入到哪一个切面
		-->
		<aop:advisor advice-ref="myBeforeAdvice" pointcut-ref="pc1"/>
	</aop:config>

</beans>
```

4.3测试类

```java
package com.hjh.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.hjh.domain.Man;

public class TestUser {
	public static void main(String[] args) {
		// 1、加载spring的配置文件，初始化IOC容器
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		
		// 2、获取代理对象
		Man man = applicationContext.getBean(Man.class);
		//System.out.println(man.getClass().getSimpleName()); // Man$$EnhancerBySpringCGLIB$$3dbcc30f
		man.eat();
	}
}
```

5、后置通知

创建MyAfterAdvice

```java
package com.hjh.advice;

import java.lang.reflect.Method;

import org.springframework.aop.AfterAdvice;
import org.springframework.aop.AfterReturningAdvice;
import org.springframework.aop.BeforeAdvice;
import org.springframework.aop.MethodBeforeAdvice;

/**
 *	后置增强
 * 
 * @author hjh
 *
 */
public class MyAfterAdvice implements AfterReturningAdvice {

	/**
	 * arg03：当前被加强的目标对象arg1：目标方法 arg2：方法参数 
	 */
	@Override
	public void afterReturning(Object arg0, Method arg1, Object[] arg2, Object arg3) throws Throwable {
		after();
	}

	/*
	 * 开启前置增强
	 */
	/*
	 * public void before() { System.out.println("饭前搞一杯！"); }
	 */

	/*
	 * 开启后置增强
	 */
	public void after() {
		System.out.println("饭后看电视~~~");
	}

}

```

6、环绕通知

创建AroundAdvice

```java
package com.hjh.advice;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;

/**
 * 	环绕增强
 * 
 * @author hjh
 *
 */
public class AroundAdvice implements MethodInterceptor {

	/**
	 * arg0:拦截的方法
	 */
	@Override
	public Object invoke(MethodInvocation arg0) throws Throwable {
		before();
		Object proceed = arg0.proceed();
		after();
		return proceed;
	}

	/*
	 * 开启前置增强
	 */
	public void before() {
		System.out.println("饭前搞一杯！");
	}

	/*
	 * 开启后置增强
	 */
	public void after() {
		System.out.println("饭后看电视~~~");
	}

}

```

7、异常通知

创建MyThrows

```java
package com.hjh.advice;

import org.springframework.aop.ThrowsAdvice;

/**
 *  异常的增强类
 * @author hjh
 *
 */
public class MyThrows implements ThrowsAdvice {
	
	/**
	 *	 接收目标方法发生的异常
	 * @param throwable
	 */
	public void afterThrowing(Throwable throwable) throws Throwable {
		System.out.println("发生的异常："+throwable.getMessage());
	}
	
}
```

**注意：此处的方法名必须是afterThrowing**

**8、配置ApplicationContext.xml （重点）**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

	<!-- 声明目标对象 -->
	<bean id="man" class="com.hjh.domain.Man"></bean>

	<!-- 声明通知对象 -->
	<bean id="myBeforeAdvice" class="com.hjh.advice.MyBeforeAdvice"></bean>
	<bean id="myAfterAdvice" class="com.hjh.advice.MyAfterAdvice"></bean>
	<bean id="aroundAdvice" class="com.hjh.advice.AroundAdvice"></bean>
	<bean id="myThrows" class="com.hjh.advice.MyThrows"></bean>
	
	<!-- 进行AOP的配置 
	 		expression：表达式 
	 -->
	<aop:config>
		<!-- 声明一个切面 -->
		<aop:pointcut expression="execution(* com.hjh.domain.*.*(..))" id="pc1"/>
		
		<!-- 织入操作 
			advice-ref:通知对象
			pointcut-ref：织入到哪一个切面
		-->
		<!-- <aop:advisor advice-ref="myBeforeAdvice" pointcut-ref="pc1"/> -->
		<!-- <aop:advisor advice-ref="myAfterAdvice" pointcut-ref="pc1"/> -->
		<!-- <aop:advisor advice-ref="aroundAdvice" pointcut-ref="pc1"/> -->
		<aop:advisor advice-ref="myThrows" pointcut-ref="pc1"/>
	</aop:config>

</beans>
```

9、测试类

```java
package com.hjh.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.hjh.domain.Man;

public class TestUser {
	public static void main(String[] args) {
		// 1、加载spring的配置文件，初始化IOC容器
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		
		// 2、获取代理对象
		Man man = applicationContext.getBean(Man.class);
		//System.out.println(man.getClass().getSimpleName()); // Man$$EnhancerBySpringCGLIB$$3dbcc30f
		man.eat();
	}
}

```

监控方法的执行时间：

```java
package com.hjh.advice;

import java.util.Date;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;

/**
 * 	环绕增强
 * 
 * @author hjh
 *
 */
public class AroundAdvice implements MethodInterceptor {

	/**
	 * arg0:拦截的方法
	 */
	@Override
	public Object invoke(MethodInvocation arg0) throws Throwable {
		before();
		long startTime = System.currentTimeMillis();
		Object proceed = arg0.proceed();
		long endTime = System.currentTimeMillis();
		System.out.println("监控方法执行耗时："+(endTime - startTime) + "ms");
		after();
		return proceed;
	}

	/*
	 * 开启前置增强
	 */
	public void before() {
		System.out.println("饭前搞一杯！");
	}

	/*
	 * 开启后置增强
	 */
	public void after() {
		System.out.println("饭后看电视~~~");
	}

}
```

# 4、使用AspectJ AOP开发(XML方式)

**什么是aspectJ**

AspectJ是一个基于Java语言的AOP框架，Spring2.0开始引入对AspectJ的支持，AspectJ扩展了Java语言，提供了专门的编译器，在编译时提供了横向代码的注入。

@AspectJ是AspectJ1.5新增功能，通过JDK1.5注解技术，允许直接在Bean中定义切面

新版本的Spring中，建议使用AspectJ方式开发AOP

**aspectJ开发两种方式**

​	  基于xml配置文件方式开发

  	基于注解方式的开发

**创建增强类(通知类)**

```java
package com.hjh.advice;

import org.aspectj.lang.ProceedingJoinPoint;

public class MyAdvice {
	/**
	 * 	前置通知
	 */
	public void before() {
		System.out.println("饭前玩游戏");
	}
	
	/**
	 * 	后置通知
	 */
	public void after() {
		System.out.println("饭后踢足球");
	}
	
	/**
	 * 	环绕通知
	 */
	public Object around(ProceedingJoinPoint joinPoint) {
		Object object = null;
		before();
		try {
			object = joinPoint.proceed();
		} catch (Throwable e) {
			e.printStackTrace();
		}
		after();
		return object;
	}
	
	/**
	 * 异常通知
	 */
	public void throwing(Throwable throwable) {
		System.out.println("异常："+throwable.getMessage());
	}
	
}
```



**创建目标类**

```java
package com.hjh.domain;
/**
 *  目标类
 * @author hjh
 *
 */
public class Man {
	
	public void eat() {
		System.out.println("人吃饭");
		int a = 1/0;
	}
	
}

```

**配置applicationContext.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

	<!-- 声明目标对象 -->
	<bean id="man" class="com.hjh.domain.Man"></bean>

	<!-- 声明通知对象 -->
	<bean id="myAdvice" class="com.hjh.advice.MyAdvice"></bean>
	
	<!-- 进行AOP的配置 
	 		expression：表达式 
	 -->
	<aop:config>
		<aop:aspect ref="myAdvice">
			<!-- 声明切面 -->
			<aop:pointcut expression="execution(* com.hjh.domain.*.*(..))" id="pc1"/>
			
			<!-- 织入操作 -->
			<!-- 前置通知 
				method ： 方法
			-->
			<!-- <aop:before method="before" pointcut-ref="pc1"/>
			<aop:after-returning method="after" pointcut-ref="pc1"/> -->
			<!-- <aop:around method="around" pointcut-ref="pc1"/> -->
			
			<!-- throwing的值必须和增强类里面方法的参数一致 -->
			<aop:after-throwing method="throwing" pointcut-ref="pc1" throwing="throwable"/>
		</aop:aspect>
	</aop:config>

</beans>
```

**测试类**

```java
package com.hjh.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.hjh.domain.Man;

public class TestUser {
	public static void main(String[] args) {
		// 1、加载spring的配置文件，初始化IOC容器
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		
		// 2、获取代理对象
		Man man = applicationContext.getBean(Man.class);
		//System.out.println(man.getClass().getSimpleName()); // Man$$EnhancerBySpringCGLIB$$3dbcc30f
		man.eat();
	}
}

```

# 5、AspectJ  AOP注解开发

创建目标类

```java
package com.hjh.domain;

import org.springframework.stereotype.Component;

/**
 *  目标类
 * @author hjh
 *
 */
@Component
public class Man {
	
	public void eat() {
		System.out.println("人吃饭");
		int a = 1/0;
	}
	
}
```

创建通知类

```java
package com.hjh.advice;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Component // 让spring容器帮我们创建对象   类似以前配置文件中的 <bean id="" class="" />
@Aspect // 让spring扫描的时候知道当前类是一个切面类
public class MyAdvice {
	
	/**
	 * 	声明一个切面
	 */
	public static final String POIN_CUT = "execution(* com.hjh.domain.*.*(..))";
	
	/**
	 * 	前置通知
	 */
	//@Before(POIN_CUT)
	public void before() {
		System.out.println("饭前玩游戏");
	}
	
	/**
	 * 	后置通知
	 */
	//@After(POIN_CUT)
	public void after() {
		System.out.println("饭后踢足球");
	}
	
	/**
	 * 	环绕通知
	 */
	//@Around(POIN_CUT)
	public Object around(ProceedingJoinPoint joinPoint) {
		Object object = null;
		before();
		try {
			object = joinPoint.proceed();
		} catch (Throwable e) {
			e.printStackTrace();
		}
		after();
		return object;
	}
	
	/**
	 * 异常通知
	 */
	@AfterThrowing(value = POIN_CUT,throwing = "throwable")
	public void throwing(Throwable throwable) {
		System.out.println("异常："+throwable.getMessage());
	}
	
}

```

配置applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

	<!-- 开启注解扫描 -->
	<context:component-scan base-package="com.hjh.domain,com.hjh.advice" />

	<!-- 切面自动代理 -->
	<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

测试类

```java
package com.hjh.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.hjh.domain.Man;

public class TestUser {
	public static void main(String[] args) {
		// 1、加载spring的配置文件，初始化IOC容器
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		
		// 2、获取代理对象
		Man man = applicationContext.getBean(Man.class);
		//System.out.println(man.getClass().getSimpleName()); // Man$$EnhancerBySpringCGLIB$$3dbcc30f
		man.eat();
	}
}

```

# 6、Spring-JdbcTemplate【重点】

```
什么是“持久化”
持久（Persistence），即把数据（如内存中的对象）保存到可永久保存的存储设备中（如磁盘）。持久化的主要应用是将内存中的数据存储在关系型的数据库中，当然也可以存储在磁盘文件中、XML数据文件中等等。
什么是“持久层”
持久层（Persistence Layer），即专注于实现数据持久化应用领域的某个特定系统的一个逻辑层面，将数据使用者和数据实体相关联。
什么是ORM
即Object-Relationl Mapping，它的作用是在关系型数据库和对象之间作一个映射，这样，我们在具体的操作数据库的时候，就不需要再去和复杂的SQL语句打交道，只要像平时操作对象一样操作它就可以了 。
为什么要做持久化和ORM设计(重要)
在目前的企业应用系统设计中，MVC，即 Model（模型）- View（视图）- Control（控制）为主要的系统架构模式。MVC 中的 Model 包含了复杂的业务逻辑和数据逻辑，以及数据存取机制（如 JDBC的连接、SQL生成和Statement创建、还有ResultSet结果集的读取等）等。将这些复杂的业务逻辑和数据逻辑分离，以将系统的紧耦 合关系转化为松耦合关系（即解耦合），是降低系统耦合度迫切要做的，也是持久化要做的工作。MVC 模式实现了架构上将表现层（即View）和数据处理层（即Model）分离的解耦合，而持久化的设计则实现了数据处理层内部的业务逻辑和数据逻辑分离的解耦合。而 ORM 作为持久化设计中的最重要也最复杂的技术，也是目前业界热点技术。
```

创建项目并导包

![image-20200307162811374](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200307162811374.png)

测试类

```java
package com.hjh.test;

import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

public class TsetJdbc {
	public static void main(String[] args) {
		/**
		 * 	连接数据库的步骤：
		 * 		1、创建数据源对象
		 * 		2、创建JdbcTemplate
		 * 		3、完成CRUD
		 */
		// 1、创建数据源对象
		DriverManagerDataSource dataSource = new DriverManagerDataSource();
		
		// 2、设置连接的参数    driver 、 url 、username、password
		dataSource.setDriverClassName("com.mysql.jdbc.Driver");
		dataSource.setUrl("jdbc:mysql://localhost:3306/vue");
		dataSource.setUsername("root");
		dataSource.setPassword("hujiahao");
		//System.out.println(dataSource);
		
		// 3、创建JdbcTemplate
		JdbcTemplate jdbcTemplate = new JdbcTemplate();
		
		// 4、让JdbcTemplate关联数据源
		jdbcTemplate.setDataSource(dataSource);
		
		// 5、创建sql
		String sql = "select count(*) from user";
		//"select name from user where id = 1"  // 可以使用queryForObject(sql, Long.class)
		//"select * from user where id = 1"  // 不可以使用queryForObject(sql, Long.class)
		
		// 查询操作
		Long queryForObject = jdbcTemplate.queryForObject(sql, Long.class);
		System.out.println(queryForObject);	
		
		// 添加操作
//		String insertSQL = "insert into department(id,name) values(6,'yyb')";
//		jdbcTemplate.update(insertSQL);
//		System.out.println("添加成功");
		
		String insertSQL = "insert into department(id,name) values(?,?)";
		Object[] objects = {6,"qqqq"};  // 可变参数的本质就是数组
		//jdbcTemplate.update(insertSQL,"6","qqq");
		jdbcTemplate.update(insertSQL, objects);
		System.out.println("添加成功");
		
		/**
		 * 	update : 做添加、修改、删除
		 * 	queryForObject : 查询某个对象(sql执行出来的结果，必须是单行单列的)
		 *	queryForList : 查询返回一个集合
		 */
	}
}
```

**通过xml配置数据源和JdbcTemplate**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

	<!-- 声明数据源 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<!-- 注入相关属性 -->
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://localhost:3306/vue"></property>
		<property name="username" value="root"></property>
		<property name="password" value="hujiahao"></property>
	</bean>
	
	<!-- 声明JdbcTemplate -->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<!-- 注入数据源 -->
		<property name="dataSource" ref="dataSource"></property>
	</bean>
</beans>
```

测试类

```java
package com.hjh.test;

import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

public class TsetJdbc2 {
	public static void main(String[] args) {
		
		// 加载spring的IOC容器
		ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		
		// 获取jdbcTemplate对象
		JdbcTemplate jdbcTemplate = (JdbcTemplate) applicationContext.getBean("jdbcTemplate");
		
		String sql = "select count(*) from user";		
		// 查询操作
		Long queryForObject = jdbcTemplate.queryForObject(sql, Long.class);
		System.out.println(queryForObject);	
	}
}

```

# 7、Spring-JdbcTemplate+重构三层结构

创建项目并导包

![image-20200307162828821](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200307162828821.png)

创建Dept

```java
package com.hjh.domain;

public class Dept {
	private Integer id;
	private String name;

	public Dept(String name) {
		super();
		this.name = name;
	}

	public Dept(Integer id, String name) {
		super();
		this.id = id;
		this.name = name;
	}

	public Integer getId() {
		return id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	@Override
	public String toString() {
		return "Dept [id=" + id + ", name=" + name + "]";
	}

}

```

创建DeptDao

```java
package com.hjh.dao;

import java.util.List;

import com.hjh.domain.Dept;

public interface DeptDao {
	void add(Dept dept);

	void update(Dept dept);

	void delete(Integer id);

	Dept queryById(Integer id);

	List<Dept> queryAll();

	List<Dept> queryByForPage(Integer currentPage, Integer pageSize);
}

```

创建DeptDaoImpl

```java
package com.hjh.dao.impl;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.PreparedStatementSetter;
import org.springframework.jdbc.core.RowMapper;

import com.hjh.dao.DeptDao;
import com.hjh.domain.Dept;

public class DeptDaoImpl implements DeptDao {

	private JdbcTemplate jdbcTemplate;

	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}

	@Override
	public void add(Dept dept) {
		String sql = "insert into department(name) values(?)";
		Object[] args = {dept.getName()};
		jdbcTemplate.update(sql, args);
	}

	@Override
	public void update(Dept dept) {
		String sql = "update department set name=? where id = ?";
		Object[] args = {dept.getName(),dept.getId()};
		jdbcTemplate.update(sql, args);
	}

	@Override
	public void delete(Integer id) {
		String sql = "delete from department where id = ?";
		jdbcTemplate.update(sql, id);
	}

	@Override
	public Dept queryById(Integer id) {
		String sql = "select * from department where id = ?";
		Object[] objs = {id};
		return jdbcTemplate.queryForObject(sql, objs , new RowMapper<Dept>() {

			@Override
			public Dept mapRow(ResultSet arg0, int arg1) throws SQLException {
				int id = arg0.getInt("id");
				String name = arg0.getString("name");
				return new Dept(id, name);
			}
			
		});
	}

	@Override
	public List<Dept> queryAll() {
		String sql = "select * from department";
		return jdbcTemplate.query(sql, new RowMapper<Dept>() {

			@Override
			public Dept mapRow(ResultSet arg0, int arg1) throws SQLException {
				int id = arg0.getInt("id");
				String name = arg0.getString("name");
				return  new Dept(id, name);
			}
			
		});
	}

	@Override
	public List<Dept> queryByForPage(Integer currentPage, Integer pageSize) {
		String sql = "select * from department limit ?,?";
		Object[] args = {(currentPage-1)*pageSize,pageSize};
		return jdbcTemplate.query(sql, args, new RowMapper<Dept>() {

			@Override
			public Dept mapRow(ResultSet arg0, int arg1) throws SQLException {
				int id = arg0.getInt("id");
				String name = arg0.getString("name");
				return  new Dept(id, name);
			}
			
		});
	}
}
```

创建DeptService

```java
package com.hjh.service;

import java.util.List;

import com.hjh.domain.Dept;

public interface DeptService {
	void add(Dept dept);

	void update(Dept dept);

	void delete(Integer id);

	Dept queryById(Integer id);

	List<Dept> queryAll();

	List<Dept> queryByForPage(Integer currentPage, Integer pageSize);
}

```

创建DeptServiceImpl

```java
package com.hjh.service.impl;

import java.util.List;

import com.hjh.dao.DeptDao;
import com.hjh.domain.Dept;
import com.hjh.service.DeptService;

public class DeptServiceImpl implements DeptService {

	private DeptDao deptDao;

	public void setDeptDao(DeptDao deptDao) {
		this.deptDao = deptDao;
	}

	@Override
	public void add(Dept dept) {
		deptDao.add(dept);
	}

	@Override
	public void update(Dept dept) {
		deptDao.update(dept);
	}

	@Override
	public void delete(Integer id) {
		deptDao.delete(id);
	}

	@Override
	public Dept queryById(Integer id) {
		return deptDao.queryById(id);
	}

	@Override
	public List<Dept> queryAll() {
		return deptDao.queryAll();
	}

	@Override
	public List<Dept> queryByForPage(Integer currentPage, Integer pageSize) {
		return deptDao.queryByForPage(currentPage, pageSize);
	}
}

```

创建DeptController

```java
package com.hjh.controller;

import java.util.List;

import com.hjh.domain.Dept;
import com.hjh.service.DeptService;

public class DeptController {

	private DeptService deptService;

	public void setDeptService(DeptService deptService) {
		this.deptService = deptService;
	}
	
	/**
	 * 	添加操作
	 */
	public void add() {
		Dept dept = new Dept("first");
		deptService.add(dept);
		System.out.println("添加成功");
	}
	
	/**
	 * 	修改操作
	 */
	public void update() {
		Dept dept = new Dept(7,"ppp");
		deptService.update(dept);
		System.out.println("修改成功");
	}
	
	/**
	 * 	删除操作
	 */
	public void delete() {
		deptService.delete(6);
		System.out.println("删除成功");
	}
	
	/**
	 * 	根据ID查询操作
	 */
	public void queryById() {
		Dept dept = deptService.queryById(5);
		System.out.println("查询一条成功");
		System.out.println(dept);
	}
	
	/**
	 * 	查询所有操作
	 */
	public void queryAll() {
		List<Dept> queryAll = deptService.queryAll();
		for (Dept dept2 : queryAll) {
			System.out.println(dept2);
		}
	}
	
	/**
	 * 	分页查询
	 */
	public void queryByForPage(Integer currentPage,Integer pageSize) {
		List<Dept> queryAll = deptService.queryByForPage(currentPage,pageSize);
		for (Dept dept : queryAll) {
			System.out.println(dept);
		}
	}

}

```

配置applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

	<!-- 声明数据源 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<!-- 注入相关属性 -->
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://localhost:3306/vue"></property>
		<property name="username" value="root"></property>
		<property name="password" value="hujiahao"></property>
	</bean>
	
	<!-- 声明JdbcTemplate -->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<!-- 注入数据源 -->
		<property name="dataSource" ref="dataSource"></property>
	</bean>
	
	<!-- 声明Dao -->
	<bean id="deptDao" class="com.hjh.dao.impl.DeptDaoImpl">
		<property name="jdbcTemplate" ref="jdbcTemplate"></property>
	</bean>
	
	<!-- 声明Service -->
	<bean id="deptService" class="com.hjh.service.impl.DeptServiceImpl">
		<property name="deptDao" ref="deptDao"></property>
	</bean>
	
	<!-- 声明Controller -->
	<bean id="deptController" class="com.hjh.controller.DeptController">
		<property name="deptService" ref="deptService"></property>
	</bean>
</beans>
```

创建测试类

```java
package com.hjh.test;

import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

import com.hjh.controller.DeptController;

public class TsetJdbc3 {
	public static void main(String[] args) {
		
		// 加载spring的IOC容器
		ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		
		// 获取jdbcTemplate对象
		DeptController deptController = (DeptController) applicationContext.getBean("deptController");
		Integer currentPage = 2;
		Integer pageSize = 3;
		//deptController.add();
		//deptController.update();
		//deptController.delete();
		//deptController.queryById();
		//deptController.queryAll();
		deptController.queryByForPage(currentPage, pageSize);
	}
}
```

**使用注解方式的配置applicationContext.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

	<!-- 声明数据源 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<!-- 注入相关属性 -->
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://localhost:3306/vue"></property>
		<property name="username" value="root"></property>
		<property name="password" value="hujiahao"></property>
	</bean>
	
	<!-- 声明JdbcTemplate -->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<!-- 注入数据源 -->
		<property name="dataSource" ref="dataSource"></property>
	</bean>
	
	<!-- 开启注解扫描 -->
	<context:component-scan base-package="com.hjh.dao.impl,com.hjh.service.impl,com.hjh.controller" />
</beans>
```

注意：jdbcTemplate不能通过注解注入dataSource，那样需要在源码里面添加注解，但是事实上源码不能修改。

```
相关方法的说明：
	1、update(String sql,Object...objs)  -- 添加、修改、删除
	2、queryForObject(String sql,Long.class)  -- 查询结果为一个对象
	3、queryForObject(String sql,Object...objs,Long.class) -- 查询结果为一个对象
	4、queryForObject(String sql,Object...objs,RowMapper<T>)  -- 查询结果为一个对象
	5、query(String sql,Object...objs,RowMapper<T>)   -- 查询结果为集合
```



# 8、定时任务

**定时任务概述**

1、Quartz介绍

　　在企业应用中，我们经常会碰到时间任务调度的需求，比如每天凌晨生成前天报表，每小时生成一次汇总数据等等。Quartz是出了名的任务调度框架,它可以与J2SE和J2EE应用程序相结合，功能灰常强大，轻轻松松就能与Spring集成，使用方便。

2、Quartz中的概念

　　主要有三个核心概念：调度器、任务和触发器。三者关系简单来说就是，调度器负责调度各个任务，到了某个时刻或者过了一定时间，触发器触动了，特定任务便启动执行。概念相对应的类和接口有：

　　1）JobDetail：望文生义就是描述任务的相关情况；

　　2）Trigger：描述出发Job执行的时间触发规则。有SimpleTrigger和CronTrigger两个子类代表两种方式，一种是每隔多少分钟小时执行，则用SimpleTrigger；另一种是日历相关的重复时间间隔，如每天凌晨，每周星期一运行的话，通过Cron表达式便可定义出复杂的调度方案。

　　3）Scheduler：代表一个Quartz的独立运行容器，Trigger和JobDetail要注册到Scheduler中才会生效，也就是让调度器知道有哪些触发器和任务，才能进行按规则进行调度任务。

**spring-context-support集成了quartz**

![image-20200307185540619](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200307185540619.png)

## Quarts和Spring的集成(方式一)[了解]

创建任务类

```java
package com.hjh.job;
/**
 * 	任务
 * @author hjh
 *
 */
public class MyJob1 {
	public void doTask() {
		System.out.println("定时任务的第一种实现方式");
	}
}

```

配置xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

	<!-- 声明任务 -->
	<bean id="myJob1" class="com.hjh.job.MyJob1" />
	
	<!-- 包装任务详情 -->
	<bean id="springQuartzJobBean" class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">
		<!-- 注入要执行的目标对象 -->
		<property name="targetObject" ref="myJob1"></property>
		
		<!-- 注入要执行的目标对象的方法 -->
		<property name="targetMethod" value="doTask"></property>
	</bean>
	
	<!-- 配置触发规则 -->
	<bean id="trigger" class="org.springframework.scheduling.quartz.CronTriggerFactoryBean">
		<!-- 注入任务详情 -->
		<property name="jobDetail" ref="springQuartzJobBean"></property>
		
		<!-- 定义触发规则 -->
		<property name="cronExpression" value="0/5 * * * * ? "></property>
	</bean>
	
	<!-- 配置调度器 -->
	<bean id="scheduler" class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
		<property name="triggers">
			<array>
				<ref bean="trigger"/>
			</array>
		</property>
	</bean>
</beans>
```

测试

```java
package com.hjh.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestJob {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
	}
}
```

## Quarts和Spring的集成(方式二)[掌握]

创建任务类

```java
package com.hjh.job;

import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;
import org.springframework.scheduling.quartz.QuartzJobBean;

/**
 * 	任务
 * @author hjh
 *
 */
public class MyJob2 extends QuartzJobBean {

	@Override
	protected void executeInternal(JobExecutionContext context) throws JobExecutionException {
		System.out.println("定时任务的第一种实现方式");
	}
	
}
```

配置xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

	
	<!-- 声明任务详情 -->
	<bean id="springQuartzJobBean" class="org.springframework.scheduling.quartz.JobDetailFactoryBean">
		<!-- 注入要执行的目标类 -->
		<property name="jobClass" value="com.hjh.job.MyJob2"></property>
		
		<!-- 是否重复执行 -->
		<property name="durability" value="true"></property>
	</bean>
	
	<!-- 配置触发规则 -->
	<bean id="trigger" class="org.springframework.scheduling.quartz.CronTriggerFactoryBean">
		<!-- 注入任务详情 -->
		<property name="jobDetail" ref="springQuartzJobBean"></property>
		
		<!-- 定义触发规则 -->
		<property name="cronExpression" value="0/5 * * * * ? "></property>
	</bean>
	
	<!-- 配置调度器 -->
	<bean id="scheduler" class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
		<property name="triggers">
			<array>
				<ref bean="trigger"/>
			</array>
		</property>
	</bean>
</beans>
```

测试类

```java
package com.hjh.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestJob2 {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext2.xml");
	}
}
```

## Quarts和Spring的集成(方式三)(注解开发)[掌握]

创建任务类

```java
package com.hjh.job;

import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

/**
 * 	任务
 * @author hjh
 *
 */
@Component
public class MyJob3 {
	@Scheduled(cron = "0/6 * * * * ? ")
	public void doTask() {
		System.out.println("定时任务的第3一种实现方式");
	}
	
	@Scheduled(cron = "0/2 * * * * ? ")
	public void eat() {
		System.out.println("吃饭~~~");
	}
}

```

配置xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:task="http://www.springframework.org/schema/task"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
	    http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
	    http://www.springframework.org/schema/task
	    http://www.springframework.org/schema/task/spring-task.xsd">
        
	<context:component-scan base-package="com.hjh.job" />
	
	<!-- 加载定时任务 -->
	<task:annotation-driven/>
	
</beans>
```

![image-20200307205633074](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200307205633074.png)

测试类

```java
package com.hjh.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestJob3 {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext3.xml");
	}
}
```

注意：日志报错处理

```
log4j.properties中添加：
log4j.logger.org.springframework.scheduling=INFO

分析：
如果ScheduledExecutorService没有找到，就使用默认的调度器。所以，这个过程中如果没有找到ScheduledExecutorService，就会在debug级别的日志输出一个异常
```

# 9、配置Druid数据源

修改配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

	<!-- 声明数据源 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<!-- 注入相关属性 -->
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://localhost:3306/vue2"></property>
		<property name="username" value="root"></property>
		<property name="password" value="hujiahao"></property>
	</bean>
	
	<!-- 读取配置文件 -->
	<context:property-placeholder location="classpath:db.properties" />
	
	<bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
		<!-- 注入相关属性 -->
		<property name="driverClassName" value="${driver}"></property>
		<property name="url" value="${url}"></property>
		<property name="username" value="${username}"></property>
		<property name="password" value="${password}"></property>
	</bean>
	
	<!-- 声明JdbcTemplate -->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<!-- 注入数据源 -->
		<property name="dataSource" ref="druidDataSource"></property>
	</bean>
	
	<!-- 开启注解扫描 -->
	<context:component-scan base-package="com.hjh.dao.impl,com.hjh.service.impl,com.hjh.controller" />
</beans>
```

创建db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/vue2
username=root
password=hujiahao
```

测试类

```java
package com.hjh.test;

import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

import com.hjh.controller.DeptController;

public class TsetJdbc3 {
	public static void main(String[] args) {
		
		// 加载spring的IOC容器
		ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		
		// 获取jdbcTemplate对象
		DeptController deptController = (DeptController) applicationContext.getBean("deptController");
		Integer currentPage = 2;
		Integer pageSize = 3;
		//deptController.add();
		//deptController.update();
		//deptController.delete();
		//deptController.queryById();
		deptController.queryAll();
		//deptController.queryByForPage(currentPage, pageSize);
	}
}
```

结果：

![image-20200307224622543](.\typora-user-images\image-20200307224622543.png)

# 10、spring和mybatis的集成

## spring和mybatis的集成【XML】

创建application-action.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
		http://www.springframework.org/schema/tx/spring-tx.xsd
        ">
	<bean id="userAction" class="com.sxt.action.UserAction" autowire="byType">
		<!-- <property name="userService" ref="userService"></property> -->
	</bean>
</beans>
```

创建application-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
		http://www.springframework.org/schema/tx/spring-tx.xsd
        ">

	<!-- 创建数据源 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/0412user"></property>
		<property name="username" value="root"></property>
		<property name="password" value="123456"></property>
	</bean>
	<!-- 还可以使用c3p0 -->
	
	<!-- 还可以使用dbcp -->
	<!-- 实例化SqlSessionFactory -->
	<bean id="sqlSessionFactory" name="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 注入dataSource -->
		<property name="dataSource" ref="dataSource"></property>
		<!-- 加载 mybtais.cfg.xml -->
		<!-- <property name="configLocation" value="classpath:mybatis.cfg.xml"></property> -->
		<!-- 如果不想使用mybatis.cfg,xml可以使用以下配置 -->
		<property name="mapperLocations">
			<array>
				<value>classpath:com/sxt/mapping/*.xml</value>
			</array>
		</property>
		
		<!-- 配置插件 -->
		<!-- <property name="plugins">
			<array>
				<bean class="com.github.pagehelper.PageHelper"></bean>
			</array>
		</property> -->
	</bean>
	
	<!-- Mapper接口所在包名，Spring会自动查找之中的类 根据Mapper接口  -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- 提供Mapper接口所在以包 如果有多个包，可以使用,去隔开-->
		<property name="basePackage" value="com.sxt.mapper"></property>
		<!-- 注入sessionFactory -->
		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
	</bean>
	
</beans>
```

创建application-service.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
		http://www.springframework.org/schema/tx/spring-tx.xsd
        ">

	<!-- 扫描 -->
	<!-- <context:component-scan base-package="com.sxt.service.impl"></context:component-scan> -->
	<!-- 配置声明式事物 -->
	<!-- 1,声明事务管理器 -->
	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>

	<!-- 2,声明事务事务传播特性 【哪些方法加事务。哪此不加】 -->
	<tx:advice id="txAdvise" transaction-manager="transactionManager">
		<tx:attributes>
			<!-- 在切面里面如查有方法名是add开头的就加事物 -->
			<tx:method name="add*" propagation="REQUIRED" />
			<tx:method name="save*" propagation="REQUIRED" />
			<tx:method name="delete*" propagation="REQUIRED" />
			<tx:method name="del*" propagation="REQUIRED" />
			<tx:method name="update*" propagation="REQUIRED" />
			<tx:method name="load*" propagation="REQUIRED" />
			<tx:method name="get*" propagation="REQUIRED" />
			<tx:method name="find*" read-only="true" />
			<tx:method name="*" read-only="true" />
		</tx:attributes>
	</tx:advice>
	<!-- 3,配置aop -->
	<aop:config>
		<!-- 声明一个切入点 -->
		<aop:pointcut expression="execution(* com.sxt.service.impl.*.*(..))"
			id="pc1" />
		<!-- <aop:pointcut expression="execution(* com.bjsxt.service.impl.*.*(..))" 
			id="pc2"/> -->
		<aop:advisor advice-ref="txAdvise" pointcut-ref="pc1" />
		<!-- <aop:advisor advice-ref="txAdvise" pointcut-ref="pc2"/> -->
	</aop:config>

	<bean id="userService" class="com.sxt.service.impl.UserServiceImpl" autowire="byType">
		<!-- <property name="userMapper" ref="userMapper"></property> -->
	</bean>

</beans>
```

创建applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
		http://www.springframework.org/schema/tx/spring-tx.xsd
        ">
	<import resource="classpath:application-dao.xml"/>
	<import resource="classpath:application-service.xml"/>
	<import resource="classpath:application-action.xml"/>
</beans>
```

创建mybatis.cfg.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
	<!-- 加入日志 -->
	<settings>
		<setting name="logImpl" value="LOG4J"/>
	</settings>
	<!-- 插件 -->
	
	<!-- 别名 -->
	<!-- 加载mybait数据库操作的映射文件 -->
	<mappers>
		<mapper resource="com/sxt/mapping/UserMapper.xml" />
	</mappers>
</configuration>
```

测试类

```java
package com.hjh.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.hjh.controller.DeptController;

public class TsetMybatis {

	public static void main(String[] args) {
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		
		DeptController dc = (DeptController) applicationContext.getBean("deptController");
		
		//dc.add();
		dc.queryById();
	}
}
```



## spring和mybatis的集成【注解】

创建application-action.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

	<context:component-scan base-package="com.hjh.controller" />

</beans>
```

创建application-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">


	<!-- 声明数据源 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<!-- 注入相关属性 -->
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/vue2"></property>
		<property name="username" value="root"></property>
		<property name="password" value="hujiahao"></property>
	</bean>
	
	<!-- 声明sqlSessionFactory -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 注入数据源 -->
		<property name="dataSource" ref="dataSource"></property>
		<!-- 注入mybatis.xml -->
		<property name="configLocation" value="classpath:mybatis.cfg.xml"></property>
	</bean>
	
	<!-- 配置接口扫描 
			因为mapper没有实现类，所以必须依靠cglib在内存里面
			构造代理对象
	-->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!--  注入mapper接口所在包的位置 
				下面扫描的是某一个包，如果系统中的mapper文件不只一个包，如何办
		 -->
		<property name="basePackage" value="com.hjh.mapper"></property>
		
		<!-- 对于多个mapper扫描 -->
		<!-- 方式一 -->
		<!-- <property name="basePackage" value="con.hjh.mapper,con.hjh.mapper2"></property> -->
		
		<!-- 方式2 -->
		<!-- <property name="basePackage">
			<value>
				con.hjh.mapper
				con.hjh.mapper2
			</value>
		</property> -->
		
		<!-- 注入sqlSessionFactory 
			这里要求是sqlSessionFactoryBeanName也就是IOC容器里面的sqlSessionFactory对象
		-->
		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
	</bean>
	
</beans>
```

创建application-service.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<!-- 注入数据源 -->
		<property name="dataSource" ref="dataSource"></property>
	</bean>
	
	<!-- 扫描 -->
	<context:component-scan base-package="com.hjh.service.impl"></context:component-scan>	
</beans>
```

创建applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

	<import resource="classpath:application-dao.xml"/>
	<import resource="classpath:application-service.xml"/>
	<import resource="classpath:application-action.xml"/>
</beans>
```

测试类

```java
package com.hjh.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.hjh.controller.DeptController;

public class TsetMybatis {

	public static void main(String[] args) {
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		
		DeptController dc = (DeptController) applicationContext.getBean("deptController");
		
		//dc.add();
		dc.queryById();
	}
}
```



## spring和mybatis【去掉mybatis.cfg.xml】

修改application-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context.xsd
	    http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">


	<!-- 声明数据源 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<!-- 注入相关属性 -->
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/vue2"></property>
		<property name="username" value="root"></property>
		<property name="password" value="hujiahao"></property>
	</bean>
	
	<!-- 声明sqlSessionFactory -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 注入数据源 -->
		<property name="dataSource" ref="dataSource"></property>
		<!-- 注入mybatis.xml -->
		<!-- <property name="configLocation" value="classpath:mybatis.cfg.xml"></property> -->
		
		<property name="mapperLocations">
			<array>
				<value>classpath:com/hjh/mapping/*Mapper.xml</value>
			</array>
		</property>
		
		<property name="plugins">
			<array>
				<bean class="com.github.pagehelper.PageInterceptor">
				</bean>
			</array>
		</property>
	</bean>
	
	<!-- 配置接口扫描 
			因为mapper没有实现类，所以必须依靠cglib在内存里面
			构造代理对象
	-->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!--  注入mapper接口所在包的位置 
				下面扫描的是某一个包，如果系统中的mapper文件不只一个包，如何办
		 -->
		<property name="basePackage" value="com.hjh.mapper"></property>
		
		<!-- 对于多个mapper扫描 -->
		<!-- 方式一 -->
		<!-- <property name="basePackage" value="con.hjh.mapper,con.hjh.mapper2"></property> -->
		
		<!-- 方式2 -->
		<!-- <property name="basePackage">
			<value>
				con.hjh.mapper
				con.hjh.mapper2
			</value>
		</property> -->
		
		<!-- 注入sqlSessionFactory 
			这里要求是sqlSessionFactoryBeanName也就是IOC容器里面的sqlSessionFactory对象
		-->
		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
	</bean>
	
</beans>
```

