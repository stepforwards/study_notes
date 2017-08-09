---
title: Spring03
tags: Spring,框架,Java
grammar_cjkRuby: true
---

## spring整合JDBC

> spring中提供了一个可以操作数据库的对象,这个对象封装了jdbc技术,这个对象叫做JDBCTemplate(JDBC模版对象)

### 演示JDBCTemplate如何进行数据库的操作

- 导包4+2+1(aop)+1(test)+JDBC驱动+c3p0连接池+spring-jdbc(spring包中)+spring-tx(spring包中)
- 书写Dao层接口
- 书写Dao的实现类
- 编写配置文件
- 书写测试类

####  书写Dao层接口

``` stylus
public interface UserDao {

	void insertUser(User u);
	void deleteUser(User u);
	void updateUser(User u);
	List<User> selectAllUser();
}

```
#### 书写Dao的实现类

``` stylus
public class UserDaoImpl implements UserDao{

	@Autowired
	private JdbcTemplate jt;

	public void setJt(JdbcTemplate jt) {
		this.jt = jt;
	}
	// 插入操作
	@Override
	public void insertUser(User u) {
		String sql = "insert into t_user values (null,?,?)";
		jt.update(sql, u.getUsername(),u.getPassword());
	}
	// 查询操作
	@Override
	public List<User> selectAllUser() {
		String sql = "select * from t_user";
		List<User> users = jt.query(sql, new RowMapper<User>(){

			@Override
			public User mapRow(ResultSet rs, int index) throws SQLException {
				User user = new User();
				user.setId(rs.getInt("id"));
				user.setPassword(rs.getString("password"));
				user.setUsername(rs.getString("username"));
				return user;
			}
			
		});

		return users;
	}

	// 删除操作
	@Override
	public void deleteUser(User u) {
		String sql = "delete from t_user where id = ?";
		jt.update(sql, u.getId());
	}
	// 更新操作
	@Override
	public void updateUser(User u) {
		String sql = "update t_user set username = ? where id = ?";
		jt.update(sql, u.getUsername(),u.getId());
	}
}

```


#### 书写配置文件

``` stylus
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd">
<context:property-placeholder location="classpath:db.properties"/>
<bean name="userService" class="top.xiesen.spring.service.impl.UserServiceImpl"></bean>

<bean name="myAdvice" class="top.xiesen.spring.aop_annotation.MyAdvice"></bean>

<!-- 帮助我们自动织入,但是不自动帮我们添加到Spring容器中 -->
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>

<bean name="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	<property name="driverClass" value="${jdbc.driverClass}"></property>
	<property name="jdbcUrl" value="${jdbc.jdbcUrl}"></property>
	<property name="user" value="${jdbc.user}"></property>
	<property name="password" value="${jdbc.password}"></property>
</bean>

<bean name="jt" class="org.springframework.jdbc.core.JdbcTemplate">
	<property name="dataSource" ref="dataSource"></property>
</bean>

<bean name="userDao" class="top.xiesen.spring.dao.impl.UserDaoImpl">
	<property name="jt" ref="jt"></property>
</bean>
</beans>
```

#### 编写测试类

``` stylus
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class TestJDBC {

	@Autowired
	UserDao userDao;

	@Test
	public void testinsert(){
		User u = new User();
		u.setUsername("xxxxx2");
		u.setPassword("xxxxxx2");
		userDao.insertUser(u);
	}
	
	@Test
	public void testselect(){
		List<User> list = userDao.selectAllUser();
		for (User user : list) {
			System.out.println(user);
		}
	}
	
	@Test
	public void testUpdate(){
		User u = new User();
		u.setId(15);
		u.setUsername("张三1");
		userDao.updateUser(u);
	}
	@Test
	public void testDelete(){
		User u = new User();
		u.setId(15);
		userDao.deleteUser(u);
	}
}

```

> **注意**:JdbcDaoSupport类内部封装了一个JDBCTemplate模版,在需要的时候调用this.getJdbcTemplate()即可,所以我们可以使我们的daoImpl继承JDBCDaoSupport,在需要的时候调用this.getJDBCTemplate

## properyies配置jdbc连接信息

1. 在src下创建db.properties文件
2. 在Spring文件中引入db.properties文件

``` stylus
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd">
		<!--引入db.properties文件-->
<context:property-placeholder location="classpath:db.properties"/>
<bean name="userService" class="top.xiesen.spring.service.impl.UserServiceImpl"></bean>

<bean name="myAdvice" class="top.xiesen.spring.aop_annotation.MyAdvice"></bean>

<!-- 帮助我们自动织入,但是不自动帮我们添加到Spring容器中 -->
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
<!--通过${}来去db.properties文件中的值
<bean name="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	<property name="driverClass" value="${jdbc.driverClass}"></property>
	<property name="jdbcUrl" value="${jdbc.jdbcUrl}"></property>
	<property name="user" value="${jdbc.user}"></property>
	<property name="password" value="${jdbc.password}"></property>
</bean>

<bean name="jt" class="org.springframework.jdbc.core.JdbcTemplate">
	<property name="dataSource" ref="dataSource"></property>
</bean>

<bean name="userDao" class="top.xiesen.spring.dao.impl.UserDaoImpl">
	<property name="jt" ref="jt"></property>
</bean>
</beans>
```

