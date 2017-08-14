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

- 创建一个包叫做 controller 这个就是web层
- 在其中创建类,用来处理请求,使用注解的形式将其配置到spring容器中,在其中创建方法处理请求

``` java

@Controller
public class RoleControl {

	// @RequestMapping是处理请求地址映射的注解
	@RequestMapping("/role/roleList.action")
	public ModelAndView roleList(){
		
		ModelAndView mv = new ModelAndView();
		// 设置的测试数据
		List<Role> list = new ArrayList<>();
		list.add(new Role(110, "hasd110", "dasa110", new Timestamp(System.currentTimeMillis())));
		list.add(new Role(120, "hasd120", "dasa120", new Timestamp(System.currentTimeMillis())));
		list.add(new Role(130, "hasd130", "dasa130", new Timestamp(System.currentTimeMillis())));
		list.add(new Role(140, "hasd140", "dasa140", new Timestamp(System.currentTimeMillis())));
		
		mv.addObject("list", list);
		mv.setViewName("/WEB-INF/view/role/roleList.jsp");
		
		return mv;
		
	}
}
```

# springmvc架构

## springmvc组件介绍及配置

### DispatcherServlet：前端控制器

> 用户请求到达前端控制器，它就相当于mvc模式中的c，dispatcherServlet是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性。

### HandlerMapping：处理器映射器

> HandlerMapping负责根据用户请求url找到Handler即处理器，springmvc提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等,在controller中对应的方法使用的 @RequestMapping 就是注解的方式配置,在企业开发过程中也是目前使用最多的配置方式.

### Handler：处理器

> Handler 是继DispatcherServlet前端控制器的后端控制器，在DispatcherServlet的控制下Handler对具体的用户请求进行处理。由于Handler涉及到具体的用户业务请求，所以一般情况需要程序员根据业务需求开发Handler。

### HandlAdapter：处理器适配器

> 通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

### ViewResolver：视图解析器

> ViewResolver负责将处理结果生成View视图，View Resolver首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象，最后对View进行渲染将处理结果通过页面展示给用户。

### View：视图

> springmvc框架提供了很多的View视图类型的支持，包括：jstlView、freemarkerView、pdfView等。我们最常用的视图就是jsp。

### 说明

> 在springmvc的各个组件中，处理器映射器、处理器适配器、视图解析器称为springmvc的三大组件。可以认为是 **一个中心三个基本点** ,需要用户开发的组件有handler、view

### 配置相关组件

- 通过观察发现默认的处理器适配器和处理器映射器,在3.2后就推荐使用新的配置,所以我们要重新去配置新的处理器映射器和处理器适配器
- 在springmvc的配置文件中需要将新的处理器映射器和处理器适配器配置如容器中

``` xml
<!-- 配置处理器映射器 -->
	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"></bean>
	<!-- 配置处理器适配器 -->
	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"></bean>
```


## 注解驱动

> 直接配置处理器映射器和处理器适配器比较麻烦，可以使用注解驱动来加载。
> springmvc使用`<mvc:annotation-driven>` 自动加载RequestMappingHandlerMapping和RequestMappingHandlerAdapter可早springmvc.xml配置文件中使用`<mvc:annotation-driven>` 替代注解处理器和适配器的配置。

## 视图解析器

> 通过配置视图解析器的前缀和后缀在前端控制器将modelAndView交给视图解析器的时候，会自动添加前前缀和后缀

``` xml
<!-- 配置视图解析器前缀后缀 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/view"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
```


``` java
@Controller
public class RoleController {

	@Autowired
	RoleService rs;
	@RequestMapping("/role/roleList.action")
	public ModelAndView roleList(){

		ModelAndView mv = new ModelAndView();
		List<Role> roleList = rs.findAllRole();
		mv.addObject("list", roleList);
		// 自动添加前缀和后缀
		mv.setViewName("/role/roleList");
		return mv;
	}
}

```

# 静态资源放行

> 前端控制器的本质是一个servlet，所以`utl-pattern` 符合servlet的规则，但是springmvc是需要拦截请求才能使用框架进行处理的，所以关于路径匹配上一般不会使用完全路径匹配和目录匹配，一般设置为后缀名进行匹配（推荐使用这种方式），也可以使用 `/` 和 `/*` 是会对jsp进行拦截的，所以需要我们在controller中进行静态资源的放行操作，显然这种方式比较麻烦。
> **注意：如果前端控制器配置为扩展名匹配，是不需要设置静态资源放行的，强烈建议使用扩展名匹配**


## 方式一

> tomcat内部有一个叫做默认servlet的 url-pattern为 / 其可以处理静态资源,我们可以在配置文件中配置一个默认的servlet为这个,当所有路径都不匹配的时候让这个servlet进行处理,必须要配置注解驱动才能正常使用

``` xml
<mvc:default-servlet-handler/>
```
## 方式二

> 手动建立映射关系,  `/**`  表示可以有下一级文件夹,这种映射路径的方法,内部是由处理器映射器去执行具体的映射过程,所以必须要配置注解驱动如果不配置注解驱动会导致web应用无法访问,此方法的好处是路径可以直接在配置文件中进行指定

``` xml
<mvc:resources location="/image/" mapping="/image/**"/>
<mvc:resources location="/css/" mapping="/css/**"/>
<mvc:resources location="/js/" mapping="/js/**"/>
<mvc:resources location="/lib/" mapping="/lib/**"/>
```

# SSM整合

- 导包
- 创建config的source folder文件夹存放配置文件,在其内部创建两个文件夹spring(放spring配置文件)和mybatis(放mybatis的核心配置文件)
- 将db.properties文件和log4j.properties文件放在config的根目录



  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502715804353.jpg