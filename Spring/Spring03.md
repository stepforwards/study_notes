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
- 填写增删改查方法

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


```


#### 填写增删改查方法


