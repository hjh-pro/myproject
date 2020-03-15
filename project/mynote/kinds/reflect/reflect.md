**反射基础和注解**

[TOC]

# 1、反射的基本概念

**JAVA反射机制**是在***\*运行状态\****中，对于任意一个实体类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性；这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制。

**框架**：半成品项目。我们可以再框架的基础上进行软件开发，简化编码。

**反射**：框架的基础，也是框架的灵魂。将类的各个组成部分封装成其他的对象。

**反射的好处**：可以在程序运行过程中，操作这些对象。可以提高程序扩展性和复用性，可以解耦。

## 1.1 Class

```
Class代表类的实体，在运行的Java应用程序中表示类和接口。在这个类中提供了很多有用的方法，这里对他们简单的分类介绍。
```

***\*获得类相关的方法\****

| ***\*方法\****             | ***\*用途\****                                         |
| -------------------------- | ------------------------------------------------------ |
| asSubclass(Class<U> clazz) | 把传递的类的对象转换成代表其子类的对象                 |
| getClassLoader()           | 获得类的加载器                                         |
| getClasses()               | 返回一个数组，数组中包含该类中所有公共类和接口类的对象 |
| getDeclaredClasses()       | 返回一个数组，数组中包含该类中所有类和接口类的对象     |
| forName(String className)  | 根据类名返回类的对象                                   |
| getName()                  | 获得类的完整路径名字                                   |
| newInstance()              | 创建类的实例                                           |
| getPackage()               | 获得类的包                                             |
| getSimpleName()            | 获得类的名字                                           |
| getSuperclass()            | 获得当前类继承的父类的名字                             |
| getInterfaces()            | 获得当前类实现的类或是接口                             |
| .class                     | 获取当前对象的类                                       |

```java
package com.hjh.reflect;

import com.hjh.reflect.domain.User;
import org.junit.Test;

/**
 * @author HJH
 * @date 2020-03-15 12:50
 */
public class Test01 {


    /**
     * 获得类相关的方法
     * @throws ClassNotFoundException
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    @Test
    public void test01() throws ClassNotFoundException, IllegalAccessException, InstantiationException {
        // 获取User类对象(方法1)
        Class<?> class1 = Class.forName("com.hjh.reflect.domain.User");
        System.out.println(class1); // class com.hjh.reflect.domain.User
        System.out.println(class1.getSimpleName()); // User
        System.out.println(class1.getName()); // com.hjh.reflect.domain.User

        // 获取User类对象(方法2)
        Class<User> class2 = User.class;
        System.out.println(class2); // class com.hjh.reflect.domain.User

        // 获取User类对象(方法3)
        User user = new User();
        Class<? extends User> class3 = user.getClass();
        System.out.println(class3); // class com.hjh.reflect.domain.User

        System.out.println("=====================");

        // 创建类的实例
        User user1 = class2.newInstance();
        System.out.println(user1);

        // 获得类的完整路径名字
        System.out.println(class3.getName()); // com.hjh.reflect.domain.User

        // 获得类的名字
        System.out.println(class2.getSimpleName()); // User

        System.out.println("=====================");
        // 返回一个数组，数组中包含该类中所有类和接口类的对象
        Class<?>[] declaredClasses = class2.getDeclaredClasses();
        for (Class<?> aClass : declaredClasses) {
            System.out.println(aClass);
        }

        //获得类的加载器（一般读取资源文件会用到）
        System.out.println(class2.getClassLoader()); // sun.misc.Launcher$AppClassLoader@18b4aac2
    }

}
```

***\*获得类中\*******\*字段\*******\*相关的方法\****

| ***\*方法\****                | ***\*用途\****         |
| ----------------------------- | ---------------------- |
| getField(String name)         | 获得某个公有的字段对象 |
| getFields()                   | 获得所有公有的字段对象 |
| getDeclaredField(String name) | 获得某个字段对象       |
| getDeclaredFields()           | 获得所有字段对象       |

```java
package com.hjh.reflect;

import com.hjh.reflect.domain.User;
import org.junit.Test;

import java.lang.reflect.Field;

/**
 * @author HJH
 * @date 2020-03-15 12:50
 */
public class Test02 {


    /**
     * 获得类中字段的相关方法
     * @throws ClassNotFoundException
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    @Test
    public void test01() throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchFieldException {
        // 获取User类对象(方法2)
        Class<User> class2 = User.class;
        System.out.println(class2); // class com.hjh.reflect.domain.User

        // 获取一个公共的字段
        Field address = class2.getField("address");
        System.out.println(address.getName());
        //Field address2 = class2.getField("name");
        //System.out.println(address2.getName()); // 报错，无法获取私有字段

        System.out.println("=======================");
        // 获取所有公共属性的方法
        Field[] fields = class2.getFields();
        for (Field field : fields) {
            System.out.println(field.getName());
        }

        System.out.println("=======================");

        // 获取单个私有属性
        Field id = class2.getDeclaredField("id");
        System.out.println(id.getName());

        System.out.println("=======================");
        // 获取所有属性的方法
        Field[] declaredFields = class2.getDeclaredFields();
        for (Field field : declaredFields) {
            System.out.println(field.getName());
        }


    }

}
```

