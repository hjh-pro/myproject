**springmvc**

[TOC]

# 一、springmvc概述

**一，概述**

Spring MVC是Spring提供的一个强大而灵活的web框架。借助于注解，Spring MVC提供了几乎是POJO的开发模式，使得控制器的开发和测试更加简单。这些控制器一般不直接处理请求，而是将其委托给Spring上下文中的其他bean，通过Spring的依赖注入功能，这些bean被注入到控制器中。 

Spring MVC主要由DispatcherServlet、处理器映射、处理器(控制器)、视图解析器、视图组成。他的两个核心是两个核心： 

处理器映射：选择使用哪个控制器来处理请求  

视图解析器：选择结果应该如何渲染

通过以上两点，Spring MVC保证了如何选择控制处理请求和如何选择视图展现输出之间的松耦合。

![image-20200309131334493](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200309131334493.png)

**二，springmvc的原理图**

![image-20200309131346130](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200309131346130.png)

1， 前端处理器

2， 控制器映射器

3， 控制器适配器

4，控制器

5， 视图处理器

------

**三，SpringMVC接口解释** 

（1）DispatcherServlet接口：  

Spring提供的前端控制器，所有的请求都有经过它来统一分发。在DispatcherServlet将请求分发给Spring Controller之前，需要借助于Spring提供的HandlerMapping定位到具体的Controller。  

（2）HandlerMapping接口：  

能够完成客户请求到Controller映射。  

（3）Controller接口：  

需要为并发用户处理上述请求，因此实现Controller接口时，必须保证线程安全并且可重用。  

Controller将处理用户请求，这和Struts Action扮演的角色是一致的。一旦Controller处理完用户请求，则返回ModelAndView对象给DispatcherServlet前端控制器，ModelAndView中包含了模型（Model）和视图（View）。  

从宏观角度考虑，DispatcherServlet是整个Web应用的控制器；从微观考虑，Controller是单个Http请求处理过程中的控制器，而ModelAndView是Http请求过程中返回的模型（Model）和视图（View）。  

（4）ViewResolver接口：  

Spring提供的视图解析器（ViewResolver）在Web应用中查找View对象，从而将相应结果渲染给客户。

**四，SpringMVC下载**

在spring包里面就有

前面spring里面的包

spring-web.xxxxx.jar

spring-webmvc.xxx.jar这两个核心包



# 二、springMVC的入门配置

## 1、创建项目

![image-20200309132017861](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200309132017861.png)

![image-20200309132047531](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200309132047531.png)

![image-20200309132100577](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200309132100577.png)

![image-20200309132132606](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200309132132606.png)

## 2、导入jar包

![image-20200309132246463](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200309132246463.png)

## 3、创建log4j.properties

```properties
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# MyBatis logging configuration...
log4j.logger.org.mybatis.example.BlogMapper=TRACE
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

## 4、创建Spring的核心配置文件springmvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 引入头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">


</beans>
```

## 5、配置前端控制器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>01SpringMVC_hello</display-name>
  <!-- 配置前端控制器 -->
  <servlet>
  	<servlet-name>springmvc</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<!-- 加载springmvc.xml -->
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:springmvc.xml</param-value>
  	</init-param>
  	<!-- 启动加载 -->
  	<load-on-startup>1</load-on-startup>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>springmvc</servlet-name>
  	<url-pattern>*.action</url-pattern>
  </servlet-mapping>
  
  
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
</web-app>
```

## 6、创建User

```java
package com.hjh.domain;

public class User {
	private Integer id;
	private String name;
	private String address;

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

	public String getAddress() {
		return address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", address=" + address + "]";
	}

}
```



## 7、配置User1Controller

```java
package com.hjh.controller;

import java.util.ArrayList;
import java.util.List;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import com.hjh.domain.User;

/**
 * 	第一种方式创建controller
 * 	@author hjh
 *
 */
public class User1Controller implements Controller {

	@Override
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
		
		List<User> users = new ArrayList<User>();
		
		for (int i = 0; i < 21; i++) {
			users.add(new User(i,"hhh"+i,"江苏南京"+i));
		}
		
		// 封装页面地址和要跳转到页面的数据
		ModelAndView modelAndView = new ModelAndView();
		// 封装数据
		modelAndView.addObject("users", users);
		//以前的方式
//		request.setAttribute("users", users);
//		request.getRequestDispatcher("WEB-INF/view/list.jsp")
//			.forward(request, response);
		
		// 封装跳转的页面
		modelAndView.setViewName("WEB-INF/view/list.jsp"); // 默认是请求转发
		// 如果非要重定向的话如下
		//modelAndView.setViewName("redirect:WEB-INF/view/list.jsp"); // response.sendRedirect(arg0);
		
		return modelAndView;
	}

}

```



## 8、配置控制器

```xml
<!-- 配置控制器 -->
	<bean id="user1Controller" name="/user1Controller" class="com.hjh.controller.User1Controller"></bean>
```



## 9、配置控制器映射器

```xml
<!-- 配置控制器映射器--开始 -->
	<!-- 有两个处理器映射器
		1:org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping把bean的名字做为url进行查找，也就是handler必须配置name属性
		2:org.springframework.web.servlet.handler.SimpleUrlHandlerMapping  里面可以配置属性【后面讲】
	 -->
	<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
	<!-- 配置控制器映射器--结束 -->
```



## 10、配置控制器适配器

```xml
<!-- 配置控制器适配器 开始 -->
	<!-- 有两个处理器适配器
		1:org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter写的controller要实现Controller的接口
		2:org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter 写controler要实现HttpRequestHandler接口【后面说】
	 -->
	<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean> 
	<!-- 配置控制器适配器 结束 -->
```



## 11、配置视图解析器

```xml
<!-- 配置视图解析器 -->
	<!-- 
		InternalResourceViewResolver : 支持JSTL
	 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"></bean>
```

## 12、发布测试

![image-20200309175236019](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200309175236019.png)

总结：

```
springmvc的执行过程分析:

1、页面通过url访问：
	|-此过程会进行到web.xml中
	http://localhost:8080/hjh/user1Controller.action
	如果后缀不是.action，则根本不会进入前端控制器
	
2、进入到前端控制器中
	|-会进入springmvc.xml文件中
	springmvc.xml中我们配置了控制器映射器，里面对应了controller
	中name属性的路径，如果页面访问的路径与里面可以匹配，则再调用
	我们配置的控制器适配器，去执行我们对应的Controller
	
3、进入到Controller中
	|-对数据进行封装
	|-设置响应的路径
	|-返回ModelAndView对象
	
4、进入视图解析器
	|-解析数据
	|-生成浏览器可以解析的代码(html,js)
```



# 三、springmvc.xml详解

## 3.1、非注解映射器

创建User2Controller

```java
package com.hjh.controller;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.HttpRequestHandler;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import com.hjh.domain.User;

