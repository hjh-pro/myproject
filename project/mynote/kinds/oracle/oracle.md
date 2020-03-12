**Oracle**

[TOC]

# 1、PL/SQL语法

## 1.1、程序语法

```plsql
declare 
	说明部分 (变量说明、游标申明、异常说明)
begin
	语句序列 （DML语句）
exception
	异常处理语句
end;
```

例子输出hello world

```plsql
begin
       dbms_output.put_line('hello world'); 
end;
```

```plsql
-- 基本数据类型的变量和常量
declare       
       name varchar2(10);   -- 声明变量
       age number(3) := 22; -- 声明常量
begin
       name := 'zhangsan';
       dbms_output.put_line(name);
       dbms_output.put_line(age);
end;
/*
       输出结果：
            zhangsan
            22
*/
```

```plsql
-- 引用数据类型的变量  ： 引用表中的列
-- into: 将左边的值赋值给右边
declare       
       pname emp.ename%type;
begin
       select t.ename into pname from emp t where t.empno = 4378;
       dbms_output.put_line(pname);
end;
-- 输出结果：马玉
```

```plsql
-- 记录类型的变量，可以对应java中的对象类型变量
declare       
       obj emp%rowtype;
begin
       select t.* into obj from emp t where t.empno = 4378;
       dbms_output.put_line(obj.ename || '  ' || obj.sal);
end;
-- 输出结果：马玉  2131
```

