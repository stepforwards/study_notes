---
title: ssm整合
tags: idea,ssm,Java
grammar_cjkRuby: true
---

1.创建maven项目，导包

``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>top.xiesen</groupId>
  <artifactId>web-app</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
<properties>
		<!-- 单元测试 -->
		<junit.version>4.12</junit.version>
		<!-- Spring -->
		<spring.version>4.2.4.RELEASE</spring.version>
		<!-- Mybatis -->
		<mybatis.version>3.2.7</mybatis.version>
		<mybatis.spring.version>1.2.2</mybatis.spring.version>
		<!-- MySql -->
		<mysql.version>5.1.32</mysql.version>
		<!-- 连接池 -->
		<c3p0.version>0.9.1.2</c3p0.version>
		<!-- 日志处理 -->
		<slf4j.version>1.6.4</slf4j.version>
		<!-- Jackson Json处理工具包 -->
		<jackson.version>2.4.2</jackson.version>
		<!-- JSP相关 -->
		<jstl.version>1.2</jstl.version>
		<servlet-api.version>2.5</servlet-api.version>
		<jsp-api.version>2.0</jsp-api.version>
		<!-- Apache工具组件 -->
		<commons-lang3.version>3.3.2</commons-lang3.version>
		<commons-io.version>1.3.2</commons-io.version>
		<commons-net.version>3.3</commons-net.version>
		<!-- 文件上传组件 -->
		<commons-fileupload.version>1.3.1</commons-fileupload.version>
	</properties>
	<dependencyManagement>
		<dependencies>
			<!-- 单元测试 -->
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>${junit.version}</version>
				<scope>test</scope>
			</dependency>
			<!-- Spring -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-beans</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-webmvc</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-jdbc</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-aspects</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-jms</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context-support</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-test</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<!-- Mybatis -->
			<dependency>
				<groupId>org.mybatis</groupId>
				<artifactId>mybatis</artifactId>
				<version>${mybatis.version}</version>
			</dependency>
			<dependency>
				<groupId>org.mybatis</groupId>
				<artifactId>mybatis-spring</artifactId>
				<version>${mybatis.spring.version}</version>
			</dependency>
			<!-- MySql -->
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
				<version>${mysql.version}</version>
				<scope>runtime</scope>
			</dependency>
			<!-- 连接池 -->
			<dependency>
				<groupId>c3p0</groupId>
				<artifactId>c3p0</artifactId>
				<version>${c3p0.version}</version>
			</dependency>
			<!-- 日志处理 -->
			<dependency>
				<groupId>org.slf4j</groupId>
				<artifactId>slf4j-log4j12</artifactId>
				<version>${slf4j.version}</version>
			</dependency>
			<!-- Jackson Json处理工具包 -->
			<dependency>
				<groupId>com.fasterxml.jackson.core</groupId>
				<artifactId>jackson-databind</artifactId>
				<version>${jackson.version}</version>
			</dependency>
			<!-- JSP相关 -->
			<dependency>
				<groupId>jstl</groupId>
				<artifactId>jstl</artifactId>
				<version>${jstl.version}</version>
			</dependency>
			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>servlet-api</artifactId>
				<version>${servlet-api.version}</version>
				<scope>provided</scope>
			</dependency>
			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>jsp-api</artifactId>
				<version>${jsp-api.version}</version>
				<scope>provided</scope>
			</dependency>
			<!-- Apache工具组件 -->
			<dependency>
				<groupId>org.apache.commons</groupId>
				<artifactId>commons-lang3</artifactId>
				<version>${commons-lang3.version}</version>
			</dependency>
			<dependency>
				<groupId>org.apache.commons</groupId>
				<artifactId>commons-io</artifactId>
				<version>${commons-io.version}</version>
			</dependency>
			<dependency>
				<groupId>commons-net</groupId>
				<artifactId>commons-net</artifactId>
				<version>${commons-net.version}</version>
			</dependency>
			<!-- 文件上传组件 -->
			<dependency>
				<groupId>commons-fileupload</groupId>
				<artifactId>commons-fileupload</artifactId>
				<version>${commons-fileupload.version}</version>
			</dependency>
		</dependencies>
	</dependencyManagement>
	<!-- 依赖管理 -->
	 <dependencies>
		<!-- 单元测试 -->
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
			</dependency>
			<!-- Spring -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-beans</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-webmvc</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-jdbc</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-aspects</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-jms</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context-support</artifactId>
			</dependency>
			<!-- Mybatis -->
			<dependency>
				<groupId>org.mybatis</groupId>
				<artifactId>mybatis</artifactId>
			</dependency>
			<dependency>
				<groupId>org.mybatis</groupId>
				<artifactId>mybatis-spring</artifactId>
			</dependency>
			<!-- MySql -->
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
			</dependency>
			<!-- 连接池 -->
			<dependency>
				<groupId>c3p0</groupId>
				<artifactId>c3p0</artifactId>
			</dependency>
			<!-- 日志处理 -->
			<dependency>
				<groupId>org.slf4j</groupId>
				<artifactId>slf4j-log4j12</artifactId>
			</dependency>
			<!-- Jackson Json处理工具包 -->
			<dependency>
				<groupId>com.fasterxml.jackson.core</groupId>
				<artifactId>jackson-databind</artifactId>
			</dependency>
			<!-- JSP相关 -->
			<dependency>
				<groupId>jstl</groupId>
				<artifactId>jstl</artifactId>
			</dependency>
			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>servlet-api</artifactId>
			</dependency>
			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>jsp-api</artifactId>
			</dependency>
			<!-- Apache工具组件 -->
			<dependency>
				<groupId>org.apache.commons</groupId>
				<artifactId>commons-lang3</artifactId>
			</dependency>
			<dependency>
				<groupId>org.apache.commons</groupId>
				<artifactId>commons-io</artifactId>
			</dependency>
			<dependency>
				<groupId>commons-net</groupId>
				<artifactId>commons-net</artifactId>
			</dependency>
			<!-- 文件上传组件 -->
			<dependency>
				<groupId>commons-fileupload</groupId>
				<artifactId>commons-fileupload</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-test</artifactId>
			</dependency>
	 </dependencies>
		<build>
		<!--打包src下的xml文件-->
		<resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
                <include>**/*.tld</include>
            </includes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
                <include>**/*.tld</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
		<plugins>