/**
 * 第2种方式创建controller
 * 
 * @author hjh
 *
 */
public class User2Controller implements HttpRequestHandler {

	@Override
	public void handleRequest(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		List<User> users = new ArrayList<User>();

		for (int i = 0; i < 21; i++) {
			users.add(new User(i, "hhh" + i, "江苏南京" + i));
		}

		request.setAttribute("users", users);
		request.getRequestDispatcher("WEB-INF/view/list.jsp").forward(request, response);
	}

}
```

修改springmvc.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 引入头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

	<!-- 配置控制器 -->
	<bean id="user1Controller" name="/user1Controller.action" class="com.hjh.controller.User1Controller"></bean>
	<bean id="user2Controller" name="/user4Controller.action" class="com.hjh.controller.User2Controller"></bean>

	<!-- 配置控制器映射器  开始 -->
	<!-- 有两个处理器映射器
		1:org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping把bean的名字做为url进行查找，也就是handler必须配置name属性
		2:org.springframework.web.servlet.handler.SimpleUrlHandlerMapping  里面可以配置属性【后面讲】
	 -->
	<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
	<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
		<property name="mappings">
			<props>
				<!-- 如果key值一样,value也一样，下面的覆盖上面的 -->
				<prop key="/user2Controller.action">user1Controller</prop>
				<prop key="/user3Controller.action">user1Controller</prop>
				<prop key="/user4Controller.action">user2Controller</prop>
			</props>
		</property>
	</bean>
	
	<!-- 配置控制器映射器 结束 -->	
	
	<!-- 配置控制器适配器 开始 -->
	<!-- 有两个处理器适配器
		1:org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter写的controller要实现Controller的接口
		2:org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter 写controler要实现HttpRequestHandler接口【后面说】
	 -->
	<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean> 
	<bean class="org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter"></bean> 
	<!-- 配置控制器适配器 结束 -->

	<!-- 配置视图解析器 -->
	<!-- 
		InternalResourceViewResolver : 支持JSTL
	 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"></bean>
	
</beans>

```

总结：

```
两种映射器可以共存，如果通过name找不到，就会去第二种映射器里面寻找

两种适配器也可以共存，如果是实现Controller接口，就会调用第一种适配器，
如果实现的是HttpRequestHandler，则会调用第二种适配器。
```

## 3.2注解映射器和适配器

创建UserController

```java
package com.hjh.controller;

import java.util.ArrayList;
import java.util.List;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.servlet.ModelAndView;

import com.hjh.domain.User;

/**
 * 	注解配置controller
 * 
 * @author hjh
 *
 */
@Controller
@RequestMapping("user")
public class UserController {

	@RequestMapping(value = {"user1Controller","user2Controller","user3Controller"},method=RequestMethod.GET)
	public ModelAndView query() {
		List<User> users = new ArrayList<User>();
		for (int i = 0; i < 21; i++) {
			users.add(new User(i, "hhh" + i, "江苏南京" + i));
		}
		// 封装页面地址和要跳转到页面的数据
		ModelAndView modelAndView = new ModelAndView();
		// 封装数据
		modelAndView.addObject("users", users);

		// 封装跳转的页面
		modelAndView.setViewName("/WEB-INF/view/list.jsp");

		return modelAndView;
	}
}

```

修改springmvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 引入头文件 -->
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

	<!-- 开启扫描 -->
	<context:component-scan base-package="com.hjh.controller" />

	<!-- 配置控制器映射器  开始 -->
	<!-- <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"></bean> -->
	<!-- 配置控制器映射器 结束 -->	
	
	<!-- 配置控制器适配器 开始 -->
	<!-- <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"></bean> -->
	<!-- 配置控制器适配器 结束 -->
	
	<!-- 增强映射器和适配器的配置
			|- 可以进行数据转化，如把对象转换成json字符串等等
	 -->
	<!-- <mvc:annotation-driven /> -->

	<!-- 配置视图解析器 -->
	<!-- 
		InternalResourceViewResolver : 支持JSTL
	 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"></bean>

</beans>
```

总结：

```
注解config的总结

1、不需要在springmvc.xml中配置url映射的Controller
	|-开启注解扫描

2、controller类添加@Controller注解
3、方法上添加@RequestMapping
	|- 表示请求的url
	|- 参数有value 和 method
		|- value返回的是string[],可以配置多个请求的url
		|- method表示页面访问时的请求方式，通过网页的地址栏访问都是Get请求

4、类上添加@RequestMapping
	|- 管理我们此类里面的所有方法(类是文件夹，某个节点)
	|- 访问url时前必须加上类里面的value值
	|- 注意：类上有次注解，设置的响应如果是"WEB-INF/view/list.jsp"，必须写绝对路径"/WEB-INF/view/list.jsp"
	
5、springmvc里面的控制器映射器和控制器适配器可以不用写，添加一个增强映射器和适配器的配置
	|- <mvc:annotation-driven />
		|- 可以进行数据转化，如把对象转换成json字符串等等
	|- <mvc:annotation-driven />：不添加也能运行，但是有对象转化成json的时候就会出问题
```

## 3.3DispatcherServlet.properties的说明

**当springmvc.xml里面的映射器、适配器和视图解析器都不配置，依然可以运行，是spring-webmvc的jar包中有默认的配置文件**

![image-20200309191245057](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200309191245057.png)

![image-20200309191337787](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200309191337787.png)

# 四、参数绑定

## 创建index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h2>用户登陆</h2>
	<form action="user/user01.action" method="post">
		用户名:<input type="text" name="name"  /><br>
		密码:<input type="text" name="password"  /><br>
		<input type="submit" value="提交" />
	</form>
	<hr>
	<h2>用户登陆+数组</h2>
	<form action="user/user02.action" method="post">
		爱好:<input type="checkbox" name="hobby" value="LOL"  />LOL
		<input type="checkbox" name="hobby" value="QQFC"  />QQFC
		<input type="checkbox" name="hobby" value="XYX"  />XYX
		<input type="checkbox" name="hobby" value="WZRY"  />WZRY
		<input type="submit" value="提交" />
	</form>
	<hr>
	<h2>用户登陆+对象</h2>
	<form action="user/user03.action" method="post">
		用户ID:<input type="text" name="id"  /><br>
		用户姓名:<input type="text" name="name"  /><br>
		地址:<input type="text" name="address"  /><br>
		<input type="submit" value="提交" />
	</form>
	<hr>
	<h2>用户登陆+对象+时间</h2>
	<form action="user/user03.action" method="post">
		用户ID:<input type="text" name="id"  /><br>
		用户姓名:<input type="text" name="name"  /><br>
		地址:<input type="text" name="address"  /><br>
		时间:<input type="text" name="date"  /><br>
		<input type="submit" value="提交" />
	</form>
</body>
</html>
```

## 创建User