***\*获得类中注解相关的方法\****

| ***\*方法\****                          | ***\*用途\****                         |
| --------------------------------------- | -------------------------------------- |
| getAnnotation(Class<A> annotationClass) | 返回该类中与参数类型匹配的公有注解对象 |

```java
package com.hjh.reflect;

import com.hjh.reflect.domain.User;
import jdk.nashorn.internal.runtime.logging.Logger;
import org.junit.Test;

import java.lang.reflect.Field;

/**
 * @author HJH
 * @date 2020-03-15 12:50
 */
public class Test03 {


    /**
     * 获得类上的注解
     * @throws ClassNotFoundException
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    @Test
    public void test01() throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchFieldException {
        // 获取User类对象(方法2)
        Class<User> class2 = User.class;
        System.out.println(class2); // class com.hjh.reflect.domain.User

        // 获得注解（如果类上不存在该注解，打印为null，用于判断注解是否存在）
        Logger annotation = class2.getAnnotation(Logger.class);
        System.out.println(annotation); // @jdk.nashorn.internal.runtime.logging.Logger(name=)

    }

}
```

***\*获得类中构造器相关的方法\****

| ***\*方法\****                                     | ***\*用途\****                         |
| -------------------------------------------------- | -------------------------------------- |
| getConstructor(Class...<?> parameterTypes)         | 获得该类中与参数类型匹配的公有构造方法 |
| getConstructors()                                  | 获得该类的所有公有构造方法             |
| getDeclaredConstructor(Class...<?> parameterTypes) | 获得该类中与参数类型匹配的构造方法     |
| getDeclaredConstructors()                          | 获得该类所有构造方法                   |

```java
package com.hjh.reflect;

import com.hjh.reflect.domain.User;
import jdk.nashorn.internal.runtime.logging.Logger;
import org.junit.Test;

import java.lang.reflect.Constructor;
import java.lang.reflect.Method;

/**
 * @author HJH
 * @date 2020-03-15 12:50
 */
public class Test04 {


    /**
     * 获得类里面的构造方法
     * @throws ClassNotFoundException
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    @Test
    public void test01() throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchFieldException, NoSuchMethodException {
        // 获取User类对象(方法2)
        Class<User> class2 = User.class;

        // 获取所有公共的构造方法
        Constructor<?>[] constructors = class2.getConstructors();
        for (Constructor<?> constructor : constructors) {
            // 获取构造方法参数的个数
            int parameterCount = constructor.getParameterCount();
            System.out.println(parameterCount+"个参数的构造方法："+constructor);
        }

        System.out.println("==========================");
        // 获取所有的构造方法
        Constructor<?>[] declaredConstructors = class2.getDeclaredConstructors();
        for (Constructor<?> declaredConstructor : declaredConstructors) {
            // 获取构造方法参数的个数
            int parameterCount = declaredConstructor.getParameterCount();
            System.out.println(parameterCount+"个参数的构造方法："+declaredConstructor);
        }
        System.out.println("==========================");
        // 获得指定参数类型的构造方法
        Constructor<User> declaredConstructor = class2.getDeclaredConstructor(Integer.class, String.class, Integer.class);
        int parameterCount = declaredConstructor.getParameterCount();
        System.out.println(parameterCount+"个参数的构造方法："+declaredConstructor);
    }

}
```

***\*获得类中方法相关的方法\****

| ***\*方法\****                                             | ***\*用途\****         |
| ---------------------------------------------------------- | ---------------------- |
| getMethod(String name, Class...<?> parameterTypes)         | 获得该类某个公有的方法 |
| getMethods()                                               | 获得该类所有公有的方法 |
| getDeclaredMethod(String name, Class...<?> parameterTypes) | 获得该类某个方法       |
| getDeclaredMethods()                                       | 获得该类所有方法       |

