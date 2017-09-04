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




	
	


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504534372962.jpg