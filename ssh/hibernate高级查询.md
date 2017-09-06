---
title: hibernate高级查询 
tags: ssh,hibernate,Java,框架
grammar_cjkRuby: true
---


# HQL查询
> 通过session调用 session.createQuery(hql) 进行查询

## 单表查询
- 查询集合通过调用 list()
- 查询单个数据通过调用 uniqueResult()
- 条件查询通过 setParameter(索引|别名占位符, 对应的数值)
- 排序查询 from User order by id desc
- 分页查询 setFirstResult(id数值) 和 setMaxResults(总条数)
- 聚合函数和sql相同可以使用 **sum(id),count(*),max(id),min(id),avg(id) 建议使用Number类型进行接收操作**

![enter description here][1]

- 查询对象中某些属性,可以使用两种方法

![enter description here][2]

- 可以使用 select new User(id,name) from User 需要在User中创建相同类型的构造方法,注意如果创建了构造方法,那么必须再创建空参的构造方法

![enter description here][3]

# sql多表查询回顾

## 交叉连接(笛卡尔积)
> 交叉连接为 select * from 表1 CROSS JOIN 表2 可以简化为 select * from 表1,表2

## 内连接

> 1.隐式内连接 select * from 表1,表2 where 表1.字段 =表2.字段
> 2.显示内连接 select * from 表1 inner join 表2 on 表1.字段 = 表2.字段

## 外连接




  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504704212766.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504704253915.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504705706131.jpg