```java
package com.hjh.reflect;

import com.hjh.reflect.domain.User;
import org.junit.Test;

import java.lang.reflect.Constructor;
import java.lang.reflect.Method;

/**
 * @author HJH
 * @date 2020-03-15 12:50
 */
public class Test05 {


    /**
     * 获得类里面的方法
     * 对于参数中的Class<?>... parameterTypes： ...代表不定长参数，可以是0个，可以是多个（本质是一个数组）
     * @throws ClassNotFoundException
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    @Test
    public void test01() throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchFieldException, NoSuchMethodException {
        // 获取User类对象(方法2)
        Class<User> class2 = User.class;

        // 获取所有公共的方法（包括继承的所有方法）
        Method[] methods = class2.getMethods();
        for (Method method : methods) {
            System.out.println(method);
        }
        System.out.println("==========================");

        // 获得该类里面的所有方法(不包括继承的所有方法)
        Method[] declaredMethods = class2.getDeclaredMethods();
        for (Method declaredMethod : declaredMethods) {
            System.out.println(declaredMethod);
        }

        System.out.println("==========================");
        // 获取指定参数类型的方法
        Method setName = class2.getDeclaredMethod("setName", String.class);
        System.out.println(setName);
        System.out.println(setName.getName());
    }

}

```

***\*类中其他重要的方法（不常用）\****

| ***\*方法\****                                               | ***\*用途\****                |
| ------------------------------------------------------------ | ----------------------------- |
| isAnnotation()                                               | 如果是注解类型则返回true      |
| isAnnotationPresent(Class<? extends Annotation> annotationClass) | 判断这个方法是否有该注解      |
| isArray()                                                    | 如果是一个数组类则返回true    |
| isEnum()                                                     | 如果是枚举类则返回true        |
| isInstance(Object obj)                                       | 如果obj是该类的实例则返回true |
| isInterface()                                                | 如果是接口类则返回true        |

```java
package com.hjh.reflect;

import com.hjh.reflect.domain.User;
import jdk.nashorn.internal.runtime.logging.Logger;
import org.junit.Test;

import java.lang.reflect.Method;

/**
 * @author HJH
 * @date 2020-03-15 12:50
 */
public class Test06 {


    /**
     * 获取其他的方法
     * 对于参数中的Class<?>... parameterTypes： ...代表不定长参数，可以是0个，可以是多个（本质是一个数组）
     * @throws ClassNotFoundException
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    @Test
    public void test01() throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchFieldException, NoSuchMethodException {
        // 获取User类对象(方法2)
        Class<User> class2 = User.class;

        // 如果是注解类型则返回true
        System.out.println(class2.isAnnotation()); // false

        // 如果是接口类则返回true
        System.out.println(class2.isInterface()); // false
        System.out.println(Logger.class.isAnnotation()); // true

        // 判断这个方法是否有该注解
        System.out.println(class2.isAnnotationPresent(Logger.class)); // true
    }

}
```

## 1.2 Field

