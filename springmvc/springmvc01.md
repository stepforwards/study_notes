---
title: springmvc01
tags: springmvc,Spring,Java,框架
grammar_cjkRuby: true
---

[TOC]

# SpringMVC

> 全名是Spring web mvc是web层的框架,它是Spring框架的一部分

## springmvc处理流程

> 前端控制器(DispatcherServlet)是springmvc的心脏,用来拦截用户的请求,其本质是一个servlet,其收到请求后将数据交给处理器(handler)去执行相关处理

![springmvc处理流程图][1]

# Hello World

-  导包 4+2+aop+web+webmvc+jstl(1.2版本只需一个文件即可)
-  设置springmvc的约束文件
-  新建config的source fold
-  创建配置文件springmvc.xml,配置扫描controller包

## 编写springmvc的约束文件
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd">
	
	<context:component-scan base-package="top.xiesen.springmvc"></context:component-scan>
	
</beans>
```
## 配置web.xml文件

> 配置web.xml文件,配置前端控制器和 servlet 的初始化参数为 springmvc 配置文件的路径,和Spring相同 `param-name` 为 `contextConfigLocation` ,只不过这个初始化参数指的是servlet的初始化参数(如果不配置路径,springmvc会默认去找 `/WEB-INF/servlet名字-servlet.xml` 此文件,如果没有此文件就会报异常)

``` xml
<!-- 配置前端控制器DispatcherServlet -->
  <servlet>
		<servlet-name>DispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<!-- 自定义加载springmvc的路径，默认加载/WEB-INF/servlet-name-servlet.xml -->
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:springmvc.xml</param-value>
		</init-param>
		<!-- 设置随tomcat启动时加载 -->
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>DispatcherServlet</servlet-name>
		<url-pattern>*.action</url-pattern>
	</servlet-mapping>
```

## 编写controller层

  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502715804353.jpg