```java
package com.hjh.domain;


import java.util.Date;

import org.springframework.format.annotation.DateTimeFormat;

public class User {
	private Integer id;
	private String name;
	private String address;
	@DateTimeFormat(pattern="yyyy-MM-dd")
	private Date date;

	public User() {
	}

	public User(Integer id, String name, String address, Date date) {
		super();
		this.id = id;
		this.name = name;
		this.address = address;
		this.date = date;
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

	public String getAddress() {
		return address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	public Date getDate() {
		return date;
	}

	public void setDate(Date date) {
		this.date = date;
	}

	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", address=" + address + ", date=" + date + "]";
	}

}

```

## 创建Controller

```java
package com.hjh.controller;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.servlet.ModelAndView;

import com.hjh.domain.User;

/**
 * 	注解配置controller
 * 
 * @author hjh
 *
 */
@Controller
@RequestMapping("user")
public class UserController {

	/**
	 * 传递String参数
	 * @param name
	 * @param password
	 * @return
	 */
	@RequestMapping("/user01")
	public ModelAndView user01(String name,String password) {
		System.out.println(name+","+password);
		// 封装页面地址和要跳转到页面的数据
		ModelAndView modelAndView = new ModelAndView();
		// 封装跳转的页面
		modelAndView.setViewName("/WEB-INF/view/success.jsp");
		return modelAndView;
	}
	
	/**
	 * 传递String[]数组参数
	 * @param name
	 * @param password
	 * @return
	 */
	@RequestMapping("/user02")
	public ModelAndView user02(String[] hobby) {
		System.out.println(Arrays.toString(hobby));
		// 封装页面地址和要跳转到页面的数据
		ModelAndView modelAndView = new ModelAndView();
		// 封装跳转的页面
		modelAndView.setViewName("/WEB-INF/view/success.jsp");
		return modelAndView;
	}
	
	/**
	 * 传递对象参数
	 * @param name
	 * @param password
	 * @return
	 */
	@RequestMapping("/user03")
	public ModelAndView user03(User user) {
		System.out.println(user);
		// 封装页面地址和要跳转到页面的数据
		ModelAndView modelAndView = new ModelAndView();
		// 封装跳转的页面
		modelAndView.setViewName("/WEB-INF/view/success.jsp");
		return modelAndView;
	}
	
	/**
	 * 传递对象+时间参数
	 * @param name
	 * @param password
	 * @return
	 */
	@RequestMapping(value="user04",method=RequestMethod.POST)
	public ModelAndView user04(User user) {
		System.out.println(user);
		// 封装页面地址和要跳转到页面的数据
		ModelAndView modelAndView = new ModelAndView();
		// 封装跳转的页面
		modelAndView.setViewName("/WEB-INF/view/success.jsp");
		return modelAndView;
	}
}

```

## 创建success.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	success
</body>
</html>
```

\*注意:springmvc.xml中开启数据转换的配置**<mvc:annotation-driven />**

## 其他WEB类型参数的绑定

### 第一钟方法【使用形式参数绑定】

```java
/**
	 * 	WEB参数的绑定
	 * 	注意： servletContext不能绑定参数，只能从request、session中取
	 * @return
	 */
	@RequestMapping(value="queryWeb")
	public String queryWeb(HttpServletRequest request,HttpServletResponse response,HttpSession session) {
		System.out.println(request);
		System.out.println(response);
		System.out.println(session);
		// 从request获取session
		HttpSession session2 = request.getSession();
		System.out.println(session2);
		
		// 从request中取出servletContext
		ServletContext servletContext = request.getServletContext();
		System.out.println(servletContext);
		
		// 从session中取出servletContext
		ServletContext servletContext2 = session.getServletContext();
		System.out.println(servletContext2);
		return "/WEB-INF/view/list.jsp";
	}
```



### 第二钟方法【注入的方式绑定】

```java
@Autowired
	private HttpServletRequest request;
	
	@Autowired
	private HttpServletResponse response;
	
	@Autowired
	private HttpSession session;
	
	/**
	 * 	WEB参数的绑定
	 * 	注意： session两次打印的不一样，是springmvc对session进行了封装，其实是同一个
	 *  	对象的hashCode的值可能不一样，但是里面的内容是一样的
	 * @return
	 */
	@RequestMapping(value="queryWeb2")
	public String queryWeb2() {
		System.out.println(request.hashCode());
		System.out.println(response.hashCode());
		System.out.println(session.hashCode());
		session.setAttribute("user", "admin");
		// 从request获取session
		HttpSession session2 = request.getSession();
		System.out.println(session2.hashCode());
		System.out.println(session2.getAttribute("user"));
		
		// 从request中取出servletContext
		ServletContext servletContext = request.getServletContext();
		System.out.println(servletContext.hashCode());
		
		// 从session中取出servletContext
		ServletContext servletContext2 = session.getServletContext();
		System.out.println(servletContext2.hashCode());
		StringUtils.getWeb();  // 同理虽然hashCode值不一样但是本质是同一个，只是springmvc对其进行了封装
		return "/WEB-INF/view/list.jsp";
	}
```



### 第三钟方法【使用解耦的方式绑定】

```java
package com.hjh.utils;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

public class StringUtils {
	
	/**
	 * 获取WEB参数
	 * 		|- 每次请求都会启动一个线程
	 * 		|- RequestContextHolder会存放当前线程的web参数
	 */
	public static void getWeb() {
		System.out.println("==============================");
		//RequestAttributes requestAttributes = RequestContextHolder.getRequestAttributes();
		//System.out.println(requestAttributes.getClass().getSimpleName());  //ServletRequestAttributes
		
		
		ServletRequestAttributes requestAttributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
		HttpServletRequest request = requestAttributes.getRequest();
		HttpServletResponse response = requestAttributes.getResponse();
		HttpSession session = request.getSession();
		System.out.println(request.hashCode());
		System.out.println(response.hashCode());
		System.out.println(session.hashCode());
	}
}
```

# 五、乱码问题

## 1、请求乱码

方式一：

String name =request.getParameter("name");

配置  request.setCharacterEncoding("UTF-8");

方式2：

如果还有乱码可以配置tomcat

**在port=8080里面添加URLEncoding=“UTF-8”**

方式3：

如果还不可以，

通过下面方式取

String name2=new String(name.getBytes("iso-8859-1"),"utf-8);

## 2、响应乱码

response.setCharacterEncoding("utf-8");

PrintWriter out = response.getWriter();

out.write("你好 世界");

```
此处response.setCharacterEncoding("utf-8");解决的是写入你好 世界这个字符串的乱码，以UTF-8写入。

