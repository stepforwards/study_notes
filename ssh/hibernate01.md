---
title: hibernate01
tags: 框架,hibernate,Java,ssh
grammar_cjkRuby: true
---
# 什么是hibernate
- Hibernate是轻量级JavaEE应用的持久层解决方案，**是一个关系数据库ORM框架**

- Hibernate具体实现的操作：
Hibernate的是开源轻量级的框架，对jdbc的代码进行封装，程序员不需要写底层的sql语句的代码，就可以实现对数据库的操作。实现对数据库的crud操作。
- jdbc 底层代码实现：
	1. 加载驱动
	2. 创建连接
	3. 对sql进行预编译操作
	4. 设置参数，执行sql语句
	5. 释放资源

# 什么是ORM
- ORM：Object Relational Mapping，对象关系映射
> ORM 就是通过将Java对象映射到数据库表，通过操作Java对象，就可以完成对数据表的操作
>  在hibernate中，让实体类和数据库进行映射对应的关系（配置文件配置），
>  操作实体类就相当于操作数据库表中的数据

![orm图形分析][1]

# Hibernate入门案例

## 导包
我们使用的是 hibernate-release-5.0.7.Final，下载地址：[http://hibernate.org/orm/downloads/][2]，自行下载
下载好hibernate文件如图所示：

![hibernate文档介绍][3]
1.引入[lib/required]下面的所有包

![hibernate核心包][4]

2.hibernate本身没有集成日志包，需要添加日志包

![log4j日志包][5]

3.junit测试包

![junit日志包][6]
4. 导入数据库驱动包，总共15个jar包

![hibernate开发jar包][7]


## 引入外部约束(此过程不是必须的)



## 创建model对象User(id,name,age)
## 创建User.hbm.xml的orm映射文件
## 在src下创建核心配置文件
## 编写测试用例

	
	


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504534372962.jpg
  [2]: http://hibernate.org/orm/downloads/
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504535102156.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504535416113.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504535471051.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504535509701.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504535587495.jpg