---
title: MySQL学习笔记02
tags:
  - MySQL
categories:
  - - MySQL
  - - 所有
index_img: 'https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/mysql.png'
date: 2021-08-27 16:04:30
---

### 连接查询

从一张表中单独查询，称为单表查询。

emp表和dept表联合起来查询数据，从emp表中取员工名字，从dept表中取部门名字。
这种跨表查询，多张表联合起来查询数据，被称为连接查询。

#### 连接查询的分类

根据语法的年代分类：

- SQL92：1992年的时候出现的语法
- SQL99：1999年的时候出现的语法

根据表连接的方式分类：

- 内连接：
  - 等值连接
  - 非等值连接
  - 自连接
- 外连接：
  - 左外连接（左连接）
  - 右外连接（右连接）
- 全连接

#### 笛卡尔积现象

当两张表进行连接查询，没有任何条件限制的时候，最终查询结果条数，是
两张表条数的乘积，这种现象被称为：笛卡尔积现象。（笛卡尔发现的，这是
一个数学现象。）

#### 如何避免笛卡尔积现象（匹配次数仍不变，只是避免现象）

连接时加条件，满足这个条件的记录被筛选出来！

思考：最终查询的结果条数是14条，但是匹配的过程中，匹配的次数减少了吗？
还是56次，只不过进行了四选一。次数没有减少。	

注意：通过笛卡尔积现象得出，表的连接次数越多效率越低，尽量避免表的
连接次数。

#### 内连接之等值连接

> 案例：查询每个员工所在部门名称，显示员工名和部门名？
> emp e和dept d表进行连接。条件是：e.deptno = d.deptno

```
SQL92语法：
	select 
		e.ename,d.dname
	from
		emp e, dept d
	where
		e.deptno = d.deptno;
```

sql92的缺点：结构不清晰，表的连接条件，和后期进一步筛选的条件，都放到了where后面。

```
SQL99语法：
	select 
		e.ename,d.dname
	from
		emp e
	join
		dept d
	on
		e.deptno = d.deptno;
			//默认就是内连接
			//inner可以省略（带着inner可读性更好！！！一眼就能看出来是内连接）
	select 
		e.ename,d.dname
	from
		emp e
	inner join
		dept d
	on
		e.deptno = d.deptno; // 条件是等量关系，所以被称为等值连接。
```

sql99优点：表连接的条件是独立的，连接之后，如果还需要进一步筛选，再往后继续添加where。

```
	SQL99语法：
		select 
			...
		from
			a
		join
			b
		on
			a和b的连接条件
		where
			筛选条件
```

#### 外连接VS内连接

内连接

```
内连接：（A和B连接，AB两张表没有主次关系。平等的。）
select 
	e.ename,d.dname
from
	emp e
join
	dept d
on
	e.deptno = d.deptno; //内连接的特点：完成能够匹配上这个条件的数据查询出来。
```

外连接（右外连接）

```
select 
	e.ename,d.dname
from
	emp e 
right join 
	dept d
on
	e.deptno = d.deptno;

// outer是可以省略的，带着可读性强。
select 
	e.ename,d.dname
from
	emp e 
right outer join 
	dept d
on
	e.deptno = d.deptno;
```

> right代表什么：表示将join关键字右边的这张表看成主表，主要是为了将
> 这张表的数据全部查询出来，捎带着关联查询左边的表。
> 在外连接当中，两张表连接，产生了主次关系。

```
外连接（左外连接）：
select 
	e.ename,d.dname
from
	dept d 
left join 
	emp e
on
	e.deptno = d.deptno;

// outer是可以省略的，带着可读性强。
select 
	e.ename,d.dname
from
	dept d 
left outer join 
	emp e
on
	e.deptno = d.deptno;
```

> 带有right的是右外连接，又叫做右连接。
> 带有left的是左外连接，又叫做左连接。
> 任何一个右连接都有左连接的写法。
> 任何一个左连接都有右连接的写法。

思考：外连接的查询结果条数一定是 >= 内连接的查询结果条数？正确。

### 子查询

select语句中嵌套select语句，被嵌套的select语句称为子查询。

#### 子查询可以出现的地方

```
select
	..(select).
from
	..(select).
where
	..(select).
```



### 关于DQL语句的总结

编写顺序

```
	select 
		...
	from
		...
	where
		...
	group by
		...
	having
		...
	order by
		...
	limit
		...
```

执行顺序

```
1.from
2.where
3.group by
4.having
5.select
6.order by
7.limit..
```

### 表的创建

建表属于DDL语句，DDL包括：create drop alter。

```
	create table 表名(字段名1 数据类型, 字段名2 数据类型, 字段名3 数据类型);

	create table 表名(
		字段名1 数据类型, 
		字段名2 数据类型, 
		字段名3 数据类型
	);
```

### MySQL中的数据类型

|     数据类型     |       描述       |                             特点                             |                     优点                      |              缺点              |
| :--------------: | :--------------: | :----------------------------------------------------------: | :-------------------------------------------: | :----------------------------: |
| varchar(最长255) | 可变长度的字符串 |              会根据实际的数据长度动态分配空间。              |             比较智能，节省空间。              |   需要动态分配空间，速度慢。   |
|  char(最长255)   |    定长字符串    |   不管实际的数据长度是多少，分配固定长度的空间去存储数据。   |         不需要动态分配空间，速度快。          | 使用不当可能会导致空间的浪费。 |
|   int(最长11)    |  数字中的整数型  |                      等同于java的int。                       |                                               |                                |
|      bigint      |  数字中的长整型  |                     等同于java中的long。                     |                                               |                                |
|      float       | 单精度浮点型数据 |                    等同于java中的float。                     |                                               |                                |
|      double      | 双精度浮点型数据 |                    等同于java中的double。                    |                                               |                                |
|       date       |    短日期类型    |                      默认格式：%Y-%m-%d                      |                                               |                                |
|     datetime     |    长日期类型    |                 默认格式：%Y-%m-%d %h:%i:%s                  |                                               |                                |
|       clob       |    字符大对象    |  最多可以存储4G的字符串。比如：存储一篇文章，存储一个说明。  | 超过255个字符的都要采用CLOB字符大对象来存储。 |                                |
|       blob       |   二进制大对象   | 专门用来存储图片、声音、视频等流媒体数据。往BLOB类型的字段上插入数据的时候，例如插入一个图片、视频等。 |                                               |       需要使用IO流才行。       |

