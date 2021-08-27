---
title: MySQL学习笔记01
tags:
  - MySQL
categories:
  - - MySQL
  - - 所有
index_img: 'https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/mysql.png'
date: 2021-08-27 10:27:05
---

### 数据库、数据库管理系统、SQL相关概念

#### 数据库：

英文单词DataBase，简称DB。按照一定格式存储数据的一些文件的组合。

顾名思义：存储数据的仓库，实际上就是一堆文件。

这些文件中存储了具有特定格式的数据。

#### 数据库管理功能

DataBaseManagement，简称DBMS。
数据库管理系统是专门用来管理数据库中数据的，数据库管理系统可以
对数据库当中的数据进行增删改查。

常见的数据库管理系统：
MySQL、Oracle、MS SqlServer、DB2、sybase等....

#### SQL：结构化查询语言

程序员需要学习SQL语句，程序员通过编写SQL语句，然后DBMS负责执行SQL语句，最终来完成数据库中数据的增删改查操作。

SQL是一套标准，程序员主要学习的就是SQL语句，这个SQL在mysql中可以使用，同时在Oracle中也可以使用，在DB2中也可以使用。

#### 三者之间的关系

```
DBMS--执行--> SQL --操作--> DB
```

### 表(Table)

数据库当中最基本的单元是表：table。

数据库当中是以表格的形式表示数据的，因为表比较直观。

任何一张表都有行和列：

- 行（row）：被称为数据/记录。
- 列（column）：被称为字段。

每一个字段都有：字段名、数据类型、约束等属性。

- 字段名可以理解，是一个普通的名字，见名知意就行。

- 数据类型：字符串，数字，日期等。
- 约束：约束也有很多，其中一个叫做唯一性约束，这种约束添加之后，该字段中的数据不能重复。

### SQL语句的分类

SQL语句有很多，最好进行分门别类，这样更容易记忆。分为：

- DQL（Data Query Language）
- DML（Data Manipulation Language）
- DDL（Data Definition Language）
- TCL（Transactional Control Language）
- DCL（Data Control Language）

| 语句种类 |     名称     |                    介绍                     |          代表          |
| :------: | :----------: | :-----------------------------------------: | :--------------------: |
|   DQL    | 数据查询语句 |     凡是带有select关键字的都是查询语句      |       select...        |
|   DML    | 数据操作语句 |    凡是对表当中的数据进行增删改的都是DML    | insert、delete、update |
|   DDL    | 数据定义语句 |   凡是带有create、drop、alter的都是DDL。    |  create、drop、alter   |
|   TCL    | 事务控制语句 | 用于快速原型开发、脚本编程、GUI和测试等方面 |    commit、rollback    |
|   DCL    | 数据控制语言 |       授予或回收访问数据库的某种特权        |     grant、revoke      |