## 事务

### 事务的特性

- **原子性（Atomicity）** <font color="red">原子性是指事务是一个不可分割的工作单位，事务中的操作 要么都发生，要么都不发生。</font>
- **一致性（Consistency）** <font color="red">一致性是指事务必须使数据库从一个一致性状态变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态.拿转账来说，假设用户A和用户B两者的钱加起来一共是5000，那么不管A和B之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是5000，这就是事务的一致性</font>
- **隔离性（Isolation）** <font color="red">隔离性是当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离.即要达到这么一种效果：对于任意两个并发的事务T1和T2，在事务T1看来，T2要么在T1开始之前就已经结束，要么在T1结束之后才开始，这样每个事务都感觉不到有其他事务在并发地执行</font>
- **持久性（Durability）** <font color="red">持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。</font>

### 事务并发引起的问题

- **脏读 ** <font color="red">B事务读取到了A事务尚未提交的数据 ------ 要求B事务要读取A事 务提交的数据</font>
- **不可重复读** <font color="red">不可重复读是指在对于数据库中的某个数据，一个事务范围内多次查询却返回了不同的数据值，这是由于在查询间隔，被另一个事务修改并提交了</font>
- **幻读(虚读)** <font color="red"> 幻读是事务非独立执行时发生的一种现象。例如事务T1对一个表中所有的行的某个数据项做了从“1”修改为“2”的操作，这时事务T2又对这个表中插入了一行数据项，而这个数据项的数值还是为“1”并且提交给数据库。而操作事务T1的用户如果再查看刚刚修改的数据，会发现还有一行没有修改，其实这行是从事务T2中添加的，就好像产生幻觉一样，这就是发生了幻读</font>


### 事务隔离级别

- read uncommitted : 读取尚未提交的数据 ：哪个问题都不能解决
- read committed：读取已经提交的数据 ：可以解决脏读 — oracle默认的
- repeatable read：重读读取：可以解决脏读 和 不可重复读 —mysql默认的
- serializable：串行化：可以解决 脏读 不可重复读 和虚读—相当于锁表
- 查看mysql数据库默认的隔离级别： `select @@tx_isolation`
- 设置mysql的隔离级别： set session transactionisolation level 设置事务隔离级别
- mysql的事务控制：
	- 开启事务：start transaction;
	- 提交：commit；
	- 回滚：rollback；
- JDBC事务控制：
	- 开启事务：conn.setAutoCommit(false);
	- 提交：conn.commit()；
	- 回滚：conn.rollback():
	- 隔离级别的性能： `read uncommitted>read committed>repeatable read>serialazable`
	- 安全性: read uncommitted < readcommitted < repeatable read < serialazable

## spring中的事务

> 不同平台操作事务的方式不同,如jdbc是使用connection,Hibernate操作事务是使用transition,mybatis操作事务是使用session操作,但是不管是哪种框架,都要实现spring的PlatformTransactionManager接口

## spring管理事务的属性
- 隔离级别
	- 读未提交
	- 读已提交
	- 可重复读
	- 串行化
- 是否只读,表示操作事务的时候,当前事务是否是只读,比如查询操作就应该只读,其他方法牵扯到修改就是非只读
	- true表示只读(不允许做修改数据库的操作)
	- false表示非只读
- 事务的传播行为,在业务方法平行调用的时候,事务应该如何处理的问题
	- 保证同一事务中
		- PROPAGATION_REQUIRED 支持当前事务，如果不存在 就新建一个(默认)
		- PROPAGATION_SUPPORTS 支持当前事务，如果不存在，就不使用事务
		- PROPAGATION_MANDATORY 支持当前事务，如果不存在，抛出异常
	- 保证不在同一事务中
		- PROPAGATION_REQUIRES_NEW 如果有事务存在，挂起当前事务，创建一个新的事务
		- PROPAGATION_NOT_SUPPORTED 以非事务方式运行，如果有事务存在，挂起当前事务
		- PROPAGATION_NEVER 以非事务方式运行，如果有事务存在，抛出异常
		- PROPAGATION_NESTED 如果当前事务存在，则嵌套事务执行

### 添加事务管理
- 导包4+2+1(aop)+1(aspect)+1(aop联盟)+1(weaving)
- 配置核心事务管理器












