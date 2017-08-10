---
title: mybatis01
tags: MyBatis,框架,Java
grammar_cjkRuby: true
---
[TOC]

## MyBatis

> MyBatis 本是apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。2013年11月迁移到Github。MyBatis是一个优秀的持久层框架，它对jdbc的操作数据库的过程进行封装，使开发者只需要关注 SQL 本身，而不需要花费精力去处理例如注册驱动、创建connection、创建statement、手动设置参数、结果集检索等jdbc繁杂的过程代码。

- 对比JDBC开发,传统的JDBC存在如下问题
	- 数据库连接创建、释放频繁造成系统资源浪费，从而影响系统性能。如果使用数据库连接池可解决此问题。
	- Sql语句在代码中硬编码，造成代码不易维护，实际应用中sql变化的可能较大，sql变动需要改变java代码。
	- 使用preparedStatement向占有位符号传参数存在硬编码，因为sql语句的where条件不一定，可能多也可能少，修改sql还要修改代码，系统不易维护。
	- 对结果集解析存在硬编码（查询列名），sql变化导致解析代码变化，系统不易维护，如果能将数据库记录封装成pojo对象解析比较方便

## Mybatis的HelloWorld

- [下载MyBatis的jar包][1]

- 添加jar包,mybatis核心包+mybatis依赖包+数据库驱动包
- 添加xml文件约束dtd,在核心包 ` /org/apache/ibatis/builder/xml/mybatis-3-config.dtd` 和`/org/apache/ibatis/builder/xml/mybatis-3-mapper.dtd` 对应的两个Key值分别是 `-//mybatis.org//DTD Config 3.0//EN` 和 `-//mybatis.org//DTD Mapper3.0//EN ` RootElements分别是`configuration` 和 `mapper`

![enter description here][2]

- 核心配置文件

``` stylus
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" 
"mybatis-3-config.dtd">
<configuration>
	<!-- 配置环境 -->
	<environments default="development">
		<environment id="development">
			<!-- 配置事务管理器 -->
			<transactionManager type="JDBC" />
			<!-- 配置数据源 -->
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url" value="jdbc:mysql:///bigdata14" />
				<property name="username" value="root" />
				<property name="password" value="root" />
			</dataSource>
		</environment>
	</environments>
	<!-- 加载映射文件 -->
	<mappers>
		<mapper resource="sqlmap/User.xml" />
		<mapper resource="top/xiesen/mybatis/dao/mapper/User.xml" />
		<mapper resource="top/xiesen/mybatis/mapper/User.xml" />
	</mappers>
	
</configuration>
```

- 配置日志文件,创建log4j.properties,将以下的内容赋值进去

``` stylus
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n

```

- 配置mapper文件

``` stylus
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
"mybatis-3-mapper.dtd">
<!--
	必须配置namespace属性，使用namespace可以区分不同的mapper中的sql名称
 -->
<mapper namespace="test">
<!-- 按照id进行查询 -->
<select id="findUserById" parameterType="Integer" resultType="top.xiesen.mybatis.pojo.User">
	select * from t_user where id = #{id}
</select>

<!-- 更新数据 -->
<update id="updateUser" parameterType="top.xiesen.mybatis.pojo.User">
	update t_user set username=#{username},password=#{password} where id=#{id}
</update>

<!-- 删除数据 -->
<delete id="deleteUserById" parameterType="Integer">
	delete from t_user where id = #{id}
</delete>

<!-- 插入数据 -->
<insert id="insertUser" parameterType="top.xiesen.mybatis.pojo.User">
	insert into  t_user values(null,#{username},#{password}); 
</insert>

<!-- 模糊查询 -->
<select id="findUserByName" parameterType="String" resultType="top.xiesen.mybatis.pojo.User">
	select * from t_user where username like '%${value}%'
</select>

<!-- 
	#{v} 表示sql中的 ？ 其中的内容可以随便写，相当于在执行的时候自动添加了 '' (推荐使用这种方式)
	${value} 表示字符串的拼接，不会加任何符号，需要我们自己拼接
 -->
<select id="findUserByName" parameterType="String" resultType="top.xiesen.mybatis.pojo.User">
	select * from t_user where username like '%' #{v} '%'
</select>
</mapper>
```

- 编写测试类

1. 创建sqlSessionFactory
2. 通过文件创建sqlSession
3. 通过openSession建立连接
4. 通过sqlSession的增删改查

``` stylus
public class TestMyBatis {

	/**
	 * 创建一个操作数据库的对象，读取配置文件
	 * 0.创建sqlSessionFactoryBuilder对象
	 * 1.创建sqlSessionFactory对象
	 * 2.通过工厂对象创建sqlSession
	 * 3.使用sqlSession操作数据库 
	 * @throws Exception 
	 */
	@Test
	public void test01() throws Exception{
		// 创建sqlSessionFactoryBuilder对象
		SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
		// 构建工厂对象读取配置文件,生成工厂
		SqlSessionFactory sessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("sqlMappingConfig.xml"));
		// 通过工厂创建sqlSession
		SqlSession sqlSession = sessionFactory.openSession();
		// 查询一个
		User user = sqlSession.selectOne("test.findUserById", 1);
		System.out.println(user);
		// 按照id进行修该
		User u = new User();
		u.setId(1);
		u.setUsername("东方不败");
		u.setPassword("123");
		sqlSession.update("test.updateUser", u);
		sqlSession.delete("test.deleteUserById",14);
		
		// 插入操作
		User u1 = new User();
		u1.setUsername("张三丰");
		u1.setPassword("zsf");
		sqlSession.insert("test.insertUser", u1);
		
		// 模糊查询
		List<User> list = sqlSession.selectList("test.findUserByName", "丰");
		System.out.println(list);
		
		// 提交事务
		sqlSession.commit();
		// 关闭sqlSession
		sqlSession.close();
	}
}

```

## HelloWorld讲解

- namespace,必须要进行设置,如果不设置,文件无法加载可以表示当前mapper中的增删改查数据哪个命名空间,用以区别其他的sql中的内容
- `#{任意写}` 和 `${value}` 的区别, `#{任意写}` 用来表示sql中的占位符 ? 使用 `${value}` 用来表示字符串的拼接
- 核心配置文件中的 `<mappers>` 标签用来在核心配置文件中引入对应的对应的mapper文件
- mapper配置文件是用来书写sql语句的,一个配置文件中只能有一个mapper标签,mapper标签内部有增删改查标签,用来书写增删改查操作
- 增删改查标签中的parameterType属性为设置执行sql语句的时候传过来的值的类型
- 增删改查标签中的resultType属性为设置执行sql语句的时候返回值的类型
- 如果返回值是list,返回值的类型同样还是写成对应集合中存取对象的类型

## 将mybatis应用到dao层
	



  [1]: https://github.com/mybatis/mybatis%C2%AD3/releases
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502364549838.jpg