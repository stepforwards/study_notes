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

#  映射文件详解

## hibernate-mapping标签

> 一个orm只能有一个hibernate-mapping标签，在这个标签里面配置表结构和对象之间的映射关系

> 注意：package属性用于指定所在的包，那么在配置文件中使用包的全路径的时候就不需要写包名了，**但是只能对应当前包，不能对应子包**

## class标签及其子标签

> class标签及其子标签是用来配置实体与表之间的关系的

**name属性** ：设置为实体的完整包名，如果已经在hibernate-mapping中设置了package属性，可以直接类名
**table属性**：设置为数据库对应的表名，可以省略，省略默认表名和类名相同
**dynamic-insert属性**: 动态插入，默认是flase，如果设置为true就代表，如果字段为空，不参与insert语句
**dynamic-update属性**: 动态更新，默认值是flase，如果设置为true就代表没有改动的属性，将不会生成到update语句中
## id标签
> 配置数据库中主键的映射属性的

- **name属性** ： 设置实体的属性名
- **column属性** ： 设置数据库中字段名，可省略，省略默认和实体类属性名相同
- **length属性** ： 设置列的数据长度
- **unsave-value属性** ： 指定主键为什么值时，当作null来处理（不常用）
- **access属性** ： 如果设置为filed，那么操作属性时，会直接操作对应的字段而不是get/set方法(不推荐使用)

## generator子标签
> 用于设置主键的生成策略，配置class属性可以配置不同的主键生成策略

- **increment**：数据库自己生成主键，先在数据库中查询最大的ID值，将ID加1，作为新的主键
- **identity**： 依赖于数据库主键自增功能
- **sequence**：序列，依赖于数据中的序列功能（Orale）
- **hilo** : hibernate自己实现的序列算法，自己生成主键。
- **native** : 自动根据数据库判断，三选一。identity | sequence | hilo
- **uuid** ： 生成32位的不重复随机字符串当作主键
- **assigned** : 自己指定主键值。表的主键是自然主键时使用。

## peoperty标签

> 配置了除了主键之外的普通属性映射

- **name属性**： 实体中属性的名称
- **column熟悉**：表中字段名称
- **length属性**：数据长度
- **precision属性**：小数点后的精度
- **scale属性**：有效位数
- **insert属性**：该属性是否加入insert语句（一般不使用）
- **update属性**：该属性是否加入update语句（一般不使用）
- **not-null属性** ：指定属性是否使用非空
- **unique属性**： 指定属性的约束是否使用唯一
- **type属性**：表达该属性的类型（一般不建议设置），可以有三种类型
	- java类型**java.lang.String**
	- 数据库类型指定，如**varchar**
	- Hibernate类型指定，如**string**

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
<class name="Video" table="t_video">
    <!--name：实体类名称 column: 数据库字段名，如果设置实体类名称和字段名相同，可省略-->
    <!--主键必须使用id标签-->
    <id name="id">
        <!--主键生成策略，现在先选择native，后面细说-->
        <generator class="native"></generator>
    </id>

    <!--其他字段使用property标签-->
    <property name="name"></property>
    <many-to-one name="speaker" class="Speaker" column="svid"></many-to-one>
