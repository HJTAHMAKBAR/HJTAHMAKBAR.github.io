---
title: MySQL学习笔记04
tags:
  - MySQL
categories:
  - - MySQL
  - - 所有
index_img: 'https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/mysql.png'
date: 2021-08-27 17:04:56
---

### 索引

索引是在数据库表的字段上添加的，是为了提高查询效率存在的一种机制。
一张表的一个字段可以添加一个索引，当然，多个字段联合起来也可以添加索引。
索引相当于一本书的目录，是为了缩小扫描范围而存在的一种机制。

对于一本字典来说，查找某个汉字有两种方式：

> 第一种方式：一页一页挨着找，直到找到为止，这种查找方式属于全字典扫描。效率比较低。
> 第二种方式：先通过目录（索引）去定位一个大概的位置，然后直接定位到这个位置，做局域性扫描，缩小扫描的范围，快速的查找。这种查找方式属于通过索引检索，效率较高。

MySQL在查询方面主要就是两种方式：

- 第一种方式：全表扫描
- 第二种方式：根据索引检索。

> 注意：
> 在实际中，汉语字典前面的目录是排序的，按照a b c d e f....排序，
> 为什么排序呢？因为只有排序了才会有区间查找这一说！（缩小扫描范围
> 其实就是扫描某个区间罢了！）

#### 索引的数据结构

在mysql数据库当中索引也是需要排序的，并且这个所以的排序和TreeSet数据结构相同。TreeSet（TreeMap）底层是一个自平衡的二叉树！在mysql当中索引是一个B-Tree数据结构。

遵循左小又大原则存放。采用中序遍历方式遍历取数据。

#### 索引的实现原理

假设有一张用户表：t_user

| id(PK) |   name   | 硬盘物理存储编号 |
| :----: | :------: | :--------------: |
|  100   | zhangsan |      0x1111      |
|  120   |   lisi   |      0x2222      |
|   99   |  wangwu  |      0x8888      |
|   88   | zhaoliu  |      0x9999      |
|  101   |   jack   |      0x6666      |
|   55   |   lucy   |      0x5555      |
|  130   |   tom    |      0x7777      |

- 提醒1：在任何数据库当中主键上都会自动添加索引对象，id字段上自动有索引，因为id是PK。另外在mysql当中，一个字段上如果有unique约束的话，也会自动创建索引对象。

- 提醒2：在任何数据库当中，任何一张表的任何一条记录在硬盘存储上都有一个硬盘的物理存储编号。

- 提醒3：在mysql当中，索引是一个单独的对象，不同的存储引擎以不同的形式存在，MyISAM存储引擎中，索引存储在一个.MYI文件中。在InnoDB存储引擎中索引存储在一个逻辑名称叫做tablespace的当中。在MEMORY存储引擎当中索引被存储在内存当中。不管索引存储在哪里，索引在mysql当中都是一个树的形式存在。（自平衡二叉树：B-Tree）

在mysql当中，主键上，以及unique字段上都会自动添加索引的！！！！

#### 添加索引的条件

1. 条件1：数据量庞大（到底有多么庞大算庞大，这个需要测试，因为每一个硬件环境不同）+
2. 条件2：该字段经常出现在where的后面，以条件的形式存在，也就是说这个字段总是被扫描。
3. 条件3：该字段很少的DML(insert delete update)操作。（因为DML之后，索引需要重新排序。）

建议不要随意添加索引，因为索引也是需要维护的，太多的话反而会降低系统的性能。
建议通过主键查询，建议通过unique约束的字段进行查询，效率是比较高的。

#### 索引的创建与删除

创建索引：

```
mysql> create index emp_ename_index on emp(ename);
给emp表的ename字段添加索引，起名：emp_ename_index
```

删除索引：

```
mysql> drop index emp_ename_index on emp;
将emp表上的emp_ename_index索引对象删除。
```

#### 索引失效

##### 失效的第1种情况：

```
select * from emp where ename like '%T';
```

ename上即使添加了索引，也不会走索引，为什么？
原因是因为模糊匹配当中以“%”开头了！
尽量避免模糊查询的时候以“%”开始。
这是一种优化的手段/策略。

##### 失效的第2种情况：

```
mysql> explain select * from emp where ename = 'KING' or job = 'MANAGER';
```

使用or的时候会失效，如果使用or那么要求or两边的条件字段都要有索引，才会走索引，如果其中一边有一个字段没有索引，那么另一个字段上的索引也会失效。所以这就是为什么不建议使用or的原因。

##### 失效的第3种情况：

```
create index emp_job_sal_index on emp(job,sal);
mysql> explain select * from emp where job = 'MANAGER';
```

使用复合索引的时候，没有使用左侧的列查找，索引失效
什么是复合索引？
两个字段，或者更多的字段联合起来添加一个索引，叫做复合索引。

##### 失效的第4种情况：

```
mysql> create index emp_sal_index on emp(sal);
mysql> explain select * from emp where sal+1 = 800;
```

