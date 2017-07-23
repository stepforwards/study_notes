---
title: JavaWeb基础第三天 
tags: sql,jdbc
grammar_cjkRuby: true
---

[TOC]

## mysql的安装(详见文档)
## mysql绑定eclipse(详见文档)
## SQL分类
- 数据定义语言：简称DDL(Data Definition Language)，用来定义数据库对象：数据库，表，列等。关键字：create，alter，drop等
- 数据操作语言：简称DML(Data Manipulation Language)，用来对数据库中表的记录进行更新。关键字：insert，delete，update等
- 数据控制语言：简称DCL(Data Control Language)，用来定义数据库的访问权限和安全级别，及创建用户。
- 数据查询语言：简称DQL(Data Query Language)，用来查询数据库中表的记录。关键字：select，from，where等

## 常用sql语句

### 数据库操作

- 创建数据库 <font color="red">**create database 库名**</font> 或者 <font color="red">**create database 库名 character set 编码**</font>
- 查看编码<font color="red">**show create database 库名**</font>
- 删除数据库<font color="red">**drop database 库名**</font>
- 使用数据库 <font color="red">use 库名</font>
- 查看当前操作的库 <font color="red">**select databases**</font>


### 表中数据类型

- 数据库中表中的数据类型


|   分类  |  类型名称   |  说明   |
| :---: | :---: | :---: |
|  整数类型   | tinyInt    |  很小的整数   |
|     |   smallint  |   小的整数  |
|     |   mediumint  |   中等大小的整数  |
|     |  int(integer)   |   普通大小的整数  |
| 小数类型    |  float   |   单精度浮点数  |
|     |   double  |   双精度浮点数  |
|     |   decimal（m,d）  |  压缩严格的定点数,m表示总位数,d表示小数点几位   |
|日期类型     |   year  |   YYYY 1901~2155  |
|     |   time  |   HH:MM:SS -838:59:59~838:59:59  |
|     |   date  |   YYYY-MM-DD 1000-01-01~9999-12-3  |
|     |   datetime  | YYYY­MM­DD HH:MM:SS 1000­01­01 00:00:00~ 9999­
12­31 23:59:59    |
|     |  timestamp   |    YYYY­MM­DD HH:MM:SS 1970~01~01 00:00:01
UTC~2038­01­19 03:14:07UTC |
|   文本、二进制类型  |   CHAR(M)  |  M为0~255之间的整数   |
|     | VARCHAR(M)    |  M为0~65535之间的整数   |
|     | TINYBLOB    |   允许长度0~255字节  |
|     |   BLOB  |  允许长度0~65535字节   |
|     |   MEDIUMBLOB  | 允许长度0~167772150字节    |
|     |  LONGBLOB   |  允许长度0~4294967295字节   |
|     |   TINYTEXT  | 允许长度0~255字节    |
|     |   TEXT  |   允许长度0~65535字节  |
|     |   MEDIUMTEXT  |    允许长度0~167772150字节 |
|     | LONGTEXT    |   允许长度0~4294967295字节  |
|     |   VARBINARY(M)  |   允许长度0~M个字节的变长字节字符串 |
|     |   BINARY(M)  |   允许长度0~M个字节的定长字节字符串  |


#### 常用数据类型

|   类型  |  描述   |
| :---: | :---: |
|  int   |  整型   |
|    double |  浮点型   |
|  varchar   |  字符型   |
|  date   |  日期型   |


### 常用约束

|   约束名称  |  sql表示   | 说明   |
| :---: | :---: | :---: |
|  主键约束    |  primary key   |  唯一性，非空性   |
|   唯一约束  |  unique   |  唯一性，可以空，但只能有一个   |
| 默认约束    |   default  | 该数据的默认值    |
|  自增长   | auto_increment    |   一般用于主键自动增长  |
|  非空约束   |  not null   |   数据不能为空  |

### 数据库中表的操作

- 创建表 

