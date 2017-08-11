---
title: mybatis02
tags: 框架,MyBatis,Java
grammar_cjkRuby: true
---

## SqlMapConfig.xml配置文件

### 引入外部文件

- properties属性引入外部的properties文件,可以用于引入外部的数据库连接文件,使用的时候使用el表达式进行引入, 路径是src路径 ,其内部有标签 <property> ,如果两个都进行设置,会先加载内部的property再加载外部的properties的resource属性

``` stylus
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" 
"mybatis-3-config.dtd">
<configuration>
<!-- 引入外部文件 -->
	<properties resource="db.properties">
		<!-- 如果配置了property属性，以外部文件为主 -->
		<!-- <property name="jdbc.driverClass" value="com.mysql.jdbc.Driver"/> --> 
	</properties>
	
	<!-- 配置环境 -->
	<environments default="development">
		<environment id="development">
			<!-- 配置事务管理器 -->
			<transactionManager type="JDBC" />
			<!-- 配置数据源 -->
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driverClass}" />
				<property name="url" value="jdbc:mysql:///bigdata14" />
				<property name="username" value="root" />
				<property name="password" value="root" />
			</dataSource>
		</environment>
	</environments>
	
</configuration>
```

## 别名

- typeAliases类型别名属性,在mapper文件中,如果使用参数类型或者返回值类型的时候需要指定类的全名,可以使用别名来进行配置
1. 使用其内部的标签 `<typeAlias type="top.xiesen.mybatis.pojo.User" alias="user"/>` 将对应的类的别名设置为User
2. 使用 `<package name="top.xiesen.mybatis.pojo"/>`会自动扫描当前包和其子包将别名配置为 类名首字母大写(User) 和 类名小写(user) ,推荐使用这种

- mybatis内部配置的别名,在mapper中写哪个都可以

![mybatis内置别名][1]


### mapper映射器--加载mapper文件

> mappers映射器属性,内部设置的标签是 `<mapper>` 用来设置mapper映射文件的路径

- `<mapper resource="sqlmap/User.xml"/>` resource指定的classpath路径不是相对路径
- `<mapper class="top.xiesen.mybatis.mapper.UserMapper"/>` **通过接口文件找到对应的xml文件**,此种状态下 <font color="red">**必须保证xml文件和接口名称相同,且放在同一个包下**</font>
- `<package name="top.xiesen.mybatis.mapper"/>` 会扫描指定包下及其子包中的所有的mapper文件,此种状态下<font color="red">**必须保证xml文件和接口名称相同,且放在同一个包下**</font>


## 输入映射(parameterType)

> POJO（Plain Ordinary Java Object）简单的Java对象，实际就是普通JavaBeans

- 简单类型
	- 整型
	- String类型
- 复杂类型
	- POJO
	- POJO的包装类(内部包含一个普通的类,可以进行多条件查询)
	- 应用范围:查询条件可能是综合的查询条件，不仅包括用户查询条件还包括其它的查询条件（比如查询用户信息的时候，将用户购买商品信息也作为查询条件），这时可以使用包装对象传递输入参数.

## 输出映射(resultType)

- 简单类型
	- 整型
	- 字符串

``` stylus
<select id="findUserCount" resultType="Integer">
	select count(*) from user
</select>
	
<select id="findUserName" resultType="String" parameterType="Integer">
	select username from user where id = #{v}
</select>
```


- 复杂类型
	- POJO
	- POJO列表

``` stylus
<select id="findAllUser" resultType="user">
		select * from t_user
</select>

<select id="findAccountById" resultMap="selector" parameterType="account">
		select * from t_account where id = #{id}
</select>
```

## resultMap

> 如果sql查询字段名和pojo的属性名不一致，可以通过resultMap将字段名和属性名作一个对应关系 ，resultMap实质上还需要将查询结果映射到pojo对象中。

- id映射 `<id column="id" property="id"/>`
- 其他字段映射 `<result column="user_id" property="userId"/>`
- 一对一映射 `<association property=""></association>`
- 一对多映射 `<collection property=""></collection>`

``` stylus
<!-- 一对多映射 -->
<resultMap type="user" id="selectorUser">
	<id column="id" property="id" />
	<result column="username" property="username" />
	<result column="password" property="password" />

	<collection property="orders" ofType="order">
		<id column="oid" property="oid" />
		<result column="userid" property="userid" />
		<result column="number" property="number" />
		<result column="brand" property="brand" />
	</collection>
	
</resultMap>

<!-- 一对一映射 -->
<resultMap type="order" id="selectOrder">
	<id column="id" property="id"/>
	<result column="userid" property="userid"/>
	<result column="number" property="number"/>
	<result column="brand" property="brand"/>
	<association property="user" javaType="user">
		<id column="id" property="id"/>
		<result column="username" property="username"/>
		<result column="password" property="password"/>
	</association>
</resultMap>

```
## 动态sql

1. if标签
> 可用于字符串的非空判断