[Field](https://developer.android.google.cn/reference/java/lang/reflect/Field)代表类的成员变量。***\*成员变量（字段）和成员属性是两个概念。比如：当一个User类中有一个name变量，那么这个时候我们就说它有name这个字段。但是如果没有getName和setName这两个方法，那么这个类就没有name属性。反之，如果这个类拥有getAge和setAge这两个方法，不管有没有age字段，我们都认为它有age这个属性\****

| ***\*方法\****                | ***\*用途\****               |
| ----------------------------- | ---------------------------- |
| get(Object obj)               | 获得obj中对应的属性值        |
| set(Object obj, Object value) | 设置obj中对应属性值          |
| SetAccessible                 | 暴力反射，忽略访问权限修饰符 |

```java
package com.hjh.reflect;

import com.hjh.reflect.domain.User;
import jdk.nashorn.internal.runtime.logging.Logger;
import org.junit.Test;

import java.lang.reflect.Field;

/**
 * @author HJH
 * @date 2020-03-15 12:50
 */
public class Test07 {


    /**
     * 获取和设置属性的值
     * @throws ClassNotFoundException
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    @Test
    public void test01() throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchFieldException, NoSuchMethodException {
        // 获取User类对象(方法2)
        Class<User> class2 = User.class;

        // 获取属性为name的值
        Field name = class2.getDeclaredField("name");
        // 每一个类都会有一个字节码class文件，name属性不知道获取哪个对象里面的name，所以需要创建一个对象
        User user = new User(1,"hjh",22);

        // 无法获得私有属性的值，需要通过暴力反射
        name.setAccessible(true);

        Object o = name.get(user);
        System.out.println(o); // hjh

        System.out.println("======================");

        // 设置属性
        Field age = class2.getDeclaredField("age");
        // 暴力反射
        age.setAccessible(true);
        // 第一个参数为被修改的对象，第二参数为修改的值
        age.set(user,18);
        System.out.println(user); // User{id=1, name='hjh', age=18}
    }

}
```

## 1.3 Method

[Method](https://developer.android.google.cn/reference/java/lang/reflect/Method)代表类的方法（不包括构造方法）。

| ***\*方法\****                     | ***\*用途\****                           |
| ---------------------------------- | ---------------------------------------- |
| invoke(Object obj, Object... args) | 传递object对象及参数调用该对象对应的方法 |
| getName                            | 获取方法名                               |
| SetAccessible                      | 暴力反射，忽略访问权限修饰符             |

```java
package com.hjh.reflect;

import com.hjh.reflect.domain.User;
import org.junit.Test;

import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

/**
 * @author HJH
 * @date 2020-03-15 12:50
 */
public class Test08 {


    /**
     * 获取和调用方法
     * @throws ClassNotFoundException
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    @Test
    public void test01() throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchFieldException, NoSuchMethodException, InvocationTargetException {
        // 获取User类对象(方法2)
        Class<User> class2 = User.class;

        // 获取无参的方法并调用
        Method getName = class2.getDeclaredMethod("getName");
        User user = new User(1,"hjh",22);
        getName.setAccessible(true);
        Object invoke = getName.invoke(user); // 获取方法调用的返回值
        System.out.println(invoke); // hjh

        System.out.println("==========================");
        // 获取有参的方法并调用
        Method setAge = class2.getDeclaredMethod("setAge",Integer.class);
        // 第一个参数是对象，第二个参数是实参
        Object invoke1 = setAge.invoke(user, 16);
        System.out.println(invoke1); // null
        System.out.println(user); // User{id=1, name='hjh', age=16}
    }

}
```

Invoke方法的用处：SpringAOP在切面方法执行的前后进行某些操作，就是使用的invoke方法。动态代理设计模式中，使用的也是invoke。

## 1.4 Constructor(用的比较少)

| ***\*方法\****                  | ***\*用途\****             |
| ------------------------------- | -------------------------- |
| newInstance(Object... initargs) | 根据传递的参数创建类的对象 |

Constructor类在实际开发中使用极少，几乎不会使用Constructor。因为：Constructor违背了Java的一些思想，比如：私有构造不让用户去new对象；单例模式保证全局只有一个该类的实例。而Constructor则可以破坏这个规则

```java
package com.hjh.reflect;

import com.hjh.reflect.domain.User;
import org.junit.Test;

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

/**
 * @author HJH
 * @date 2020-03-15 12:50
 */
public class Test09 {


    /**
     * 通过构造方法创建对象
     * @throws ClassNotFoundException
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    @Test
    public void test01() throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchFieldException, NoSuchMethodException, InvocationTargetException {
        /**
         * 通过class对象里面的newInstance()创建对象:功能简单，局限性大
         * 只能通过公共无参的构造方法去创建对象
         */
        Class<User> userClass = User.class;
        User user = userClass.newInstance();
        //System.out.println(user); // User{id=null, name='null', age=null}

        // 通过有参构造方法创建对象
        Constructor<User> declaredConstructor = userClass.getDeclaredConstructor(Integer.class, String.class, Integer.class);
        User user1 = declaredConstructor.newInstance(111, "qqq", 22);
        System.out.println(user1); // User{id=111, name='qqq', age=22}

        // 通过有参构造方法创建对象，通过私有构造方法创建对象，但是这个极不安全
        Constructor<User> declaredConstructor1 = userClass.getDeclaredConstructor(String.class, Integer.class);
        declaredConstructor1.setAccessible(true);
        User user2 = declaredConstructor1.newInstance("ls", 25);
        System.out.println(user2); // User{id=null, name='ls', age=25}
    }

}
```

# 2、注解

## 2.1概念

概念：注解就是说明程序的一个标识，给计算机看的。

注释：用文字描述程序，给程序员看的。

定义：也叫作元数据，是一种代码级别的说明。它是JDK1.5引入的一个特性，是一种特殊的接口。它可以声明在类、字段、方法、变量、参数、包等前面，作为一个描述去使用。

 

注意：注解本身并没有任何功能，仅仅起到一个标识性的作用，我们只是通过反射去获取到注解，再根据是否有这个注解、注解中的一些属性去判断和执行哪种业务逻辑。

 

作用分类：

编写文档：通过代码中标识的注解生成文档（Swagger）

代码分析：通过代码里的注解对代码进行分析（逻辑判断）

编译检查：通过代码里对应的注解让编译器实现基本的编译检查（Override，Deprecated，FunctionalInterface）

JDK中预定义的一些注解

Override：检测该注解标识的方法是否继承自父类（接口）

Deprecated：标识方法、类、字段等已经过时，后续的版本可能会将其移除

SuppressWarnings：压制警告，指定这个级别下的警告全部不显示

## 2.2 自定义注解

在实际开发中，可能存在一些代码极其复杂或者复用性很低的业务逻辑，比如：

导出Excel(第一列姓名，第二列性别，写死的)(用户信息和商品信息不相干的两个，就可以使用自定义注解)、缓存、将返回值转json、事务等等。

## 2.3 格式

元注解

Public @interface 注解名称 {

属性列表

}

本质：注解本质上是一个接口，该接口事实上默认继承自Annotation接口

## 2.4 属性

事实上是接口中的抽象方法

\1. 如果定义了属性，在使用属性的时候需要给属性赋值。如果有默认值可以不赋值

\2. 如果只有一个属性需要赋值，并且这个属性名称是value，则可以省略value

\3. 数组赋值时，需要使用{}包起来。如果数组中只有一个元素，则大括号可以省略

属性中的返回值类型有下列取值：

\* 基本数据类型

\*  String

\* 枚举

\* 注解

\* 以上类型的数组

## 2.5 元注解

用于描述注解的注解

**@Target**：描述该注解作用范围

ElementType取值：

Type：作用于类

METHOD：作用于方法

FIELD：作用于字段

**@Retention**：描述注解被保留的阶段

RetentionPolicy.RUNTIME：当前描述的注解，会保留到class字节码文件中，并被jvm读取到
**@Documented**：描述注解是否被抽取到api文档中

**@Inherited**：描述注解是否可以被继承

```
总结：

反射：就是在程序运行中对Class、对象等进行一系列的操作；
注解：注解就是个标识，相当于是给程序看的注释，注释本身不存在功能，需要通过反射去进行某些判断，根据判断结果去执行对应的逻辑，这个过程就是给注解赋予功能的过程。

反射和自定义注解是框架的基础，写框架看源码则是架构师的基础。
```

# 3、程序在计算机中的三个阶段：

![程序在计算机中的三个阶段](img\程序在计算机中的三个阶段.png)

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package com.hjh.utils;

import com.hjh.annotation.CacheAnnotation;
import java.lang.reflect.Method;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

public final class MethodUtils {
    private static Map<String, Object> cacheReuturnVal = new ConcurrentHashMap();
    public static final String CACHE_SEPARATE = "::";

    private MethodUtils() {
    }

    public static Object invokeMethod(Object obj, String methodName, Object... params) {
        Class<?> aClass = obj.getClass();
        Object invoke = null;

        try {
            long startTime = System.currentTimeMillis();
            if (null == params && params.length == 0) {
                Method method = aClass.getDeclaredMethod(methodName);
                method.setAccessible(true);
                CacheAnnotation cacheAnnotation = (CacheAnnotation)method.getAnnotation(CacheAnnotation.class);
                String key;
                if (null != cacheAnnotation) {
                    key = cacheAnnotation.key();
                    Object o = cacheReuturnVal.get(key);
                    if (null != o) {
                        return o;
                    }
                }

                invoke = method.invoke(obj);
                if (null != cacheAnnotation) {
                    key = cacheAnnotation.key();
                    cacheReuturnVal.put(key, invoke);
                }
            } else {
                int size = params.length;
                Class<?>[] paramClass = new Class[size];

                for(int i = 0; i < size; ++i) {
                    paramClass[i] = params[i].getClass();
                }

                Method method = aClass.getDeclaredMethod(methodName, paramClass);
                method.setAccessible(true);
                CacheAnnotation cacheAnnotation = (CacheAnnotation)method.getAnnotation(CacheAnnotation.class);
                String key;
                Object param;
                String cacheKey;
                if (null != cacheAnnotation) {
                    key = cacheAnnotation.key();
                    param = params[0];
                    cacheKey = key + "::" + param;
                    Object value = cacheReuturnVal.get(cacheKey);
                    if (null != value) {
                        return value;
                    }
                }

                invoke = method.invoke(obj, params);
                if (null != cacheAnnotation) {
                    key = cacheAnnotation.key();
                    param = params[0];
                    cacheKey = key + "::" + param;
                    cacheReuturnVal.put(cacheKey, invoke);
                }
            }

            long var17 = System.currentTimeMillis();
        } catch (Exception var15) {
            var15.printStackTrace();
        }

        return invoke;
    }

    public static Object getObject(Class<?> c) {
        Object obj = null;
        if (null != c) {
            try {
                String name = c.getSimpleName();
                obj = c.newInstance();
            } catch (Exception var3) {
                var3.printStackTrace();
            }
        }

        return obj;
    }
}

```