<!--			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.5.1</version>  
				<configuration>
					<source>1.7</source>
					<target>1.7</target>
					<encoding>UTF-8</encoding>
				</configuration>
			</plugin>
-->			
			<!--tomcat7:run-->
	 		 <plugin>
 				<groupId>org.apache.tomcat.maven</groupId>
 				<artifactId>tomcat7-maven-plugin</artifactId>
 				<version>2.2</version>
 				<configuration>
 					<path>/hello</path>
 					<port>8081</port>
 				</configuration>
 			</plugin> 
		</plugins>
	</build>
		
</project>
```


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

``` xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://www.springframework.org/schema/beans" xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd ">

<context:component-scan base-package="top.xiesen.video"></context:component-scan>
<!-- 配置注解驱动，实际上是配置的映射器和处理器 -->
<mvc:annotation-driven/>
<!-- 配置视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<property name="suffix" value=".jsp"></property>
	<property name="prefix" value="/WEB-INF/view/"></property>
</bean> 
</beans>
```


5.编写db.properties文件

``` java
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///zy_video
jdbc.user=root
jdbc.password=root
```
6.编写web.xml文件

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" 
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" version="2.5">
  <display-name>ssm-video</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  
	<!-- 配置全局编码格式 -->  
   <filter>
  	<filter-name>CharacterEncodingFilter</filter-name>
  	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  	<init-param>
  		<param-name>encoding</param-name>
  		<param-value>UTF-8</param-value>
  	</init-param>
  </filter>
  <filter-mapping>
  	<filter-name>CharacterEncodingFilter</filter-name>
  	<url-pattern>*.action</url-pattern>
  </filter-mapping>
  
  
  <!-- 启动Web容器时，自动装配ApplicationContext的配置信息 -->
  <listener>
  	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  
  <context-param>
  	<param-name>contextConfigLocation</param-name>
  	<param-value>classpath:spring/applicationContext*.xml</param-value>
  </context-param>
  
  <!-- 配置springmvc前端控制器 -->
  <servlet>
  	<servlet-name>DispatcherServlet</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:spring/springmvc.xml</param-value>
  	</init-param>
  </servlet>
  <servlet-mapping>
  	<servlet-name>DispatcherServlet</servlet-name>
  	<url-pattern>*.action</url-pattern>
  </servlet-mapping>
  
</web-app>

```

# ajax 异步校验用户名是否存在

1. 添加jquery支持

``` javascript
<script type="text/javascript" src="${pageContext.request.contextPath }/static/js/jquery-1.12.4.min.js"></script>
```
2. 编写ajax校验代码，设置在页面加载的时候加载

``` javascript
$("#name").blur(function(){
	var value = $("#name").val();
	$.ajax({
		   type: "POST",
		   url: "${pageContext.request.contextPath}/user/checkForm.action",
		   data: "name=" + value,
		   success: function(message){
			   $("#msg").html(message);
		   }
	});
});
```

3. 编写url数据库校验代码

``` java
@RequestMapping(value="/checkForm.action")
public void checkForm(String name,HttpServletResponse response) throws IOException{
	response.setContentType("text/html;charset=UTF-8");
	User user = us.selectUserByName(name);
	if(user != null){
		response.getWriter().write("<b style='color: red'>用户名已存在</b>");
	}
}
```

# 注册校验

1.添加js支持

``` javascript
<script type="text/javascript" src="${pageContext.request.contextPath }/static/js/jquery-1.12.4.min.js"></script>
	<script type="text/javascript" src="${pageContext.request.contextPath }/static/js/jquery.validate.min.js"></script>
	<script type="text/javascript" src="${pageContext.request.contextPath }/static/js/messages_zh.min.js"></script>
```

2.编写校验规则

``` javascript
<script type="text/javascript">
	$(function(){
		$("#checkForm").validate({
			rules:{
				user:{//user是输入框的name属性值
					required:true,
					minlength:6,
					email:true
				},
				pwd:{
					required:true
				},
				rpwd:{
					required:true,
					equalTo:"input[name=pwd]"
				}
			},

			messages:{
				user:{//user是输入框的name属性值，保证rules和message对应
					required:"用户名不能为空",
					minlength:"长度太小"
				},
				pwd:{
					required:"请输入密码"

				},
				rpwd:{
					required:"请输入确认密码",
					equalTo:"两次密码不一致"
				}

			}
		});
	});
</script>
```

# idea


