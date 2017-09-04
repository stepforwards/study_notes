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

![外部约束文件位置][8]

idea[setting/language&Frameworks/]

![enter description here][9]

![enter description here][10]

## 创建model对象User(id,name,age)

``` java
public class User {
    private Integer id;
    private String name;
    private Integer age;	
	private Integer money;
	setter/getter...
}
```

## 创建User.hbm.xml的orm映射文件
> 创建映射文件命名为User.hbm.xml，命名可以随意，但是规范是：*.hbm.xml
``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<!--
    配置package以后能够在当前文件下省去路径，但是不包含子包
-->
<hibernate-mapping package="top.xiesen.hibernate.model">
<!--
    name:表示model实体类名称
    table：表名，如果表名和实体类名称相同，可省略table属性
-->
<class name="User" table="t_user">
    <!--name：实体类名称 column: 数据库字段名，如果设置实体类名称和字段名相同，可省略-->
    <!--主键必须使用id标签-->
    <id name="id" column="id">
        <!--主键生成策略，现在先选择native，后面细说-->
        <generator class="native"></generator>
    </id>
    <!--其他字段使用property标签-->
    <property name="name"></property>
    <property name="age" ></property>
    <property name="money"></property>
</class>

</hibernate-mapping>
```
## 在src下创建核心配置文件

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <!--配置数据库的相关信息-->
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="connection.url">jdbc:mysql://localhost:3306/hibernate</property>
        <property name="connection.username">root</property>
        <property name="connection.password">root</property>

        <!--配置数据库的方言-->
        <property name="dialect">org.hibernate.dialect.MySQL5Dialect</property>

        <!--配置hibernate的相关信息-->
        <!--是否显示底层的sql语句-->
        <property name="show_sql">true</property>
        <!--是否格式化显示sql语句-->
        <property name="format_sql">true</property>

        <!--hibernate会自动给我们创建表，默认是不会创建的，需要配置hbm2ddl.auto
            值 update：如果数据库中不存在表，创建；存在，更新
        -->
        <!--
            #hibernate.hbm2ddl.auto create-drop //启动的时候创建，close的时候删除
            #hibernate.hbm2ddl.auto create //每次启动创建新表，旧的数据会被覆盖掉
            #hibernate.hbm2ddl.auto update //启动的时候如果没有就创建，存在就更新，不会对原来数据有影响(测试和开发)
            #hibernate.hbm2ddl.auto validate // 不会创建表，如果执行的时候没有表，抛异常
        -->
        <property name="hbm2ddl.auto">update</property>
        <!--引入ORM配置文件-->
        <mapping resource="top/xiesen/hibernate/model/User.hbm.xml"></mapping>
    </session-factory>
</hibernate-configuration>
```

## 编写测试用例
> 创建测试步骤：
> 1. 加载核心配置文件
> 2. 创建SessionFactory
> 3. 通过SessionFactory对象创建Session
> 4. 通过Session开启事务
> 5. 编写crud操作
> 6.提交事务
> 7.释放资源

``` java

@Test
    public void test01(){
        // 调用configure()方法默认加载src下hibernate.cfg.cml 文件，所以我们的核心配置文件名必须是hibernate.cfg.xml
        Configuration conf = new Configuration().configure();
        // 当代码执行到BuilderSessionFactory时，读取文件，创建所有的表结构就会创建出来，比较耗资源
        SessionFactory sessionFactory = conf.buildSessionFactory();
        // session就是操作数据库的对象
        Session session = sessionFactory.openSession();
        // 通过session开启事务
        Transaction tx = session.beginTransaction();
        // 创建对象
        User user = new User();
        user.setName("李四");
        user.setAge(18);
        // 保存数据
        session.save(user);
        tx.commit();
        session.close();
        sessionFactory.close();
    }
}
```

	


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504534372962.jpg
  [2]: http://hibernate.org/orm/downloads/
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504535102156.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504535416113.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504535471051.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504535509701.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504535587495.jpg
  [8]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504536776154.jpg
  [9]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504537134562.jpg
  [10]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504537365213.jpg