``` stylus
<select id="findUserByUsernameAndPasswordInUserVo" resultType="user"
	parameterType="userVo">
	select * from t_user
	<where>
		<if test="user.username != null and user.username != '' ">
			and username like '%' #{user.username} '%'
		</if>
		<if test="user.password != null and user.password != '' ">
			and password = #{user.password}
		</if>
	</where>
</select>
```

2. where标签
> 可以去掉第一个前and标签,代码示例参考if标签示例

3. sql片段
> 可以把多次出现的sql封装成一个片段,在使用的时候进行引用

``` stylus
<!-- sql片段 -->
<sql id="selector">
	select * from t_user
</sql>

<select id="findUserById" resultType="user">
	<!-- 引用sql片段 -->
	<include refid="selector"/>
	where id = #{id} 
</select>
```
4. foreach标签

> 向sql传递数组或List，mybatis使用foreach解析，如下：根据多个id查询用户信息查询sql：`SELECT * FROM user WHERE id IN (1,10,24)`
- collection= "List集合 
``` stylus
<!-- 按id数组进行查询，collection= "List集合 目的是测试动态sql中的foreach标签 -->
<select id="findUserByIds" resultType="user" parameterType="userVo">
	select * from t_user
	<where>
		id in
		<foreach collection="idList" item="item" open="(" separator="," close=")">
			#{item}
		</foreach>
	</where>
</select>
```
测试代码

``` stylus
	@Test
	public void testFindUserByIds(){
		SqlSession session = factory.openSession();
		UserMapper mapper = session.getMapper(UserMapper.class);
		UserVo userVo = new UserVo();
		
		List<Integer> i = new ArrayList<Integer>();
		i.add(1);
		i.add(2);
		i.add(3);
		userVo.setIdList(i);
		List<User> list = mapper.findUserByIds(userVo);
		System.out.println(list);
	}
```
- collection= "数组" 

``` stylus
	<select id="findUserByIds" resultType="user" parameterType="userVo">
		select * from t_user
		<where>
			id in
			<foreach collection="ids" item="item" open="(" separator=","
				close=")">
				#{item}
			</foreach>
		</where>
	</select>
```
测试代码

``` stylus
@Test
public void testFindUserByIds(){
	SqlSession session = factory.openSession();
	UserMapper mapper = session.getMapper(UserMapper.class);
	UserVo userVo = new UserVo();
	Integer[] ids = {1,2,3};
	userVo.setIds(ids);

	List<User> list = mapper.findUserByIds(userVo);
	System.out.println(list);
}
```

## 关联查询
关联查询，以用户和订单的案例进分析。一个用户可以有多个订单，用户-->订单 ==》一对多；一个订单只属于一个用户，订单--> 用户 ==》 一对一；

### 一对一查询

1. 在订单的pojo中添加一个成员变量用户，生成其setter和getter方法
2. 编写接口`List<Order> findOrderWithUser();` 
3. 在mapper文件中编写如下代码：

``` stylus
<!-- mybatis是半自动的dao层框架，涉及到多表查询需要自定义返回值类型 -->
<resultMap type="order" id="selectOrder">
	<id column="id" property="id"/>
	<result column="userid" property="userid"/>
	<result column="number" property="number"/>
	<result column="brand" property="brand"/>
	<association property="user" javaType="user">
		<id column="id" property="id"/>
		<result column="username" property="username"/>
		<result column="password" property="password"/>
	</association>
</resultMap>

<select id="findOrderWithUser" resultMap="selectOrder">
	select * from t_order o left join t_user u on o.userid = u.id  
</select>
```
4.编写测试类

``` stylus
@Test
public void testFindOrderWithUser(){
	SqlSession session = factory.openSession();
	OrderMapper mapper = session.getMapper(OrderMapper.class);

	System.out.println(mapper.findOrderWithUser());
}
```

### 一对多查询
1.在用户的pojo中添加order的成员变量，一个用户对应多个订单，所以应该定义为`private List<Order> orders;`并生成其setter和getter方法

2.编写对应的接口 `List<User> findUserWithOrder();`

3.在mapper文件中编写如下代码：

``` stylus
	<resultMap type="user" id="selectorUser">
		<id column="id" property="id" />
		<result column="username" property="username" />
		<result column="password" property="password" />

		<collection property="orders" ofType="order">
			<id column="oid" property="oid" />
			<result column="userid" property="userid" />
			<result column="number" property="number" />
			<result column="brand" property="brand" />
		</collection>
	</resultMap>
	<select id="findUserWithOrder" resultMap="selectorUser">
		select u.*,o.* from
		t_user u left join t_order o on o.userid = u.id
	</select>
```
4.编写测试类

``` stylus
	@Test
	public void testFindUserWithOrder(){
		SqlSession session = factory.openSession();
		UserMapper mapper = session.getMapper(UserMapper.class);
		System.out.println(mapper.findUserWithOrder());
	}
```





  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502455876896.jpg