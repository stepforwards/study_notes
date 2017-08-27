---
title: idea使用
tags:idea,Java
grammar_cjkRuby: true
---

# 在idea中开发web项目

1. 在idea中配置tomcat服务器
> settings -->applications server --> + --> 定位到tomcat解压目录 ---> ok

2. 创建java模块，添加javaEE 和maven的支持
3. 运行web程序

# 配置idea中tomcat热部署
1.关闭tomcat
2.run-->edit configuration
3.server选项卡 -- >VM options部分

> on "Update" action : update Classes and resources
> on Frame deactivarion : update Classes and resource

4.启动服务器要选择 "debug" 模式

# 在web项目中添加maven支持

![springmvc结构][1]


1. 添加springmvc的依赖

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>top.xiesen</groupId>
    <artifactId>myspringmvc</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>4.3.3.RELEASE</version>
        </dependency>

    </dependencies>
</project>
```

2. 在web/WEB-INF/web.xml中配置DispacherServlet分发器

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <!--配置分发器Servlet-->
    <servlet>
        <servlet-name>DispacherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispacherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

3. 配置springmvc配置文件，使用注解驱动配置项(默认名称dispacher-servlet.xml)
	[web/WEB-INF/dispacher-servlet.xml]
	

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd">

    <!--使用注解驱动-->
    <mvc:annotation-driven/>

</beans>
```

4. 编写控制器类

[ top.xiesen.springmvc.web.controller.HomeController]

``` java
package top.xiesen.springmvc.web.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * HomeController
 */
@Controller
public class HomeController {
    /**
     * 打开主页
     */
    @RequestMapping(value = {"/"})
    public String openHome(){
        System.out.println("Hello World");
        return null;
    }
    
}

```
5.在dispacher-servlet.xml文件中，增加扫描路径配置

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/mvc/spring-context-4.3.xsd">
    <!--配置扫描路径-->
    <context:component-scan base-package="top.xiesen.springmvc.web.controller"/>
    <!--使用注解驱动-->
    <mvc:annotation-driven/>

</beans>
```

6.启动程序，访问地址
	http://localhost:8080/springmvc/

7. 出现类找不到的原因。
	> idea de web项目默认不会将依赖库放置在WEB-INF/lib下，需要手动设置
	> project structure --> artifacts --> myspringmvc:war exploded --> 选择out layout选项卡--> 选择右侧的available elements 下 myspringmvc 条目的所有类 --> 右键-->put into WEB-INF/lib即可

8. 运行程序
9. 配置内部资源视图解析器(InteralResourceViewResolver)

  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1503814915477.jpg