[TOC]

# 1、mybatis概述

## 1.1什么是mybatis

MyBatis是支持普通SQL查询，存储过程和高级映射的优秀持久层框架。MyBatis消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索。MyBatis使用简单的XML或注解用于配置和原始映射，将接口和Java的POJOs（Plan Old Java Objects，普通的Java对象）映射成数据库中的记录.

​      每个MyBatis应用程序主要都是使用SqlSessionFactory实例的，一个SqlSessionFactory实例可以通过SqlSessionFactoryBuilder获得。SqlSessionFactoryBuilder可以从一个xml配置文件或者一个预定义的配置类的实例获得。

​    用xml文件构建SqlSessionFactory实例是非常简单的事情。推荐在这个配置中使用类路径资源（classpath resource)，但你可以使用任何Reader实例，包括用文件路径或file://开头的url创建的实例。MyBatis有一个实用类----Resources，它有很多方法，可以方便地从类路径及其它位置加载资源。

## 1.2**orm工具的基本思想**

无论是用过的hibernate,mybatis,你都可以法相他们有一个共同点：

\1. 从配置文件(通常是XML配置文件中)得到 SqlSessionfactory.

\2. 由sessionfactory  产生 SqlSession

\3. 在session 中完成对数据的增删改查和事务提交等.

\4. 在用完之后关闭session 。

\5. 在Java 对象和 数据库之间有做mapping 的配置文件，也通常是xml 文件。

## 1.3**mybatis和JDBC的比较**

![图片1](img\图片1.png)

# 2、mybatis入门配置

**2.1下载相关jar包**

​	注意引入数据库的jar

**2.2编写mybatis的核心配置文件mybatis.cfg.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 引入头文件 -->
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- 整个mybtais的配置中心 -->
<configuration>
	<!-- 数据源环境配置  default默认数据源的ID -->
	<!-- <properties resource="classpath:mybatis.cfg.xml"></properties> -->
	<environments default="mysql">
		<environment id="mysql">
			<!-- 配置mybatis的事务管理器 -->
			<transactionManager type="JDBC"></transactionManager>
			<dataSource type="POOLED">
				<!-- <property name="driver" value="${driver}"/>
				<property name="url" value="${url}"/>
				<property name="username" value="${username}"/>
				<property name="password" value="${password}"/> -->
				<property name="driver" value="com.mysql.jdbc.Driver"/>
				<property name="url" value="jdbc:mysql://localhost:3306/0412user"/>
				<property name="username" value="root"/>
				<property name="password" value="123456"/>
			</dataSource>
		</environment>
	</environments>
	
	<!-- 配置映射 -->
	<mappers>
		<mapper resource="com/sxt/mapper/UserMapper.xml"/>
	</mappers>
	

</configuration>
```

**2.3创建数据库**

**2.4创建实体类User**

**2.5创建UserDAO**

**2.6建立UserDaoImpl实现类**

**2.7创建UserMapper.xml**

**2.8测试**

**2.9Log4j的配置log4j.properties**

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

```xml
//配置日志的输出方式
// 在mybatis.cfg.xml中配置
  <settings>

    <setting name="logImpl" value="LOG4J" />

  </settings>

