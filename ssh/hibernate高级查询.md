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

> 1.左连接 select * from 表1 left join 表2 on 表1.字段= 表2.字段
2.又连接 select * from 表1 right join 表2 on 表1.字段 = 表2.字段

# HQL多表查询

## 内连接

>  1.内连接: from Speaker s inner join s.videoSet ,其结果是list集合,集合中放的是 Object[] ,第一项是 speaker 对象,第二项是 video 对象

![enter description here][4]

![enter description here][5]

> 2.迫切内连接: select distinct s from Speaker s inner join fetch s.videoSet ,其结果是list集合,集合中是 Speaker 对象, hibernate 会自动帮助我们进行封装处理

![enter description here][6]

![enter description here][7]

## 外连接

> 1.左外连接 from Speaker s left join s.videoSet 返回值类型是list,其中放的是 Object[] ,第一项是 speaker 对象,第二项是 video 对象

![enter description here][8]

![enter description here][9]

>2.迫切左外连接 select distinct s from Speaker s left join fetch s.videoSet 其结果是list集合,集合中是 Speaker 对象, hibernate 会自动帮助我们进行封装处理

![enter description here][10]

![enter description here][11]

> 3.右外连接(和左外连接相同,left换成right)
> 4.迫切右外连接(和迫切左外连接相同,left换成right)

**注意需要做去重操作,添加 select distinct s**

# QBC查询

> 通过session调用 session.createCriteria(类名.class)

|   限定方法  |  说明   |
| :---: | :---: |
|  Restrictions.eq   |  equal,等于   |
|  Restrictions.allEq   |  参数为Map对象,使用key/value进行多个等于的比对,相当于多个Restrictions.eq的效果   |
|  Restrictions.gt   |   great-than > 大于  |
|  Restrictions.ge   | great-equal >= 大于等于    |
|  Restrictions.lt   | less-than, < 小于    |
|   Restrictions.le  | less-equal <= 小于等于    |
|  Restrictions.between   |   对应SQL的between子句 |
|  Restrictions.like   | 对应SQL的LIKE子句    |
|  Restrictions.in   |   对应SQL的in子句  |
|Restrictions.and     |   and 关系  |
| Restrictions.or	    |    or 关系 |
|   Restrictions.isNull  |    判断属性是否为空,为空则返回true |
|   Restrictions.isNotNull  |   与isNull相反  |
|Restrictions.sqlRestriction     | SQL限定的查询    |
|   Order.asc  |  根据传入的字段进行升序排序   |
|  Order.desc   |   根据传入的字段进行降序排序  |
|  MatchMode.EXACT   |   字符串精确匹配.相当于”like ‘value’”  |
|   MatchMode.ANYWHERE  |   字符串在中间匹配.相当于”like ‘%value%’”  |
| MatchMode.START    |   字符串在最前面的位置.相当于”like ‘value%’”  |
| MatchMode.END    |  MatchMode.END   |

- 查询集合通过调用 **list()**
- 查询单个数据通过调用 **uniqueResult()**
- 条件查询通过调用 **add(Restrictions.eq())** 等操作
- 排序操作 **addOrder(Order.asc("id"))**
- 统计操作 **setProjection(Projections.rowCount());**

![enter description here][12]

# 离线查询

> 在通常的web开发过程中,我们需要将web层提交的数据,传递给service,再由service传递给dao,最终在dao层通过factory创建session,由session创建查询,如果在dao层想要对所有的查询方法进行封装的话,因为不同的业务传递的参数会有所不同,所以很难封装,hibernate为我们提供了离线查询,也就是说可以在脱离session的情况下创建出来查询条件,由web层给service层,再由service层给dao,在dao层只需写一个方法就能完成就能完成很多工程的查询

创建离线 ==Criteria DetachedCriteria dc = DetachedCriteria.forClass(User.class);==
通过我们之前的QBC添加条件, ==dc.add(Restrictions.idEq(1));==
当传递到dao层的时候可以通过dc调用方法,去绑定session, ==Criteria criteria = dc.getExecutableCriteria(session);==


![enter description here][13]

# 查询优化
## 类级别查询优化
### 懒加载

> 在使用OID查询的时候我们之前使用的是 ==User user = session.get(User.class, 1);== 会发现当代码走到这一行的时候,sql语句就会打印,当我们使用 User user =session.load(User.class, 1); 方法的时候,只有当使用user的时候才会打印sql语句懒加载的策略默认是true,我们可以在通过在class元素上配置lazy属性进行控制,lazy值如果是true,表示加载的时候不查询,使用的时候才查询

![enter description here][14]

**注意：** ==所有延迟加载的策略只适用与load方法,不适用get方法==

- 原理是在没有使用的时候创建的不是User对象,仅仅是代理对象,这个代理对象会关联session,在使用的时候通过session查询数据库

![enter description here][15]

- 确保在调用属性加载数据的时候,session还是打开的

## 关联级别查询优化

> 主要是在关联级别的查询的时候操作延时加载和抓取策略

### 集合级别的关联

> 对于speaker中的set标签可以设置两个属性lazy和fetch

- lazy属性可以设置3个值
	- true (默认值)延时加载|懒加载
	- false 立即加载
	- extra 极其懒惰的加载	

- fetch属性可以设置3个值
	- select (默认值)单表select语句查询关联对象
	- join 发送一条迫切左连接查询关联对象
	- subselect 发送一条子查询,查询其关联对象

- 效果 fetch="select"
- lazy="true" 在使用的集合的时候,发送单表查询语句
- lazy="false" 在获取集合的时候,发送单表查询语句
- lazy="extra" 会发现在查看集合的size时候只会查询大小,在使用其他内容的时候再去查询

![enter description here][16]

![enter description here][17]

> 效果 fetch="join" ,lazy属性无论是什么值都会失效,因为在获取speaker的时候会进行多表查询将所有数据都查出来

![enter description here][18]

![enter description here][19]

- 效果 fetch="subselect" ,如果查询的是单条数据那么和select没有区别,所以可以查询所有的speaker,通过遍历查看效果

![enter description here][20]

- lazy="true" 在查询到size的时候会把所有数据通过子查询都查出来
- lazy="false" 在查询到list的时候会把所有数据通过子查询都查出来
- lazy="extra" 在查询到 sp.getVideoSet() 的时候把通过子查询把数据都查找出来

### 对象级别的关联

> 在video的多对标签可以设置fetch和lazy

- lazy属性可以设置两种
	- false 立即加载
	- proxy 有video的类加载级别决定
- fetch属性可以设置两种
	- select 单表查询
	- join 迫切左连接

#### 扩大session的范围

> 如果不去扩到session的范围,因为web层会使用dao层的数据,如果此时数据是懒加载,就会出现no-session问题



  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504704212766.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504704253915.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504705706131.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504705815667.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504705833258.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504705873937.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504705909391.jpg
  [8]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504705948314.jpg
  [9]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504705968193.jpg
  [10]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504705998212.jpg
  [11]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504706015799.jpg
  [12]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504706856881.jpg
  [13]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504706976001.jpg
  [14]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504707086263.jpg
  [15]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504707154855.jpg
  [16]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504707338881.jpg
  [17]: http://markdown.xiaoshujiang.com/img/spinner.gif "[[[1504707362131]]]"
  [18]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504707428618.jpg
  [19]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504707485886.jpg
  [20]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504707519367.jpg