</class>
</hibernate-mapping>
```

# 核心配置文件

> 用于配置hibernate中的数据库连接和映射文件所需的基本信息,Hibernate的配置文件有两种格式,一种是propertiest文件,默认是hibernate.properties,一种是xml格式的配置,默认名字是hibernate.cfg.xml,在实际开发过程中大部分时候使用的都是xml文件进行配置.一般将此文件放在src下

## 必选配置
- 数据库驱动 **hibernate.connection.driver_class**
- 数据库**url hibernate.connection.url**
- 数据库连接用户名 **hibernate.connection.username**
- 数据库连接密码 **hibernate.connection.password**
- 数据库方言 **hibernate.dialect**
> 因为hibernate需要生成sql语句,在不同的数据库中sql语法略有差别,指定方言就是设置hibernate在生成sql语句的时候针对哪个库生成的sql
 
- org.hibernate.dialect.MySQLDialect 通用存储引擎(我们一般设置为通用存储引擎,无论对应的哪个存储引擎的数据库都能进行操作)
- org.hibernate.dialect.MySQLInnoDBDialect InnoDB存储引擎
- org.hibernate.dialect.MySQLMyISAMDialect MyISAM存储引擎

## 可选配置

- **hibernate.show_sql** 将生成的sql语句打印到控制台hibernate.format_sql 进行格式化sql语句,如果为false,sql语句就会打印到一行
- **hibernate.hbm2ddl.auto** 自动导出表结构,通过设置此项,hibernate可以通过实体自动创建表结构其有4个设置
	- create 自动建表,每次hibernate运行都会创建新表,之前的内容都会被覆盖(用于测试环境,不常用)
	-	create-drop 自动建表,每次运行都会创建表,运行结束后删除表
	- update 自动建表,如果有就不再自动生成,如果没有创建新表,数据变动不会影响原来的表的内容(推荐使用),比如重新修改了表的映射关系,不会影响原有的数据
	- validate 不自动建表,如果没有表就会抛出异常

> 注意，如果hibernate版本较新,需要自动建表,需要设置方言mysql的方言为 **org.hibernate.dialect.MySQL5Dialect**

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
        <!--
            配置此项，调用getCurrentSession，获取的session与线程进行绑定
            使用getCurrentSession，session会在线程结束后，自动关闭，不能调用session.close
        -->
        <property name="current_session_context_class">thread</property>
        <!--引入ORM配置文件-->
        <mapping resource="top/xiesen/hibernate/model/User.hbm.xml"></mapping>
        <mapping resource="top/xiesen/hibernate/model/Speaker.hbm.xml"></mapping>
        <mapping resource="top/xiesen/hibernate/model/Video.hbm.xml"></mapping>
    </session-factory>
</hibernate-configuration>
```

# API

## Configuration
> 加载核心配置文件类,默认会加载src下的**hibernate.cfg.xml**

## SessionFactory
> 通过conf调用 **buildSessionFactory()** 创建factory,因为核心配置文件中的根标签是 **session-factory** ,所以创
建出来的factory就是我们在配置文件中配置的那个factory可以通过这个对象创建session对象因为factory创建消耗资源比较大,并且一个factory可以创建多个session,所以在实际开发过程中SessionFactory只需要创建一个即可
	

## Session
> 是操作数据库的核心对象,通过SessionFactory 进行创建,可以进行增删改查

## Transaction
> hibernate中处理事务的类

``` java
public class UserTest {

    /**
     * 1. 加载核心配置文件
     * 2. 创建sessionFactory工厂
     * 3. 通过sessionFactory创建session
     * 4. 通过session开启事务
     * 5. 数据库操作
     * 6. 提交事务
     * 7. 关闭资源
     */
    @Test
    public void test01(){
        // 调用configure()方法默认加载src下hibernate.cfg.cml 文件，所以我们的核心配置文件名必须是hibernate.cfg.xml
        Configuration conf = new Configuration().configure();
        // 创建sessionFactory工厂
        SessionFactory sessionFactory = conf.buildSessionFactory();
        // 通过sessionFactory创建session
        Session session = sessionFactory.openSession();

        Transaction tx = session.beginTransaction();
        User user = new User();
        user.setName("李四");
        user.setAge(18);
        user.setMoney(1000);
        session.save(user);
        tx.commit();
        session.close();
        sessionFactory.close();
    }
}

```

# 封装工具类

> SessionFactory具有如下特点:
1.线程安全,可以保证在并发操作的时候,多个线程使用一个SessionFactory对象
2.在创建的时候,会读取所有hibernate.cfg.xml和所有的映射文件,并且会创建数据库中的表,所以比较耗费资源,不能轻易的创建和销毁
因此，在实际开发过程中我们可以使用一个工具类来创建一个SessionFactory对象,通过调用方法来创建操作数据库的session对象

``` java
public class HibernateUtil {
    private static  Configuration cfg;
    private static SessionFactory sessionFactory;
    static {
        cfg = new Configuration();
        cfg.configure();
        sessionFactory = cfg.buildSessionFactory();
    }
    public static Session getSession(){
        return sessionFactory.openSession();
    }

    public static Session openCurrentSession(){
        return sessionFactory.getCurrentSession();
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