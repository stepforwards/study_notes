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

![项目结构示意图][2]

- 在spring文件夹下创建springmvc.xml

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
	
	<!-- 配置扫描controller层 -->
	<context:component-scan base-package="top.xiesen.ssm"></context:component-scan>
	
	<!-- 设置注解处理器映射器,处理器适配器注解驱动 -->
	<mvc:annotation-driven/>
	
	<!-- 配置视图解析器前缀后缀 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/view"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>

</beans>
```

	- 设置注解处理器映射器，处理器适配器注解驱动
	- 配置视图解析器前缀后缀
	- 配置扫描controller层

- 在spring文件下创建applicationContext-dao.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd">


	<!-- 加载配置文件 -->
	<context:property-placeholder location="classpath:db.properties" />

	<!-- 配置连接池 -->
	<bean name="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="driverClass" value="${jdbc.driverClass}"></property>
		<property name="jdbcUrl" value="${jdbc.jdbcUrl}"></property>
		<property name="user" value="${jdbc.user}"></property>
		<property name="password" value="${jdbc.password}"></property>
	</bean>
	
	<bean name="sqlSessionFactor" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<property name="configLocation" value="classpath:mybatis/sqlMappingConfig.xml"></property>
	</bean>
	
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="top.xiesen.ssm.mapper"></property>
	</bean>
	
</beans>
```

	- 配置引入数据库db.properties
	- 配置数据库连接池
	- 配置sqlSessionFactory
	- 配置mapper扫描

- 在spring文件夹下创建applicationContext-service.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd">

<import resource="applicationContext-dao.xml"/>

<!-- 配置事务管理器 -->
	<bean name="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>  
	</bean>

	<!-- 配置事务通知 -->
	<tx:advice transaction-manager="transactionManager" id="txAdvice">
		<tx:attributes>
			<tx:method name="transfer" isolation="REPEATABLE_READ"
				read-only="false" propagation="REQUIRED" />
		</tx:attributes>
	</tx:advice>

	<!-- 配置切面 -->
	<aop:config>
		<aop:pointcut
			expression="execution(* top.xiesen.ssm.service.impl..*ServiceImpl.*(..))"
			id="pc" />
		<aop:advisor advice-ref="txAdvice" pointcut-ref="pc" />
	</aop:config>
	
</beans>
```

	- 配置扫描service层注解
- 在spring文件夹下创建applicationContext-tx.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd">

<import resource="applicationContext-service.xml"/>
<!-- 配置扫描service层注解 -->
<context:component-scan base-package="top.xiesen.ssm"></context:component-scan>
	
</beans>

```
 
	 - 配置spring事务
 - 在mybatis文件夹下创建sqlMapConfig.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" 
"mybatis-3-config.dtd">
<configuration>
	<!-- 配置别名 -->
	<typeAliases>
		<!-- <typeAlias type="top.xiesen.mybatis.pojo.User" alias="user"/> -->
		<!-- 会扫描当前包和子包下，会将别名设置为User user ，设置过别名以后再mapper文件中使用这个类的类型别名和全名都可以-->
		<package name="top.xiesen.ssm.model"/>
	</typeAliases>
</configuration>
```

- 通过逆向工程生成mapper文件和model类
- 给service层添加注解，并注入mapper

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
		mv.setViewName("/role/roleList");
		return mv;
	}
}
```

- 配置web.xml文件
	- 配置Spring容器
	- 配置DispatcherServlet


``` xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	id="WebApp_ID" version="2.5">
	<display-name>chapter31_ssm</display-name>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>

	<!-- 配置Spring容器 -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:spring/applicationContext-tx.xml</param-value>
	</context-param>

	<!-- 配置SpringMVC -->
	<servlet>
		<servlet-name>DispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:spring/springmvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>DispatcherServlet</servlet-name>
		<url-pattern>*.action</url-pattern>
	</servlet-mapping>

</web-app>
```


# 参数绑定
## 默认支持参数类型
> 如果我们定义了请求，响应，session，springmvc就会将HttpServletRequest，HttpServletResponse，HttpSession三个对象作为参数传递过来，我们在定义的时候还可以写Model/ModelMap
> Model/ModelMap是用来向页面进行传值使用的，这两个哪个都可以

``` java
@RequestMapping("/role/updateRole.action")
	public ModelAndView roleListId(HttpServletRequest request,HttpServletResponse response,HttpSession session,Model model,ModelMap modelMap){
		System.out.println(request+"---"+response+"----"+session+"------"+model+"-----"+modelMap);
		ModelAndView mv = new ModelAndView();
		String id = request.getParameter("id");
		System.out.println(id);
		Role role = rs.findRoleById(Integer.parseInt(id));
		mv.addObject("role", role);
		mv.setViewName("role/updateRole");
		return mv;
	}
```

## 简单参数类型

> 参数类型推荐使用包装数据类型，因为基础数据类型不可以为null

- 整形：Integer、int
- 字符串：STring
- 单精度：Float、float
- 双精度：Double、double
- 布尔值：Boolean、boolean

> 说明：对于布尔类型的参数，请求的参数为true或false。或者1或0
> 请求url：`http://localhost:8080/xxx.action?id=2&status=false
> 处理器方法：`public String editltem(Model model,Integer id, Boolean status)`

## @RequestParam

> 如果表单中的name属性和参数名称不一致的时候可以使用`@RequestParam` 建立映射关系，一旦使用了这个注解，传递的参数就必须含有改名称，就会报错误

![enter description here][3]

- required属性表示页面传过来的值是否是必须的，默认true
- defaultValue表示如果没有该值默认是多少

``` java
@RequestMapping("/role/updateRole.action")
	public ModelAndView roleListId(@RequestParam(value="id",defaultValue="1",required=true) Integer theid){
		ModelAndView mv = new ModelAndView();
		Role role = rs.findRoleById(theid);
		mv.addObject("role", role);
		mv.setViewName("role/updateRole");
		return mv;
	}