```

# 3、**搭建mybatis框架的基本步骤**

a)导入相关jar包

b)编写核心配置文件（配置数据库连接的相关信息以及配置了mapper映射文件）

c)编写dao操作

d)编写mapper映射文件

e)编写实体类

# 4、核心配置文件mybatis.cfg.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 引入头文件 -->
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- 整个mybtais的配置中心 -->
<configuration>
	<settings>
	<!-- 让log4j去记录sql日志 -->
		<setting name="logImpl" value="LOG4J"/>
	</settings>


	<!-- <properties resource="classpath:mybatis.cfg.xml"></properties> -->
	
		<!-- 数据源环境配置  default默认数据源的ID
		environments 指mybatis可以配置多个环境   default指向默认的环境
		每个SqlSessionFactory对应一个环境environment
		 -->
	<environments default="mysql">
	<!-- 环境 
	可以配置多个数据源  但是能使用一个
	 -->
		<environment id="mysql">
			<!-- 配置mybatis的事务管理器
			type 的配置
				JDBC – 这个配置直接使用JDBC 的提交和回滚功能。它依赖于从数据源获得连接来管理
					事务的生命周期。
				MANAGED – 这个配置基本上什么都不做。它从不提交或者回滚一个连接的事务。而是让
					容器（例如：Spring 或者J2EE 应用服务器）来管理事务的生命周期
			-->
			<transactionManager type="JDBC"></transactionManager>
			<!-- 这个环境里面的数据源
			type：POOLED
					 这个数据源的实现缓存了JDBC 连接对象，用于避免每次创建新的数据库连接时都初始
					化和进行认证，加快程序响应。并发WEB 应用通常通过这种做法来获得快速响应。
				 UNPOOLED:每次连接数据库时都创建新的连接【不推荐】
				 JNDI:使用外部的数据源 如tomcat可以配置数据源在这里使用
			 -->
			<dataSource type="POOLED">
				<!-- <property name="driver" value="${driver}"/>
				<property name="url" value="${url}"/>
				<property name="username" value="${username}"/>
				<property name="password" value="${password}"/> -->
				<property name="driver" value="com.mysql.jdbc.Driver"/>
				<property name="url" value="jdbc:mysql://localhost:3306/0412user"/>
				<property name="username" value="root"/>
				<property name="password" value="123456"/>
			</dataSource>
		</environment>
	</environments>
	
	<!-- 配置映射 -->
	<mappers>
		<mapper resource="com/sxt/mapper/UserMapper.xml"/>
           //注解的时候使用class
	</mappers>

</configuration>
```

## 4.1**优化配置文件**

导入properties配置文件