``` stylus
	create table 表名(
		字段名 类型(长度) [约束],
		字段名 类型(长度) [约束],
		字段名 类型(长度) [约束]
	);
```
- 查看当前数据库中的表<font color="red">** show tables**</font>
- 查看表结构 <font color=“red” >**desc 表名**</font>
- 修改表
	- 添加一列 <font color="red" >**alter table 表名 add 字段名 类型(长度) [约束]**</font>
	- 修该列的类型(长度，类型) <font color="red">alter table 表名modify 要修改的字段名 类型(长度) [约束]</font>
	- 修改列名<font color="red"> **alter table 表名 change 旧列名 新列名 类型(长度) [约束]**</font>
	- 删除表中的列 <font color="red">**alter table 表名 drop 列名**</font>
	- 修改表名<font color="red"> **rename table 表名 to 新表名**</font>
	- 修改表的字符集 <font color="red">**alter table 表名 character set 编码**</font>
	- 查看当前表结构<font color="red">**show create table 表名**</font>


### 数据库表中记录的操作

- 插入记录<font color="red">** insert into 表名(列名1,列名2,列名3……) values(值1,值2,值3……) 或者 insertinto 表名 values(值1,值2,值3……)**</font>
- 插入记录中文乱码问题 set names gbk;
- 修改表记录 update 表名 set 字段名=值, 字段名=值, 字段名=值
- 修改表记录带条件 update 表名 set字段名=值, 字段名=值, 字段名=值…… where 条件
- 删除表记录带条件 delete from 表名 where 条件
- 删除表记录不带条件 delete from 表名; 或者 truncate table 表名;



### 数据库表中记录的查询操作

- 查询所有 `**select * from product;**`
- 查询列 `**select pname ，price from product；**`
- 查询所有商品信息使用表别名: `**select * from product as p;**`
- 查询商品使用列别名 `**select pname as p from product**`
- 查询去掉重复 `**select distinct(price) from product;**`
- 将所有的商品的价格+10进行显示 `**select pname,price+10 from product;**`


#### 条件查询

|  运算符分类   |  运算符表示   |  说明   |
| :---: | :---: | :---: |
|  比较运算符   |  < <= >= = <>   |  大于、小于、大于(小于)等于、不等于   |
|     |  BETWEEN …AND…   |   显示在某一区间的值(含头含尾) |
|     |   IN(set)  | 显示在in列表中的值，例：in(100,200)    |
|     |   LIKE 通配符  |  模糊查询，Like语句中有两个通配符:% 用来匹配多个字符；例first_name like ‘a%’;_ 用来匹配一个字符。例first_name like ‘a_’;   |
|     |  IS NULL   |  判断是否为空 is null; 判断为空 is not null; 判断不为空   |
|   逻辑运算符  | and    | 多个条件同时成立    |
|     |or     |    多个条件任一成立 |
|     | not    |  不成立，例：where not(salary>100);   |


- 查询商品名称为洗衣机的商品信息 **select * from product where pname='洗衣机';**
- 查询价格>60的商品信息 ** select * from product where price > 60;**
- 查询名字中含有机的商品信息 **select * from product where pname like '%机%';** 或者**select * from product where pname like "%"'机'"%";**
- 查询id在(3,6,9)范围的商品信息 **select * from product where pid in (3,6,9);**
- 查询商品名称中含有机并且id为6的商品信息 **select * from product where pname like'%机%' and pid=6;**
- 查询id为2或者6的商品信息 **select * from product where pid=2 or pid=6;**
- 按照商品价格(升序/降序 )**select * from product order by price asc;** , **select *from product order by price desc;**
- 查询名称含有器的并且按照价格降序排序 **select * from product where pname like '%器%' order by price desc;**



#### 聚合查询
- 根据pname字段分组,分组后统计商品的个数 **select pname,count(*) from productgroup by pname;**
- 根据pname分组,分组后统计商品的平均价格,并且平均价格>3000 **select
pname,avg(price) as avgprice from product group by pname having avgprice >3000 ;**


#### 分组查询

#### 分页查询

#### 查询总结

#### 设计表所需数据

## 多表查询

### 数据准备

### 交叉连接
### 内连接查询

### 外链接查询

### 子查询

## JDBC

### 开发步骤



