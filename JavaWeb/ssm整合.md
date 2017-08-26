---
title: ssm整合
tags: idea,ssm,Java
grammar_cjkRuby: true
---

1.创建maven项目，导包


2.编写mybatis配置文件

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "mybatis-3-config.dtd">

<configuration>
	<typeAliases>
		<package name="top.xiesen.video.model"/>
	</typeAliases>
</configuration>
```

3.编写applicationContext.xml文件

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://www.springframework.org/schema/beans" xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.2.xsd 
	http://www.springframework.org/schema/aop 
	http://www.springframework.org/schema/aop/spring-aop-4.2.xsd 
	http://www.springframework.org/schema/mvc 
	http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd 
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx-4.2.xsd 
	http://www.springframework.org/schema/context 
	http://www.springframework.org/schema/context/spring-context-4.2.xsd ">

	<context:property-placeholder location="classpath:db.properties"/>
	<!--配置连接池-->
	<bean name="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="driverClass" value="${jdbc.driver}"></property>
		<property name="jdbcUrl" value="${jdbc.url}"></property>
		<property name="user" value="${jdbc.user}"></property>
		<property name="password" value="${jdbc.password}"></property>
	</bean>
	<!--配置工厂-->
	<bean name="factory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<property name="configLocation" value="classpath:mybatis/sqlMappingConfig.xml"></property>
	</bean>
	<!--配置mapper文件扫描-->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="top.xiesen.video.mapper"></property>
	</bean>
</beans>
<!--配置扫描注解-->
<context:component-scan base-package="top.xiesen.video"></context:component-scan>
```


4.编写springmvc.xml文件


5.编写db.properties文件

``` java
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///zy_video
jdbc.user=root
jdbc.password=root
```