在where当中索引列参加了运算，索引失效。
mysql> create index emp_sal_index on emp(sal);

##### 失效的第5种情况：

	explain select * from emp where lower(ename) = 'smith';
在where当中索引列使用了函数

##### 失效的第6...

##### 失效的第7...

#### 索引的分类

- 单一索引：一个字段上添加索引。
- 复合索引：两个字段或者更多的字段上添加索引。
- 主键索引：主键上添加索引。
- 唯一性索引：具有unique约束的字段上添加索引。
- .....

注意：唯一性比较弱的字段上添加索引用处不大。

### 视图

view:站在不同的角度去看待同一份数据。

创建视图对象：

```
create view dept2_view as select * from dept2;
```

删除视图对象：

```
drop view dept2_view;
```

注意：只有DQL语句才能以view的形式创建。

```
create view view_name as 这里的语句必须是DQL语句;
```

#### 视图的操作

我们可以面向视图对象进行增删改查，对视图对象的增删改查，会导致原表被操作！（视图的特点：通过对视图的操作，会影响到原表数据。）

//面向视图查询

```
select * from dept2_view; 
```

// 面向视图插入

```
insert into dept2_view(deptno,dname,loc) values(60,'SALES', 'BEIJING');
```

// 查询原表数据

```
mysql> select * from dept2;
```

// 面向视图删除

```
mysql> delete from dept2_view;
```

// 查询原表数据

	mysql> select * from dept2;
	Empty set (0.00 sec)

// 创建视图对象

```
create view 
	emp_dept_view
as
	select 
		e.ename,e.sal,d.dname
	from
		emp e
	join
		dept d
	on
		e.deptno = d.deptno;
```

// 查询视图对象

```
mysql> select * from emp_dept_view;
```

// 面向视图更新

```
update emp_dept_view set sal = 1000 where dname = 'ACCOUNTING';
```

// 原表数据被更新

```
mysql> select * from emp;
```

#### 视图的作用

> 方便，简化开发，利于维护

假设有一条非常复杂的SQL语句，而这条SQL语句需要在不同的位置上反复使用。
每一次使用这个sql语句的时候都需要重新编写，很长，很麻烦，怎么办？
可以把这条复杂的SQL语句以视图对象的形式新建。
在需要编写这条SQL语句的位置直接使用视图对象，可以大大简化开发。
并且利于后期的维护，因为修改的时候也只需要修改一个位置就行，只需要修改视图对象所映射的SQL语句。

面向视图开发的时候，使用视图的时候可以像使用table一样。
可以对视图进行增删改查等操作。视图不是在内存当中，视图对象也是存储在硬盘上的，不会消失。

视图对应的语句只能是DQL语句。
但是视图对象创建完成之后，可以对视图进行增删改查等操作。

#### 增删改查（CRUD）

- C:Create（增）			
- R:Retrive（查：检索）
- U:Update（改）			
- D:Delete（删）

### DBA常用命令

- #### 数据导出

  - 导出数据库到D盘

    ```
    mysqldump xxxxxx>D:\xxxxxx.sql -uyourname -pyourpassword
    ```

  - 导出数据库指定的表到D盘

    ```
    mysqldump xxxxxx yourtable>D:\xxxxxx.sql -uyourname -pyourpassword
    ```

- #### 数据导入

  - 登录到mysql数据库服务器上

  - 创建数据库xxxxxx

    ```
    create database xxxxxx;
    ```

  - 使用数据库xxxxxx

    ```
    use xxxxxx
    ```

  - 使用D盘下的sql文件初始化数据库

    ```
    source D:\xxxxxx.sql
    ```

### 数据库设计三范式

- 第一范式：要求任何一张表必须有主键，每一个字段原子性不可再分。
- 第二范式：建立在第一范式的基础之上，要求所有非主键字段完全依赖主键，不要产生部分依赖。
- 第三范式：建立在第二范式的基础之上，要求所有非主键字段直接依赖主键，不要产生传递依赖。

设计数据库表的时候，按照以上的范式进行，可以避免表中数据的冗余，空间的浪费。

#### 第一范式

> 最核心，最重要的范式，所有表的设计都需要满足。必须有主键，并且每一个字段都是原子性不可再分。

#### 第二范式

> 建立在第一范式的基础之上，要求所有非主键字段必须完全依赖主键，不要产生部分依赖。

#### 第三范式

> 第三范式建立在第二范式的基础之上要求所有非主键字典必须直接依赖主键，不要产生传递依赖。

### 表的设计

- 一对多：一对多，两张表，多的表加外键！
- 多对多：多对多，三张表，关系表两个外键！
- 一对一：一张表字段太多，太庞大。这个时候要拆分表。一对一，外键唯一！

#### 总结

> 数据库设计三范式是理论上的。实践和理论有的时候有偏差。

最终的目的都是为了满足客户的需求，有的时候会拿冗余换执行速度。

因为在sql当中，表和表之间连接次数越多，效率越低。（笛卡尔积）

有的时候可能会存在冗余，但是为了减少表的连接次数，这样做也是合理的，