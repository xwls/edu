# Oracle基础操作

[TOC]

## SQL语句

- SQL 语句是不区分大小写的.
- SQL 语句可以是一行，也可以是多行.
- 关键字不能缩写或分裂成两行.
- 子句通常放在单独的行中.
- 缩进是为了增强可读性.
- 在 SQL Developer工具中, 执行一条SQL 语句, SQL 语句可视情况使用`;`结尾. 若要同时执行多条SQL 语句，每一句都要以`;`结束.
- 在 SQL*Plus工具中, 每一句 SQL 语句都要以`;`结尾.

## SQL*Plus

### 简介

- SQL*Plus是Oracle提供的一个命令行数据库管理工具。它允许用户从提示行以交互方式输入SQL语句，这些语句被直接发送给数据库，执行后的结果返回到屏幕上

### 打开方式

- 方式一、`Win+R`输入`sqlplus`，然后回车
- 方式二、从Oracle安装目录找到SQLPlus

## 简单命令

### 页面显示相关

- 设置每行显示的长度：

```sql
set linesize 长度;
```

- 设置每页显示记录的数量：

```sql
set pagesize 页数;
```

### 用户相关

- 切换登录用户：

```sql
conn 用户名/口令[AS SYSDBA|SYSOPER];
```

- 查看当前登录用户：

```sql
SHOW USER;
```
- 查询当前用户下的所有表：

```sql
SELECT * FROM tab;
```

## SELECT语句

### 基本查询语句

#### 语法

```sql
SELECT *|{[DISTINCT] column|expression [alias],...}
FROM    table;
```

- SELECT 确定处理那些列
- FROM 确定处理那个表
- 关键字不区分大小写

#### 查询所有列

```sql
SQL> SELECT * FROM departments;
```

- 查询结果：

```
DEPARTMENT_ID DEPARTMENT_NAME      MANAGER_ID LOCATION_ID
------------- -------------------- ---------- -----------
           10 Administration              200        1700
           20 Marketing                   201        1800
           30 Purchasing                  114        1700
```

#### 查询特定的列

```sql
SQL> SELECT DEPARTMENT_ID,DEPARTMENT_NAME FROM departments;
```

- 查询结果：

```
DEPARTMENT_ID DEPARTMENT_NAME
------------- --------------------
           10 Administration
           20 Marketing
           30 Purchasing
```

此查询可以查询指定的列，列名之间用逗号隔开。

#### 算术表达式

- 使用算数运算符，可以创建数字和日期数据表达式

```sql
SELECT last_name, salary, salary + 300 FROM   employees;
```

​	查询结果

```
LAST_NAME        SALARY SALARY+300
------------ ---------- ----------
King              24000      24300
Kochhar           17000      17300
De Haan           17000      17300
```

- 混合运算

```sql
SELECT last_name,salary,salary*12+100 FROM employees;
```

​	查询结果

```
LAST_NAME                SALARY SALARY*12+100
-------------------- ---------- -------------
King                      24000        288100
Kochhar                   17000        204100
De Haan                   17000        204100
```

#### 空值

- 空值是指不可用、未分配、未知或不适用的值.
- 空值不同于零和空格. 

在`Employees`表中，`commission_pct`字段会有空值

```sql
SELECT last_name, job_id, salary, commission_pct FROM employees;
```

​	查询结果

```
LAST_NAME            JOB_ID                   SALARY COMMISSION_PCT
-------------------- -------------------- ---------- --------------
King                 AD_PRES                   24000
Kochhar              AD_VP                     17000
De Haan              AD_VP                     17000
```

- 空值参与任何运算得到的结果，仍是空值

```sql
SELECT last_name, 12*salary*commission_pct FROM employees;
```
​	查询结果

```
LAST_NAME            12*SALARY*COMMISSION_PCT
-------------------- ------------------------
King
Kochhar
De Haan
```

## 字段别名

### 特点和功能

- 重命名列标题
- 有助于计算

### 注意

1. 紧跟在列名的后面（可以在列名和列别名之间加`AS`关键字）
2. 如果列别名包含特殊字符、空格或大小写，则需要使用双引号`""`