> a）在src下加入db.properties配置文件

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/0412user
username=root
password=123456
```

> b)在mybatis.cfg.xml中添加 properties标签

![图片2](img\图片2.png)



![图片3](img\图片3.png)

## 4.2**别名的优化**

![图片4](img\图片4.png)

注意，这个代码只能放到enviroment之前

# 5、Mapper.xml详解

mapper	

​		|--namespace:命名空间  指当mytbais的API去访问crud的方法时会从某一个命名空间里面去找想应的ID对执行

insert

​		|-- id 唯一标记 【一般会和dao里面的接口名保持一样】

​		|-- parameterType 传参类型 insert(id的值,参数类型)

​		|-- useGeneratedKeys 是否使用自动增长【了解】

update

​		|-- id 唯一标记 【一般会和dao里面的接口名保持一样】

​		|-- parameterType 传参类型 insert(id的值,参数类型)	

​	

delete

​		|-- id 唯一标记 【一般会和dao里面的接口名保持一样】

​		|-- parameterType 传参类型 insert(id的值,参数类型)	

 

select 

​		|-- id 唯一标记 【一般会和dao里面的接口名保持一样】

​		|-- parameterType 传参类型 insert(id的值,参数类型)	

​		|--parameterMap 当传参数类型为Map集合时要设置【和parameterType不能共存】

​		|--resultType 查询返回的结果的类型可以使用基本数据类型的包装类和自定义类型如Integer和 User

​		|-- resultMap 自定义返回的类型

## **5.1mybatis占位符的处理**

1，占位符一：#{xxx}

|--  PreparedStatement预编译sql语句

|-- xxx表达式的写法

|--  参数类型为javabean类，xxx表达式必须和javabean中属性对应的get方法名字一样

|--  参数类型为简单类型，xxx表达式随表写，保持和参数的名字一致

2，占位符二：${xxx}

|--  Statement拼接sql语句

|--  xxx表达式的写法

|--  参数类型为javabean类，xxx表达式必须和javabean中属性对应的get方法名字一样

|--  参数类型为简单类型，xxx表达式执行能写${value}

# 6、模糊查询



```java
//模糊查询的方式
       1、不推荐select id as uid,name as uname,address as addr,birthday as birth from sys_user where name LIKE #{name}
        List<User> users = userMapper.queryLike("%张三1%");

       2、推荐select id as uid,name as uname,address as addr,birthday as birth from sys_user where name LIKE "%"#{name}"%"

       3、(Oracle中没有concat方法)select id as uid,name as uname,address as addr,birthday as birth from sys_user where name LIKE "%${value}%"

       4、select id as uid,name as uname,address as addr,birthday as birth from sys_user where name LIKE concat('%',#{value},'%')
```

# 7、分页的实现

**7.1分析mysql的分页语句:limit startIndex,pageNum**

```xml
<!-- 查询所有用户 -->
2.	<select id="selectAll" parameterType="Map" resultType="User">
3.		select * from user limit #{startIndex},#{pageSize}
4.	</select>
```

```java



1.//分页查询
2.public List<User> getAll(int currentPage,int pageSize) throws IOException{
3.	SqlSession session=MyBatisUtil.getSession();
4.	Map<String,Integer> map = new HashMap<String,Integer>();
5.	map.put("startIndex", (currentPage-1)*pageSize);
6.	map.put("pageSize", pageSize);
7.	List<User> list = session.selectList("cn.sxt.entity.UserMapper.selectAll",map);
8.	session.close();
9.	return list;
10.}
//注意：不用为参数设置类，可以采用map结构来解决这个问题。
```

**7.2通过RowBounds来实现分页**(**不常用**)

Mapper文件不用做任何改变

```xml
<select id="getAll" resultType="User">
	select * from user
</select>
```

Dao中需要新建RowBounds对象

```java
//使用mybatis里面提供的API



//分页查询
public List<User> getAll(int currentPage,int pageSize) throws IOException{
	SqlSession session=MyBatisUtil.getSession();
	RowBounds rowBounds = new RowBounds((currentPage-1)*pageSize,pageSize);
	List<User> list=session.selectList("cn.sxt.entity.UserMapper.getAll",null,rowBounds);
	session.close();
	return list;
}

```

**7.3分页插件的使用【重点】(阿里云)**

1、下载jar并导入

pagehelper-5.1.4.jar、jsqlparser-1.2.jar

2、修改mybtais.cfg.xml

​	让mybatis知道有pageHelper

添加拦截器插件

![图片5](img\图片5.png)

3、测试

![图片6](img\图片6.png)

**4、说明【必看】**

实际开发中，在dao里面是不会写PageHepler的，所以一般的分页的拦截在service层里面

![图片7](img\图片7.png)

```java
public class MybatisTest {

    static SqlSession session = MybatisUtils.openSession();
    static UserMapper userMapper = session.getMapper(UserMapper.class);

    @Test
    public void init() {
    	// 使用阿里云分页插件(掌握)
        /*分页查询6--实战*/
    	//实际开发的时候要把以下代码写在service层里面
        PageBean pageBean = new PageBean();
        pageBean.setPageSize(8);
        pageBean.setCurrentPage(2);
        User user = new User();
        user.setName("三6");
        
        Page<User> page = PageHelper.startPage(pageBean.getCurrentPage(), pageBean.getPageSize());
        
        
		List<User> list = userMapper.pageInfo(user);
		for (User u : list) {
			System.out.println(u);
		}
        System.out.println("总条数：" + page.getTotal());
        System.out.println("分页查询成功");

     }
}
```

```xml
<mapper namespace="com.hjh.mapper.UserMapper">
    <select id="pageInfo" resultType="User" parameterType="User"> 
        select * from sys_user where name like "%"#{name}"%"
    </select>
</mapper>
```

# 8、属性名和字段名不一样的问题

**8.1使用sql  as 去改名字，改成和实体类里面一样的名字**

![图片8](img\图片8.png)



![图片9](img\图片9.png)



![图片10](img\图片10.png)**8.2使用resultMap把实体类里面的属性和结果集去映射**(查询的结果集与属性不一致采用解决方法)

Mapper.xml的写法

![图片11](img\图片11.png)



# 9、SQL片段

**目地：提高sql的复用性**

![图片12](img\图片12.png)

# 10、动态sql

## 10.1概述

MyBatis 的强大特性之一便是它的动态 SQL。如果你有使用 JDBC 或其他类似框架的经验，你就能体会到根据不同条件拼接 SQL 语句有多么痛苦。拼接的时候要确保不能忘了必要的空格，还要注意省掉列名列表最后的逗号。利用动态 SQL 这一特性可以彻底摆脱这种痛苦。

​    通常使用动态 SQL 不可能是独立的一部分,MyBatis 当然使用一种强大的动态 SQL 语言来改进这种情形,这种语言可以被用在任意的 SQL 映射语句中。

​    动态 SQL 元素和使用 JSTL 或其他类似基于 XML 的文本处理器相似。在 MyBatis 之前的版本中,有很多的元素需要来了解。MyBatis 3 大大提升了它们,现在用不到原先一半的元素就可以了。MyBatis 采用功能强大的基于 OGNL 的表达式来消除其他元素。

if

choose (when, otherwise)

where, set

foreach

## 10.2where

作用：添加一个where的sql字符串，如果where里面没有sql mybtais去自动删除这个where

![图片13](img\图片13.png)

## 10.3if

作用：判断是否满足某个条件 ，如果满足就向sql里面去拼接 if里面的sql

![图片14](img\图片14.png)

## 10.4choose（when  otherwise）

相关于if()else if() else{}

![图片15](img\图片15.png)

## 10.5set

做修改时使用  加入set关键字

![图片16](img\图片16.png)

## 10.6foreach

遍历

![图片17](img\图片17.png)

# 11、关联表处理(xml)

## 11.1多对一的处理

![11](img\11.png)



![12](img\12.png)



![13](img\13.png)

方法一：

![14](img\14.png)



![15](img\15.png)

方法二：

![16](img\16.png)

![17](img\17.png)

## **11.2处理一对多的关系**

![18](img\18.png)

![19](img\19.png)

# 12、使用注解实现mybatis

![21](img\21.png)

![22](img\22.png)

![23](img\23.png)

![24](img\24.png)

![25](img\25.png)

![26](img\26.png)

![27](img\27.png)

![28](img\28.png)

# 13、缓存

![29](img\29.png)



![30](img\30.png)

# 14、逆向工程

帮我们生成Pojo  mapper  mapper.xml

## 14.1什么是逆向工程

MyBatis的一个主要的特点就是需要程序员自己编写sql，那么如果表太多的话，难免会很麻烦，所以mybatis官方提供了一个逆向工程，可以针对单表自动生成mybatis执行所需要的代码（包括mapper.xml、mapper.java、po..）。一般在开发中，常用的逆向工程方式是通过数据库的表生成代码。

## 14.2使用逆向工程

1、使用MyBatis的逆向工程，需要导入逆向工程的jar包，我用的是mybatis-generator-core-1.3.2.jar

2、创建项目并导包(创建普通的java项目即可)

3、创建配置文件

![图片32](img\图片32.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE generatorConfiguration  
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"  
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
	<!-- 数据库驱动 -->
	<classPathEntry location="lib/mysql-connector-java-5.1.25-bin.jar" />
	<context id="DB2Tables" targetRuntime="MyBatis3">
		<commentGenerator>
			<property name="suppressDate" value="true" />
			<!-- 是否去除自动生成的注释 true：是 ： false:否 -->
			<property name="suppressAllComments" value="true" />
		</commentGenerator>
		<!--数据库链接URL，用户名、密码 -->
		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
			connectionURL="jdbc:mysql://localhost/0412user" userId="root"
			password="123456">
		</jdbcConnection>
		<javaTypeResolver>
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>
		<!-- 生成模型的包名和位置   实体
		targetProject:生成的源代码放到位置  可以是相对位置 也可以绝对位置
		targetProject=‘src’指本项目的src
		-->
		<javaModelGenerator targetPackage="com.sxt.domain"
			targetProject="src">
			<property name="enableSubPackages" value="true" />
			<property name="trimStrings" value="true" />
		</javaModelGenerator>
		<!-- 生成映射文件的包名和位置  生成xml的位置-->
		<sqlMapGenerator targetPackage="com.sxt.mapping"
			targetProject="src">
			<property name="enableSubPackages" value="true" />
		</sqlMapGenerator>
		<!-- 生成DAO的包名和位置 -->
		<javaClientGenerator type="XMLMAPPER"
			targetPackage="com.sxt.mapper" targetProject="src">
			<property name="enableSubPackages" value="true" />
		</javaClientGenerator>
		<!-- 要生成的表 tableName是数据库中的表名或视图名 domainObjectName是实体类名 -->
		<table tableName="sys_user" domainObjectName="User"
			enableCountByExample="false" enableUpdateByExample="false"
			enableDeleteByExample="false" enableSelectByExample="false"
			selectByExampleQueryId="false">
		</table>
		<table tableName="sys_dept" domainObjectName="Dept"
			enableCountByExample="false" enableUpdateByExample="false"
			enableDeleteByExample="false" enableSelectByExample="false"
			selectByExampleQueryId="false">
		</table>
		<table tableName="sys_emp" domainObjectName="Emp"
			enableCountByExample="false" enableUpdateByExample="false"
			enableDeleteByExample="false" enableSelectByExample="false"
			selectByExampleQueryId="false">
		</table>
	</context>
</generatorConfiguration>  
/*
	从上面的配置文件中可以看出，配置文件主要做的几件事是： 

  1，连接数据库，这是必须的，要不然怎么根据数据库的表生成代码呢？ 

  2，指定要生成代码的位置，要生成的代码包括po类，mapper.xml和mapper.java 

  3，指定数据库中想要生成哪些表

*/
```

4、创建GeneratorTest去生成

```java

public class GeneratorTest {

	 public static void generator() throws Exception{
	        List<String> warnings = new ArrayList<String>();
	        boolean overwrite = true;
	        //指定 逆向工程配置文件
	        File configFile = new File("config/generatorConfig.xml"); 
	        ConfigurationParser cp = new ConfigurationParser(warnings);
	        Configuration config = cp.parseConfiguration(configFile);
	        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
	        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
	                callback, warnings);
	        myBatisGenerator.generate(null);
	    } 
	
	public static void main(String[] args) {
		try {
			generator();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

5、效果

![图片33](img\图片33.png)

6、把文件生成到其它项目里面

修改 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE generatorConfiguration  
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"  
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
	<!-- 数据库驱动 -->
	<classPathEntry location="lib/mysql-connector-java-5.1.25-bin.jar" />
	<context id="DB2Tables" targetRuntime="MyBatis3">
		<commentGenerator>
			<property name="suppressDate" value="true" />
			<!-- 是否去除自动生成的注释 true：是 ： false:否 -->
			<property name="suppressAllComments" value="true" />
		</commentGenerator>
		<!--数据库链接URL，用户名、密码 -->
		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
			connectionURL="jdbc:mysql://localhost/0412user" userId="root"
			password="123456">
		</jdbcConnection>
		<javaTypeResolver>
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>
		<!-- 生成模型的包名和位置   实体
		targetProject:生成的源代码放到位置  可以是相对位置 也可以绝对位置
		targetProject=‘src’指本项目的src
		-->
		<javaModelGenerator targetPackage="com.sxt.domain"
			targetProject="D:\Workspaces\ADV0412_mybatis\13_mybtais_erp\src">
			<property name="enableSubPackages" value="true" />
			<property name="trimStrings" value="true" />
		</javaModelGenerator>
		<!-- 生成映射文件的包名和位置  生成xml的位置-->
		<sqlMapGenerator targetPackage="com.sxt.mapping"
			targetProject="D:\Workspaces\ADV0412_mybatis\13_mybtais_erp\src">
			<property name="enableSubPackages" value="true" />
		</sqlMapGenerator>
		<!-- 生成DAO的包名和位置 -->
		<javaClientGenerator type="XMLMAPPER"
			targetPackage="com.sxt.mapper" targetProject="D:\Workspaces\ADV0412_mybatis\13_mybtais_erp\src">
			<property name="enableSubPackages" value="true" />
		</javaClientGenerator>
		<!-- 要生成的表 tableName是数据库中的表名或视图名 domainObjectName是实体类名 -->
		<table tableName="sys_user" domainObjectName="User"
			enableCountByExample="false" enableUpdateByExample="false"
			enableDeleteByExample="false" enableSelectByExample="false"
			selectByExampleQueryId="false">
		</table>
		<table tableName="sys_dept" domainObjectName="Dept"
			enableCountByExample="false" enableUpdateByExample="false"
			enableDeleteByExample="false" enableSelectByExample="false"
			selectByExampleQueryId="false">
		</table>
		<table tableName="sys_emp" domainObjectName="Emp"
			enableCountByExample="false" enableUpdateByExample="false"
			enableDeleteByExample="false" enableSelectByExample="false"
			selectByExampleQueryId="false">
		</table>
	</context>
</generatorConfiguration> 
//修改三处src的路径
```

7、把table里面的所有属性改成true之生生成的代码

```java

public interface UserMapper {
	//查询 满足example条件的数据总条数
    int countByExample(UserExample example);

    int deleteByExample(UserExample example);

    int deleteByPrimaryKey(Integer id);

    int insert(User record);

    int insertSelective(User record);
    //查询 满足example条件的数据
    List<User> selectByExample(UserExample example);

    User selectByPrimaryKey(Integer id);

    //批量选择更新 满足example条件的数据总条数
    int updateByExampleSelective(@Param("record") User record, @Param("example") UserExample example);

    //批量全部更新 满足example条件的数据总条数
    int updateByExample(@Param("record") User record, @Param("example") UserExample example);

    int updateByPrimaryKeySelective(User record);

    int updateByPrimaryKey(User record);
}
```