```

## 参数绑定model类

> 需保证表单的name属性和model中的属性中的属性一一对应

``` java
@RequestMapping("/role/editRole.action")
	public ModelAndView EditRole(Role role){
		ModelAndView mv = new ModelAndView();
		rs.updateRole(role);
		mv.addObject("role", role);
		mv.setViewName("role/success");
		return mv;
	}
```

![enter description here][4]

## 解决乱码

> 在web.xml中配置编码过滤器

``` xml
<!-- 配置全局编码 -->
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
```

## 参数绑定包装类

> 如果是包装类中的属性，需要将表单信息中的name属性名设置为类中属性的属性

![enter description here][5]

![enter description here][6]

``` java
@RequestMapping("/role/mulSubmit.action")
	public ModelAndView mulSubmit(RoleVo rv){
		ModelAndView mv = new ModelAndView();
		
		rs.updateMulRole(rv.getList());
		mv.setViewName("role/success");
		return mv;
	}
```

## 数组参数绑定
> 在表单如果有CheckBox这种多选框，提交到controller，可以通过数组的形式进行获取，那么需要将CheckBox的name属性设置为方法的参数的名称，也可以在包装类中定义成员变量的名字为表单的name属性

![enter description here][7]

![enter description here][8]

``` java
@RequestMapping("/role/editRole.action")
	public ModelAndView EditRole(RoleVo rv,String[] hobbys){
		ModelAndView mv = new ModelAndView();
		rs.updateRole(rv.getRole());
		System.out.println(rv);
		System.out.println(Arrays.toString(hobbys));
		mv.setViewName("role/success");
		return mv;
	}
```
## 集合类型参数绑定

> 在表单中如果牵扯到表单中的数据批量修改，并且每一行有多个数据都有更改且有多行，我们可以把每一行当作一个对象，把多行数据放入对象集合中进行提交，**不过这种方法仅仅适合于包装类，不能直接把值传入相应的参数**

![enter description here][9]

![enter description here][10]

``` java
	/**
	 * 这种情况仅仅适合包装类，不能将集合赋值给参数
	 * @param rv
	 * @return
	 */
	@RequestMapping("/role/mulSubmit.action")
	public ModelAndView mulSubmit(RoleVo rv /*,List<Role> list*/){
		//System.out.println(list);
		ModelAndView mv = new ModelAndView();
		rs.updateMulRole(rv.getList());
		mv.setViewName("role/success");
		return mv;
	}
```

## 自定义格式类型转换(了解即可)

> 可以进行日期的转换,以及数据的相应格式化(比如将表单提交的数据加前后缀)springmvc给我们提供一个转换器工程,我们可以通过这个转换器工程生产很多转换类(实现Convert接口)按照我们的要求进行数据的格式转换,对方法参数赋值的事情是处理器适配器做的,所以配置的工作应该交给处理器适配器,而在springmvc中我们已经通过注解配置了这个组件,所以配置应该在 <mvc:annotation-driven/> 进行设置

- 编写转换类DateConvert

``` java
/**
 * Converter<S, T>
 * S:source,需要转换的源的类型
 * T:target,需要转换的目标类型 
 */
public class DateConvert implements Converter<String,Timestamp>{

	@Override
	public Timestamp convert(String string) {
		DateFormat df = new SimpleDateFormat("yyyy+MM+dd HH:mm:ss");
		Timestamp timestamp =null;
		try {
			timestamp = new Timestamp(df.parse(string).getTime());
		} catch (ParseException e) {
			e.printStackTrace();
		}
		return timestamp;
	}
}

```

- 在springmvc中配置DateConvert，并注册到适配器中

``` xml
<!-- 2.设置注解处理器映射器,处理器适配器注解驱动 -->
	<mvc:annotation-driven conversion-service="formattingConversion" />
	<!-- 配置日期转换工厂 -->
	<bean name="formattingConversion"
		class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
		<property name="converters">
			<set>
				<bean class="top.xiesen.ssm.util.DateConvert"></bean>
			</set>
		</property>
	</bean>
```
> **注意页面显示和model类**

![enter description here][11]

``` java
    @DateTimeFormat(pattern="yyyy-MM-dd'T'HH:mm")
    private Date rUpdatetime;
```

![enter description here][12]

``` html?linenums
<td colspan="3" class="control">
	<input type="datetime-local" name="role.rUpdatetime" 
	value="<fmt:formatDate value="${role.rUpdatetime }" type="both" pattern="yyyy-MM-dd'T'HH:mm"/>" >
</td>
```

![enter description here][13]

``` java
@RequestMapping("/role/editRole.action")
	public ModelAndView EditRole(RoleVo rv,String[] hobbys){
		ModelAndView mv = new ModelAndView();
		rs.updateRole(rv.getRole());
		System.out.println(rv);
		System.out.println(Arrays.toString(hobbys));
		mv.setViewName("role/success");
		return mv;
	}
```


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502715804353.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502758550357.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502795616758.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502796009427.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502796264606.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502796313108.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502796264606.jpg
  [8]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502796660552.jpg
  [9]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502797001442.jpg
  [10]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502796264606.jpg
  [11]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502799634697.jpg
  [12]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502799685153.jpg
  [13]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502799718698.jpg