```

response.setContentType("text/html;charset=utf-8");

```
强制浏览器使用这种编码去解析
```

## 3、过滤器

**spring里面的解决方法**

web.xml中配置

```xml
<!-- 配置过滤器 -->
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>
	
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<!-- 所有的请求都经过过滤器 ： servlet里面推荐使用的-->
		<!-- <url-pattern>/*</url-pattern> -->
		
		<!-- springmvc里面推荐使用 ：过滤进入springmvc这个servlet的才会过滤 -->
		<servlet-name>springmvc</servlet-name>
	</filter-mapping>
```



## 3、数据库乱码

第一种情况：

创建数据库的时候选择utf-8

第二种在配置文件里面添加下面的语句

```
添加
useUnicode=true&characterEncoding=UTF8
```

第三种修改mysql的默认编码

修改m.ini的文件

​	将里面的改成

​		|- default-character-set=utf8



# 六、转发和重定向

## servlet里面的

转发

一次请求一次响应，只能在服务器内部转发，页面发生变化，之后客户端不知道

```java
request.getRequestDispatcher("WEB-INF/view/list.jsp")
			.forward(request, response);
```

重定向

两次请求两次响应，可以重定向到其他服务器

页面的url会发生变化

```java
response.sendRedirect(”WEB-INF/view/list.jsp“);
```



## springmvc里面的

第一种方式请求转发

```java
@RequestMapping("/user01")
	public ModelAndView user01(String name,String password) {
		System.out.println(name+","+password);
		// 封装页面地址和要跳转到页面的数据
		ModelAndView modelAndView = new ModelAndView();
		// 封装跳转的页面
		modelAndView.setViewName("/WEB-INF/view/success.jsp");
		return modelAndView;
	}
```

第2种方式请求转发

```java
@RequestMapping(value="query")
public String query() {
	return "/WEB-INF/view/list.jsp";
}
```

第3种方式重定向

```java
/**
	 * 重定向
	 */
	@RequestMapping("sendRedirect")
	public String sendRedirect() {
		return "redirect:/index.jsp";  // /代表WebContent目录
	}
```

# 七、视图解析器

## 配置前后缀

```xml
<!-- 配置视图解析器 -->
	<!-- 
		InternalResourceViewResolver : 支持JSTL
	 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<!-- 添加前缀 -->
		<property name="prefix" value="/WEB-INF/view/"></property>
		<!-- 添加后缀 -->
		<property name="suffix" value=".jsp"></property>
	</bean>
```

控制器

```java
/**
	 * 视图解析器配置前后缀
	 */
	@RequestMapping("res")
	public String res() {
		return "success";  
	}
```

# 八、SSM的集成

8.1创建项目

​			导包

8.2创建配置文件

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
</configuration>
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
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/vue"></property>
		<property name="username" value="root"></property>
		<property name="password" value="hujiahao"></property>
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
		<property name="plugins">
			<array>
				<bean class="com.github.pagehelper.PageHelper"></bean>
			</array>
		</property>
	</bean>
	
	<!-- Mapper接口所在包名，Spring会自动查找之中的类 根据Mapper接口  -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- 提供Mapper接口所在以包 如果有多个包，可以使用,去隔开-->
		<property name="basePackage" value="com.hjh.mapper"></property>
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
</beans>
```

到此spring和mybatis集成完毕

## 集成springMVC

创建springmvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

	<!-- 开启扫描 -->
	<context:component-scan base-package="com.hjh.controller" />

	<!-- 配置控制器映射器  开始 -->
	<!-- <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"></bean> -->
	<!-- 配置控制器映射器 结束 -->	
	
	<!-- 配置控制器适配器 开始 -->
	<!-- <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"></bean> -->
	<!-- 配置控制器适配器 结束 -->
	
	<!-- 配置视图解析器 -->
	<!-- 
		InternalResourceViewResolver : 支持JSTL
	 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<!-- 添加前缀 -->
		<property name="prefix" value="/WEB-INF/view/"></property>
		<!-- 添加后缀 -->
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<!-- 增强映射器和适配器的配置
			|- 可以进行数据转化，如把对象转换成json字符串等等
	 -->
	<mvc:annotation-driven />
	
</beans>
```

配置web.xml的前端控制器

```xml
<!-- 配置前端控制器 -->
	<servlet>
		<servlet-name>springmvc</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:springmvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>springmvc</servlet-name>
		<url-pattern>*.action</url-pattern>
	</servlet-mapping>
```

配置web.xml的过滤器

```xml
<!-- 配置过滤器 -->
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>

	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<!-- 所有的请求都经过过滤器 ： servlet里面推荐使用的 -->
		<!-- <url-pattern>/*</url-pattern> -->

		<!-- springmvc里面推荐使用 ：过滤进入springmvc这个servlet的才会过滤 -->
		<servlet-name>springmvc</servlet-name>
	</filter-mapping>
```

如果xml文件有报错，解决方法

```
首先可以尝试通过禁用xml的命名空间引用的验证来解决，具体设置是在：Window → Preferences → Validation → XML 将设置里的对勾取消掉即可
```

spring和mybatis集成通过控制台加载applicationContext.xml文件，解析里面的内容。

但现在是web程序，不需要通过控制台程序。

第一种解决方法：(在springmvc.xml中添加)

```
<!-- 第一种加载applicationContext.xml的方法，不推荐 -->
<import resource="classpath:applicationContext.xml"/>
```

第二种解决方法：

配置spring监听器

```xml
<!-- 加载Spring容器配置,配置监听器 -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<!-- Spring容器加载所有的配置文件的路径，启动的时候加载applicationContext.xml -->
	<context-param>
	 	<param-name>contextConfigLocation</param-name>
		<param-value>classpath*:applicationContext.xml</param-value>
	</context-param>
```

web.xml的配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	id="WebApp_ID" version="3.1">

	<display-name>06SSM集成</display-name>
	<!-- 配置过滤器 -->
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>

	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<!-- 所有的请求都经过过滤器 ： servlet里面推荐使用的 -->
		<!-- <url-pattern>/*</url-pattern> -->

		<!-- springmvc里面推荐使用 ：过滤进入springmvc这个servlet的才会过滤 -->
		<servlet-name>springmvc</servlet-name>
	</filter-mapping>
	
	<!-- 加载Spring容器配置,配置监听器 -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<!-- Spring容器加载所有的配置文件的路径，启动的时候加载applicationContext.xml -->
	<context-param>
	 	<param-name>contextConfigLocation</param-name>
		<param-value>classpath*:applicationContext.xml</param-value>
	</context-param>

	<!-- 配置前端控制器 -->
	<servlet>
		<servlet-name>springmvc</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:springmvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>springmvc</servlet-name>
		<url-pattern>*.action</url-pattern>
	</servlet-mapping>

	<!-- 欢迎页面 -->
	<welcome-file-list>
		<welcome-file>index.jsp</welcome-file>
	</welcome-file-list>
</web-app>
```

到此集成完毕

```
classpath和classpath*区别： 

classpath：只会到你的class路径中查找找文件。

classpath*：不仅包含class路径，还包括jar文件中（class路径）进行查找。

注意： 用classpath*:需要遍历所有的classpath，所以加载速度是很慢的；因此，在规划的时候，应该尽可能规划好资源文件所在的路径，尽量避免使用classpath*。

classpath*的使用：

当项目中有多个classpath路径，并同时加载多个classpath路径下（此种情况多数不会遇到）的文件，*就发挥了作用，如果不加*，则表示仅仅加载第一个classpath路径。
```



## 完成用户的分页查询

创建User

```java
package com.hjh.domain;

import java.util.Date;

public class User {
    private Integer id;

    private String name;

    private String address;

    private Date birthday;

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
        this.name = name == null ? null : name.trim();
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address == null ? null : address.trim();
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }
}
```

创建UserMapper

```java
package com.hjh.mapper;

import java.util.List;

import com.hjh.domain.User;

public interface UserMapper {
    int deleteByPrimaryKey(Integer id);

    int insert(User record);

    int insertSelective(User record);

    User selectByPrimaryKey(Integer id);

    int updateByPrimaryKeySelective(User record);

    int updateByPrimaryKey(User record);
    /**
     * 进行模糊查询
     * @param user
     * @return
     */
    List<User> queryAllUsers(User user);
}
```

修改UserMapper.xml

```xml
<select id="queryAllUsers" resultMap="BaseResultMap" parameterType="com.hjh.domain.User" >
    select 
    <include refid="Base_Column_List" />
    from user
    <where>
    	<if test="id != null">
    		and id = #{id}
    	</if>
    	<if test="name != null and name !='' ">
    		and name like "%"#{name}"%"
    	</if>
    	<if test="address != null and address !='' ">
    		and address like "%"#{address}"%"
    	</if>
    </where>
  </select>