### 例子

#### 简单列别名

```sql
SELECT last_name AS name ,commission_pct comm FROM employees;
```

​	查询结果

```
NAME                  COMM
--------------- ----------
King
Kochhar
De Haan
Hunold
```

#### 使用双引号

```sql
SELECT last_name AS "Name",salary*12 AS "Annual Salary" FROM employees; 
```

​	查询结果

```
Name            Annual Salary
--------------- -------------
King                   288000
Kochhar                204000
De Haan                204000
```

## 连接运算符

### 特点和功能

1. 将列和字符串连接到其他列
2. 由两条竖线`||`表示
3. 创建一个合并列，是字符表达式

### 注意

1. 文字是指 SELECT 列表中的字符、数字和日期.
2. 日期和字符文字必须包含在单引号中.
3. 在输出结果中，每个返回行都会将每个字符串显示一次.

### 例子

#### 简单连接

```sql
SELECT	last_name||job_id AS "Employees" FROM 	employees;
```

​	查询结果

```
Employees
--------------------
AbelSA_REP
AndeSA_REP
AtkinsonST_CLERK
```

#### 文本字符串

```sql
SELECT last_name ||' is a '||job_id AS "Employee Details" FROM   employees;
```

​	查询结果

```
Employee Details
-------------------------
Abel is a SA_REP
Ande is a SA_REP
Atkinson is a ST_CLERK
```

#### 引用定界符（q）

```sql
SELECT department_name || q'[ Department's Manager Id: ]'
       || manager_id 
       AS "Department and Manager" 
FROM departments;
```

​	查询结果

```
Department and Manager
--------------------------------------------
Administration Department's Manager Id: 200
Marketing Department's Manager Id: 201
Purchasing Department's Manager Id: 114
```

## DISTINCT 关键字

### 概述

>  默认会查询所有行，包括重复行。有时我们需要去除重复行，这就需要`DISTINCT`关键字。

### 例子

#### 包含重复行

```sql
SELECT department_id FROM employees;
```

​	查询结果：

```
DEPARTMENT_ID
-------------
           90
           90
           90
           60
           60
           60
           60
           60
          100
```

#### 去除重复行

```sql
SELECT DISTINCT department_id FROM employees;
```

​	查询结果：

```
DEPARTMENT_ID
-------------
          100
           30

           90
           20
           70
          110
```

## 查看表结构-DESCRIBE关键字

### 概述

> 有时候我们需要知道的是一张表的结构而不是内容，包括表有哪些字段，每个字段的数据类型，这时我们可以使用`DESCRIBE`关键字，此关键字可以简写为`DESC`.

### 例子

```sql
DESC employees;
```

​	执行结果

```
SQL> DESC employees;
 名称                                      是否为空? 类型
 ----------------------------------------- -------- ---------------------------

 EMPLOYEE_ID                               NOT NULL NUMBER(6)
 FIRST_NAME                                         VARCHAR2(20)
 LAST_NAME                                 NOT NULL VARCHAR2(25)
 EMAIL                                     NOT NULL VARCHAR2(25)
 PHONE_NUMBER                                       VARCHAR2(20)
 HIRE_DATE                                 NOT NULL DATE
 JOB_ID                                    NOT NULL VARCHAR2(10)
 SALARY                                             NUMBER(8,2)
 COMMISSION_PCT                                     NUMBER(2,2)
 MANAGER_ID                                         NUMBER(6)
 DEPARTMENT_ID                                      NUMBER(4)
```

**在列的类型中主要有以下几种类型：**

- `NUMBER(6)` --> 表示是数字，长度为6
- `VARCHAR2(20)` --> 表示是字符串，最多容纳20个长度
- `DATE` --> 表示日期
- `NUMBER(8,2)` --> 表示数字，小数位占2位，整数位6位，一共8位

## 使用脚本

### 概述

> 有时我们需要批量执行很多SQL语句，这时一般不会把全部SQL都写在SQLPlus，而是将sql语句写入文件，这个包含SQL语句的文件叫做SQL脚本

### 执行脚本

```sql
SQL>@test.sql
```

