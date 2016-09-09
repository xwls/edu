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

| 类型           | 说明                     |
| ------------ | ---------------------- |
| NUMBER(6)    | 表示是数字，长度为6             |
| VARCHAR2(20) | 表示是字符串，最多容纳20个长度       |
| DATE         | 表示日期                   |
| NUMBER(8,2)  | 表示数字，小数位占2位，整数位6位，一共8位 |

## 使用脚本

### 概述

> 有时我们需要批量执行很多SQL语句，这时一般不会把全部SQL都写在SQLPlus，而是将sql语句写入文件，这个包含SQL语句的文件叫做SQL脚本

### 执行脚本

```sql
SQL>@test.sql
```