```

创建PageBean

```java
package com.hjh.utils;

/**
 * 分页实体
 * @author hjh
 *
 */
public class PageBean {
	private Integer currentPage;
	private Integer pageSize;
	private Integer totalPage;
	private Integer totalCount;

	public Integer getCurrentPage() {
		return currentPage;
	}

	public void setCurrentPage(Integer currentPage) {
		this.currentPage = currentPage;
	}

	public Integer getPageSize() {
		return pageSize;
	}

	public void setPageSize(Integer pageSize) {
		this.pageSize = pageSize;
	}

	public Integer getTotalPage() {
		return totalPage;
	}

	public void setTotalPage(Integer totalPage) {
		this.totalPage = totalPage;
	}

	public Integer getTotalCount() {
		return totalCount;
	}

	public void setTotalCount(Integer totalCount) {
		this.totalCount = totalCount;
		this.totalPage = (int) Math.ceil((this.totalCount*1.0/this.pageSize));
	}

}
```

创建UserVo

```java
package com.hjh.vo;

import com.hjh.domain.User;

public class UserVo extends User {

}

```

创建UserService

```java
package com.hjh.service;

import java.util.List;

import com.hjh.domain.User;
import com.hjh.utils.PageBean;
import com.hjh.vo.UserVo;

public interface UserService {
	public List<User> queryUserForPage(PageBean pageBean,UserVo userVo);
}

```

创建UserController

```java
package com.hjh.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.hjh.domain.User;
import com.hjh.service.UserService;
import com.hjh.utils.PageBean;
import com.hjh.vo.UserVo;

@Controller
@RequestMapping("user")
public class UserController {
	
	@Autowired
	private UserService userService;
	
	/**
	 * UserVo : 表示view object 和页面中的对应
	 * domain : 和数据库对应的
	 * 需要从页面上获取当前第几页
	 * @param userVo
	 * @return
	 */
	@RequestMapping("queryAllUsers")
	public String queryAllUsers(UserVo userVo,Model model) {
		PageBean pageBean = new PageBean();
		// 判断currPage是否为空
		if(null==userVo.getCurrPage()) {
			pageBean.setCurrentPage(1);
		}else {
			pageBean.setCurrentPage(userVo.getCurrPage());
		}
		pageBean.setPageSize(5);
		List<User> users = userService.queryUserForPage(pageBean , userVo);
		model.addAttribute("users", users);
		return "list";
	}

}
```

创建list.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
	String path = request.getContextPath();
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	
	<h2 align="center">用户列表</h2>
	<hr>
	<table align="center" width="60%" border="1" cellpadding="5">
		<tr>
			<th>编号</th>
			<th>姓名</th>
			<th>地址</th>
			<th>生日</th>
		</tr>
		<c:forEach var="x" items="${users}">
			<tr align="center">
				<td>${x.id }</td>
				<td>${x.name }</td>
				<td>${x.address }</td>
				<td>${x.birthday }</td>
			</tr>
		</c:forEach>
	</table>
</body>
</html>
```

# 九、springmvc返回字符串到页面

```java
package com.hjh.controller;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import com.hjh.domain.User;
import com.hjh.mapper.UserMapper;
import com.hjh.service.UserService;
import com.hjh.utils.PageBean;
import com.hjh.vo.UserVo;

@Controller
@RequestMapping("user")
public class UserController {
	
	private static ArrayList<String> users=new ArrayList<String>();
	
	static {
		users.add("admin");
		users.add("zs");
		users.add("ls");
		users.add("ww");
	}
	
	/**
	 *	验证用户名是否存在
	 * @param userVo
	 * @return
	 */
	@RequestMapping("validateUserName")
	@ResponseBody  // 返回字符串到页面
	public String queryAllUsers(String username) {
		boolean flag = false;
		for (String string : users) {
			if(string.equals(username)) {
				flag = !flag;
				break;
			}
		}
		return String.valueOf(flag);
	}
	
	/**
	 * 	会出现乱码，返回的是string，而且return里面有中文，解决方法：produces="text/html;charset=utf8"
	 * @param username
	 * @return
	 */
	@RequestMapping(value="validateUserName2",produces="text/html;charset=utf8")
	@ResponseBody  // 返回字符串到页面
	public String queryAllUsers2(String username) {
		boolean flag = false;
		for (String string : users) {
			if(string.equals(username)) {
				flag = !flag;
				break;
			}
		}
		return flag==true?"存在":"不存在";
	}
	
	/**
	 * 	springmvc提供的返回对象的方法，需要导入jackson的三个包
	 * @param username
	 * @return
	 */
	@RequestMapping(value="validateUserName3")
	@ResponseBody  // 返回字符串到页面
	public User queryAllUsers3(String username) {
		User user = new User(1, "hjh", "江苏南京", new Date());
		return user;
	}

}
```

index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!-- <h1>
		<a href="user/queryAllUsers.action">查询用户</a>
	</h1> -->
	
	<h2>用户登陆</h2>
	<input type="text" name="username" id="username" />
	<label id="msg"></label>
	<!-- <label>用户名已存在</label> -->
	
	<script type="text/javascript" src="res/js/jquery-3.1.1.min.js"></script>
	<script type="text/javascript">
		$(function() {
			
			$("#username").blur(function(){
				var username = $(this).val();
				var url = "user/validateUserName.action";
				$.get(url,{username:username},function(res){
					//console.log(res=="true");
					if(res=="true"){
						$("#msg").html('<font color=red>用户名已存在</fotn>');
					}else{
						$("#msg").html('<font color=blue>用户名不存在</fotn>');
					}
				})
			})
			
		})
	</script>
</body>
</html>
```

# 十、文件上传

```java
package com.hjh.controller;

import java.io.File;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.multipart.MultipartFile;

import com.hjh.utils.RandomUtils;


@Controller
@RequestMapping("upload")
public class UserController {

	/**
	 * 	上传一个文件不改名字
	 */
	@RequestMapping("upload01")
	public String upload01(MultipartFile mf,HttpServletRequest request) {
		
		// 1、得到文件的相关信息
		String type = mf.getContentType(); // 得到文件的类型
		//mf.getInputStream(); // 得到文件的流
		String name = mf.getName(); // 得到input里面对应name的值
		String fileName = mf.getOriginalFilename(); // 得到文件的名称
		long size = mf.getSize(); // 文件大小
		System.out.println(type); // image/png
		System.out.println(name); // mf
		System.out.println(fileName); // delete.PNG
		System.out.println(size); // 1019
		
		// 2、得到上传的路径
		String realPath = request.getServletContext().getRealPath("/upload/images");
		System.out.println(realPath);
		
		// 3、组装文件对象
		File file = new File(realPath, fileName);
		
		// 4、把文件流写到file
		try {
			mf.transferTo(file);
		} catch (Exception e) {
			e.printStackTrace();
		}
		
		return "success";
	}
	
	/**
	 * 	上传多个文件不改名字
	 */
	@RequestMapping("upload02")
	public String upload02(MultipartFile[] mf,HttpServletRequest request) {
		//System.out.println(mf[2].getSize() == 0);
		if (null!=mf && mf.length>0) {
			for (int i = 0; i < mf.length; i++) {
				// 1、得到文件的相关信息
				String type = mf[i].getContentType(); // 得到文件的类型
				//mf.getInputStream(); // 得到文件的流
				String name = mf[i].getName(); // 得到input里面对应name的值
				String fileName = mf[i].getOriginalFilename(); // 得到文件的名称
				long size = mf[i].getSize(); // 文件大小
				System.out.println(type); // image/png
				System.out.println(name); // mf
				System.out.println(fileName); // delete.PNG
				System.out.println(size); // 1019
				
				if (size == 0) {
					continue;
				}
				
				// 2、得到上传的路径
				String realPath = request.getServletContext().getRealPath("/upload/images");
				System.out.println(realPath);
				
				// 3、组装文件对象
				File file = new File(realPath, fileName);
				
				// 4、把文件流写到file
				try {
					mf[i].transferTo(file);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		}
		
		return "success";
	}
	
	/**
	 * 	上传一个文件改名字
	 */
	@RequestMapping("upload03")
	public String upload03(MultipartFile mf,HttpServletRequest request) {
		
		// 1、得到文件的相关信息
		String fileName = mf.getOriginalFilename(); // 得到文件的名称
		
		// 2、得到上传的路径
		String realPath = request.getServletContext().getRealPath("/upload/images");
		
		// 得到新名字
		String newName = RandomUtils.createRandomStrUseTime(fileName);
		
		// 3、组装文件对象
		File file = new File(realPath, newName);
		
		// 4、把文件流写到file
		try {
			mf.transferTo(file);
		} catch (Exception e) {
			e.printStackTrace();
		}
		
		return "success";
	}
	
	/**
	 * 	上传一个文件改名字+分文件夹管理
	 */
	@RequestMapping("upload04")
	public String upload04(MultipartFile mf,HttpServletRequest request) {
		
		// 1、得到文件的相关信息
		String fileName = mf.getOriginalFilename(); // 得到文件的名称
		
		// 2、得到上传的路径
		String realPath = request.getServletContext().getRealPath("/upload/images");
		
		// 得到新名字
		String newName = RandomUtils.createRandomStrUseTime(fileName);
		
		// 得到新文件夹的名字
		String generateDirDateName = RandomUtils.generateDirDateName(); // 根据当前日期获取文件夹名称
		File dirFile = new File(realPath,generateDirDateName);
		
		// 判断文件夹是否存在
		if(!dirFile.exists()) {
			dirFile.mkdirs();
		}
		
		// 3、组装文件对象
		File file = new File(dirFile, newName);
		
		// 4、把文件流写到file
		try {
			mf.transferTo(file);
		} catch (Exception e) {
			e.printStackTrace();
		}
		
		return "success";
	}
}
```

index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>一个文件上传【不改名】</h1>
	<form action="upload/upload01.action" method="post" enctype="multipart/form-data">
		选择文件：<input type="file" name="mf" /><br>
		<input type="submit" value="提交" />
	</form>
	<hr>
	<h1>多文件上传【不改名】</h1>
	<form action="upload/upload02.action" method="post" enctype="multipart/form-data">
		选择文件：<input type="file" name="mf" /><br>
		选择文件：<input type="file" name="mf" /><br>
		选择文件：<input type="file" name="mf" /><br>
		<input type="submit" value="提交" />
	</form>
	<hr>
	<h1>一个文件上传【改名】</h1>
	<form action="upload/upload03.action" method="post" enctype="multipart/form-data">
		选择文件：<input type="file" name="mf" /><br>
		<input type="submit" value="提交" />
	</form>
	<hr>
	<h1>一个文件上传【改名】 + 分文件夹管理</h1>
	<form action="upload/upload04.action" method="post" enctype="multipart/form-data">
		选择文件：<input type="file" name="mf" /><br>
		<input type="submit" value="提交" />
	</form>
</body>
</html>
```

上传文件+保存到数据库

```java
package com.hjh.controller;

import java.io.File;
import java.util.Date;

import javax.servlet.http.HttpServletRequest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.multipart.MultipartFile;

import com.hjh.service.AttachmentService;
import com.hjh.utils.RandomUtils;
import com.hjh.vo.AttachmentVo;


@Controller
@RequestMapping("upload")
public class AttachmentController {
	
	@Autowired
	private AttachmentService  attachmentService; 
	
	/**
	 * 	上传一个文件改名字+分文件夹管理 + 保存到数据库
	 */
	@RequestMapping("upload01")
	public String upload04(MultipartFile mf,HttpServletRequest request) {
		long size = mf.getSize();
		String contentType = mf.getContentType();
		
		// 1、得到文件的相关信息
		String fileName = mf.getOriginalFilename(); // 得到文件的名称
		
		// 2、得到上传的路径
		String realPath = request.getServletContext().getRealPath("/upload/images");
		
		// 得到新名字
		String newName = RandomUtils.createRandomStrUseTime(fileName);
		
		// 得到新文件夹的名字
		String generateDirDateName = RandomUtils.generateDirDateName(); // 根据当前日期获取文件夹名称
		File dirFile = new File(realPath,generateDirDateName);
		
		// 判断文件夹是否存在
		if(!dirFile.exists()) {
			dirFile.mkdirs();
		}
		
		// 3、组装文件对象
		File file = new File(dirFile, newName);
		
		// 4、把文件流写到file
		try {
			mf.transferTo(file);
		} catch (Exception e) {
			e.printStackTrace();
		}
		
		
		// 保存到数据库
		AttachmentVo attachmentVo = new AttachmentVo();
		attachmentVo.setContentType(contentType);
		attachmentVo.setSize(Double.valueOf(size));
		attachmentVo.setOldName(fileName);
		attachmentVo.setUploadTime(new Date());
		attachmentVo.setPath("upload/images/" + generateDirDateName + "/" + newName);
		attachmentService.addData(attachmentVo);
		
		return "success";
	}
}

```

index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>一个文件上传【改名】+保存到数据库</h1>
	<form action="upload/upload01.action" method="post" enctype="multipart/form-data">
		选择文件：<input type="file" name="mf" /><br>
		<input type="submit" value="提交" />
	</form>
	<!-- <img alt="pic" src="upload/images/2020-03-12/202003121737302677625.PNG"> -->
	<img alt="pic" src="${pageContext.request.contextPath }/upload/images/2020-03-12/202003121737302677625.PNG">
</html>
```

# 十一、文件下载

创建AttachmentController

```java
package com.hjh.controller;

import java.io.File;
import java.io.UnsupportedEncodingException;
import java.net.URLEncoder;
import java.util.Date;
import java.util.List;

import javax.servlet.http.HttpServletRequest;

import org.apache.commons.io.FileUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.multipart.MultipartFile;

import com.hjh.domain.Attachment;
import com.hjh.service.AttachmentService;
import com.hjh.utils.PageBean;
import com.hjh.utils.RandomUtils;
import com.hjh.vo.AttachmentVo;

@Controller
@RequestMapping("upload")
public class AttachmentController {

	@Autowired
	private AttachmentService attachmentService;

	/**
	 * 上传一个文件改名字+分文件夹管理 + 保存到数据库
	 */
	@RequestMapping("upload01")
	public String upload04(MultipartFile mf, HttpServletRequest request) {
		long size = mf.getSize();
		String contentType = mf.getContentType();

		// 1、得到文件的相关信息
		String fileName = mf.getOriginalFilename(); // 得到文件的名称

		// 2、得到上传的路径
		String realPath = request.getServletContext().getRealPath("/upload/images");

		// 得到新名字
		String newName = RandomUtils.createRandomStrUseTime(fileName);

		// 得到新文件夹的名字
		String generateDirDateName = RandomUtils.generateDirDateName(); // 根据当前日期获取文件夹名称
		File dirFile = new File(realPath, generateDirDateName);

		// 判断文件夹是否存在
		if (!dirFile.exists()) {
			dirFile.mkdirs();
		}

		// 3、组装文件对象
		File file = new File(dirFile, newName);

		// 4、把文件流写到file
		try {
			mf.transferTo(file);
		} catch (Exception e) {
			e.printStackTrace();
		}

		// 保存到数据库
		AttachmentVo attachmentVo = new AttachmentVo();
		attachmentVo.setContentType(contentType);
		attachmentVo.setSize(Double.valueOf(size));
		attachmentVo.setOldName(fileName);
		attachmentVo.setUploadTime(new Date());
		attachmentVo.setPath("upload/images/" + generateDirDateName + "/" + newName);
		attachmentService.addData(attachmentVo);

		return "success";
	}

	/**
	 * 上传一个文件改名字+分文件夹管理 + 保存到数据库
	 */
	@RequestMapping("queryAll")
	public String queryAll(AttachmentVo attachmentVo, Model model) {
		PageBean pageBean = new PageBean();

		if (null == attachmentVo.getCurrPage()) {
			pageBean.setCurrentPage(1);
		} else {
			pageBean.setCurrentPage(attachmentVo.getCurrPage());
		}

		List<Attachment> users = this.attachmentService.doSerachAllData(attachmentVo, pageBean);
		model.addAttribute("users", users);
		model.addAttribute("pageBean", pageBean);

		return "list";
	}

	/**
	 * 下载文件
	 */
	@RequestMapping("download")
	public ResponseEntity download(Integer id, HttpServletRequest request) {
		Attachment attachment = this.attachmentService.doSearchAttachment(id);
		String path = attachment.getPath();
		// System.out.println(path);

		String realPath = request.getServletContext().getRealPath("/");

		// 组装路径
		String newPath = realPath + path;
		// 读取文件
		File file = new File(newPath);

		// 将下载的文件，封装byte[]
		byte[] bytes = null;

		try {
			bytes = FileUtils.readFileToByteArray(file);
		} catch (Exception e) {
			e.printStackTrace();
		}

		// 创建封装响应头信息的对象
		HttpHeaders header = new HttpHeaders();

		// 封装响应内容类型(APPLICATION_OCTET_STREAM 响应的内容不限定)
		header.setContentType(MediaType.APPLICATION_OCTET_STREAM);

		// 如果文件名为中文，必须使用URLEncoder进行编码
		String oldName = "";
		try {
			oldName = URLEncoder.encode(attachment.getOldName(),"UTF-8");
		} catch (UnsupportedEncodingException e) {
			e.printStackTrace();
		}
		
		// 设置下载的文件的名称
		header.setContentDispositionFormData("attachment", oldName);

		// 创建ResponseEntity对象
		ResponseEntity entity = new ResponseEntity(bytes, header, HttpStatus.CREATED);

		return entity;

	}
}
```

创建index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>一个文件上传【改名】+保存到数据库</h1>
	<form action="upload/upload01.action" method="post" enctype="multipart/form-data">
		选择文件：<input type="file" name="mf" /><br>
		<input type="submit" value="提交" />
	</form>
	<!-- <img alt="pic" src="upload/images/2020-03-12/202003121737302677625.PNG"> -->
	<%-- <img alt="pic" src="${pageContext.request.contextPath }/upload/images/2020-03-12/202003121737302677625.PNG"> --%>
	
	<h2>
		<a href="upload/queryAll.action">查询所有</a>
	</h2>
</html>
```

第一种下载方法：

​	直接通过路径下载

​			优点：不用写java代码

​			缺点：path路径暴露出来了，而且数据库的文件老名字没有用到，实际开发应该下载的老名字。

创建list.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<%
	String path = request.getContextPath();
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
	body{user-select:none;}
</style>
</head>
<body>
	<h2 align="center">用户列表</h2>
	<hr>
	<table align="center" width="80%" border="1" cellpadding="5">
		<tr>
			<th>编号</th>
			<th>旧名称</th>
			<th>文件类型</th>
			<th>文件大小</th>
			<th>上传时间</th>
			<th>下载</th>
		</tr>
		<c:choose>
			<c:when test="${empty users}">
				<tr align="center">
					<td  colspan="6">未搜索到数据</td>
				</tr>
			</c:when>
			<c:otherwise>
				<c:forEach var="x" items="${users}">
					<tr align="center">
						<td>${x.id }</td>
						<td>${x.oldName }</td>
						<td>${x.contentType }</td>
						<td>
							<fmt:formatNumber value="${x.size }" pattern="0"></fmt:formatNumber>
						</td>
						<td id="${x.id }">${x.path }</td>
						<td>
							<fmt:formatDate value="${x.uploadTime }" pattern="yyyy-MM-dd HH:mm:ss"/>
						</td>
						<td>
							<a href="javascript:void(0);" onclick="downloadFile(${x.id })">下载</a>
						</td>
					</tr>
				</c:forEach>
			</c:otherwise>
		</c:choose>
		<tr>
			<td colspan="6">
				<c:choose>
					<c:when test="${pageBean.currentPage==1 }">
						上一页			
					</c:when>
					<c:otherwise>
						<a href="javascript:void(0)" onclick="onPrePage(${pageBean.currentPage-1})">上一页</a>
					</c:otherwise>
				</c:choose>
				&nbsp;&nbsp;&nbsp;
				<c:forEach begin="1" end="${pageBean.totalPage }" var="x">
					<c:choose>
						<c:when test="${x==pageBean.currentPage }">
							${x }
						</c:when>
						<c:otherwise>
							<a href="javascript:void(0)" onclick="onPrePage(${x})">${x}</a>
						</c:otherwise>
					</c:choose>
				</c:forEach>
				&nbsp;&nbsp;&nbsp;
				<c:choose>
					<c:when test="${pageBean.currentPage==pageBean.totalPage }">
						下一页			
					</c:when>
					<c:otherwise>
						<a href="javascript:void(0)" onclick="onPrePage(${pageBean.currentPage+1})">下一页</a>
					</c:otherwise>
				</c:choose>
				&nbsp;&nbsp;&nbsp;
				总共${pageBean.totalPage }页
				&nbsp;&nbsp;&nbsp;
				跳转到: <select onchange="changeSelect(this)">
					<c:forEach begin="1" end="${pageBean.totalPage }" var="x">
						<c:choose>
							<c:when test="${pageBean.currentPage==x }">
								<option value="${x }" selected="selected">${x }</option>
							</c:when>
							<c:otherwise>
								<option value="${x }">${x }</option>
							</c:otherwise>
						</c:choose>
					</c:forEach>
				</select>
			</td>
		</tr>
	</table>
	<script type="text/javascript" src="${pageContext.request.contextPath}/res/js/jquery-3.1.1.min.js"></script>
	<script type="text/javascript">
		function onPrePage(currPage) {
			window.location.href = "queryAll.action?currPage="+currPage;
		}
		
		function changeSelect(obj){
			var currPage = obj.value;
			onPrePage(currPage);
		}
		
		function downloadFile(id) {
			
			var path = $("#" + id).html();
			//alert(path);
			window.location.href = "${pageContext.request.contextPath}/" + path;
		}
	</script>
</body>
</html>
```

第二种方法：

		文件下载的原理：
	把文件ID传到后台，后台根据id查询文件在服务器的相对路径，得到
	服务器的路径，再组装文件的绝对路径，使用IO去读取前使用response
	或ResponseEntity写出去。
创建list.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<%
	String path = request.getContextPath();
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
	body{user-select:none;}
</style>
</head>
<body>
	<h2 align="center">用户列表</h2>
	<hr>
	<table align="center" width="80%" border="1" cellpadding="5">
		<tr>
			<th>编号</th>
			<th>旧名称</th>
			<th>文件类型</th>
			<th>文件大小</th>
			<th>上传时间</th>
			<th>下载</th>
		</tr>
		<c:choose>
			<c:when test="${empty users}">
				<tr align="center">
					<td  colspan="6">未搜索到数据</td>
				</tr>
			</c:when>
			<c:otherwise>
				<c:forEach var="x" items="${users}">
					<tr align="center">
						<td>${x.id }</td>
						<td>${x.oldName }</td>
						<td>${x.contentType }</td>
						<td>
							<fmt:formatNumber value="${x.size }" pattern="0"></fmt:formatNumber>
						</td>
						<td>
							<fmt:formatDate value="${x.uploadTime }" pattern="yyyy-MM-dd HH:mm:ss"/>
						</td>
						<td>
							<a href="javascript:void(0);" onclick="downloadFile(${x.id })">下载</a>
						</td>
					</tr>
				</c:forEach>
			</c:otherwise>
		</c:choose>
		<tr>
			<td colspan="6">
				<c:choose>
					<c:when test="${pageBean.currentPage==1 }">
						上一页			
					</c:when>
					<c:otherwise>
						<a href="javascript:void(0)" onclick="onPrePage(${pageBean.currentPage-1})">上一页</a>
					</c:otherwise>
				</c:choose>
				&nbsp;&nbsp;&nbsp;
				<c:forEach begin="1" end="${pageBean.totalPage }" var="x">
					<c:choose>
						<c:when test="${x==pageBean.currentPage }">
							${x }
						</c:when>
						<c:otherwise>
							<a href="javascript:void(0)" onclick="onPrePage(${x})">${x}</a>
						</c:otherwise>
					</c:choose>
				</c:forEach>
				&nbsp;&nbsp;&nbsp;
				<c:choose>
					<c:when test="${pageBean.currentPage==pageBean.totalPage }">
						下一页			
					</c:when>
					<c:otherwise>
						<a href="javascript:void(0)" onclick="onPrePage(${pageBean.currentPage+1})">下一页</a>
					</c:otherwise>
				</c:choose>
				&nbsp;&nbsp;&nbsp;
				总共${pageBean.totalPage }页
				&nbsp;&nbsp;&nbsp;
				跳转到: <select onchange="changeSelect(this)">
					<c:forEach begin="1" end="${pageBean.totalPage }" var="x">
						<c:choose>
							<c:when test="${pageBean.currentPage==x }">
								<option value="${x }" selected="selected">${x }</option>
							</c:when>
							<c:otherwise>
								<option value="${x }">${x }</option>
							</c:otherwise>
						</c:choose>
					</c:forEach>
				</select>
			</td>
		</tr>
	</table>
	<script type="text/javascript" src="${pageContext.request.contextPath}/res/js/jquery-3.1.1.min.js"></script>
	<script type="text/javascript">
		function onPrePage(currPage) {
			window.location.href = "queryAll.action?currPage="+currPage;
		}
		
		function changeSelect(obj){
			var currPage = obj.value;
			onPrePage(currPage);
		}
		
		function downloadFile(id) {
			window.location.href = "${pageContext.request.contextPath}/upload/download.action?id=" + id;
		}
	</script>
</body>
</html>
```

