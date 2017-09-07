---
title: struts01
tags: struts2,框架,Java,ssh
grammar_cjkRuby: true
---


# 介绍

> Struts2是Struts1的下一代产品,是在Struts1和WebWork技术的基础上尽心合并的全新框架,(WebWork是由WebSymhony组织开发的),虽然说Struts2和Struts1名字相似,但是设计思想有很大的不同

# HelloWorld

- 下载 [https://struts.apache.org/][1]
- 目录结构

![struts2目录结构][2]

- 导包struts没有将包进行分类,所以我们可以在官方实例的 `struts-blank.war` 进行解压,在lib包中把jar包找到

![struts2ja包位置][3]

![enter description here][4]

- 创建一个普通java类HelloWorldAction,写一个返回值为String的方法叫hehe

``` java
public class HelloWorkAction {
    public String hehe(){
        System.out.println("hehehe");
        return "success";
    }
}
```

- 在 src 根目录下创建一个名字为 ==struts.xml== 的文件

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
        "-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
        "http://struts.apache.org/dtds/struts-2.3.dtd">
<struts>
    <!--
        struts.i18n.encoding=UTF-8 设置post的response和request响应请求的编码格式
        struts.enable.DynamicMethodInvocation = true 设置动态加载
        struts.devMode = true 设置热部署，以开发者的模式运行程序，出错了，提示信息比较丰富
        struts.handle.exception=true 设置后缀名
    -->
    <constant name="struts.i18n.encoding" value="UTF-8"></constant>
    <constant name="struts.devMode" value="true"></constant>
    <constant name="struts.action.extension" value="action,do,"></constant>

    <package name="haha" extends="struts-default" namespace="/hello">
        <action name="he" class="top.xiesen.struts.web.action.demo1.HelloWorkAction" method="hehe">
            <result name="success">/hehe.jsp</result>
        </action>
    </package>
    <include file="top/xiesen/struts/web/action/demo2/struts.xml"></include>
    <include file="top/xiesen/struts/web/action/demo3/struts.xml"></include>
    <include file="top/xiesen/struts/web/action/demo4/struts.xml"></include>
</struts>
```

- 在webcontent目录下,创建一个名为hehe的jsp文件
- web.xml中配置struts的核心过滤

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <filter>
        <filter-name>strutsFilter</filter-name>
        <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>strutsFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>
```

# structs.xml文件配置

## package标签

> <package> 配置web应用的不同模块,一般在一个功能模块下配置一个package,在当前的package下配置这个模块的多个action

- name属性 给不同的模块起不同的名字,随便写,不重复即可
- namespace属性 给不同的模块设置访问的根路径,可以配置成 /
- extends属性 表示继承, struts-default 是struts2给我们提供的一个 package



## action标签

> action 标签表示配置一个请求

- name 属性表示请求路径的后缀,一般表示功能模块中的具体请求,name的名字就代表访问路径的名称,==名字前面不能加斜杠==
- class 属性表示当有请求过来的时候调用的是哪个类中的方法,配置全类名
- method 表示 class 请求调用的是 class 中的哪个方法,指的是具体的方法名

## result标签

> result 结果配置,用于设置不同的方法返回值,可以配置不同的返回值对应不同的视图

- name 属性表示结果处理名称,与action中的返回值对应
- type 属性表示指定哪个 result 类来处理显示的页面,默认是内部转发,可以在 struts-default 的文件中进行查看
- 标签体表示相对路径,相对于 web应用开始

## include标签

> 如果有多个配置文件,可以在主配置文件中加载其他配置文件

# 常量配置

![默认常量位置][5]

> 对于常量的配置, 默认加载的是structs核心包中的default.properties,如果通过以下3中进行配置,就会按照 ==default.properties==-->==structs.xml==-->==structs.properties(我们应用中的)==-->==web.xml== 的顺序加载,后面设置的常量会覆盖之前设置的常量

1.在structs.xml文件中,在structs的根标签下,书写 constant 标签进行配置,在项目中
主要使用这种方式

![enter description here][6]

2.在src下创建structs.properties文件,将内容复制到此文件进行修改

![enter description here][7]

3.在web.xml文件中,配置 context-param

![enter description here][8]

## 常用常量设置

- struts.i18n.encoding=UTF-8 用于配置接收参数和向外输出中文的编码格式一般设置为 UTF-8
- struts.action.extension=action,, 指定访问action的路径的后缀名,使用 , 表示可以有两个后缀名,可以是action也可以是没有后缀名
- struts.devMode = false 指定structs是否是以开发模式运行,能够支持修改配置文件后进行热部署,所以我们可以将其设置为true

# 动态方法调用

> 如果一个业务模块有多个方法,我们可以使用动态方法调用省略action的配置,设置动态方法调用有两种方法

## 方法一

- 开启动态方法调用 `<constant name="struts.enable.DynamicMethodInvocation" value="true"></constant>`
- 配置action的时候不写method
- 在访问的时候输入网址 http://localhost:8080/Struts2Test/命名空间/action的name!方法名
- 注意action属性值后面要加 ==!== ,在写方法名

``` xml
<!--
        动态加载方式二：不推荐 使用此方法进行动态加载
        1. 设置常量struts.enable.DynamicMethodInvocation = true
        2. 不写method属性
        3. 访问路径：http://localhost:9090/struts/user/user!delete
    -->
<constant name="struts.enable.DynamicMethodInvocation" value="true"></constant>
<package name="demo3" extends="struts-default" namespace="/user">
	<action name="user" class="top.xiesen.struts.web.action.demo3.Demo3Action">
		<result name="success">/hehe.jsp</result>
	</action>
</package>
```
## 方法二 通配符方式

- 关闭动态方法调用
- 对于方法名可以使用一个 * 通配符,在后面的class和method可以使用 {索引} 来读取前面的内容

``` xml
 <!--
        动态加载方式一，推荐使用这种方式进行部署
        设置name属性值为通配符的形式，method方法设置统配的值
        访问路径：http://localhost:9090/struts/user/user_方法名
    -->
<package name="demo3" extends="struts-default" namespace="/user">
	<action name="user_*" class="top.xiesen.struts.web.action.demo3.Demo3Action" method="{1}">
		<result name="success">/hehe.jsp</result>
	</action>
</package>

```



  [1]: https://struts.apache.org/
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504785374788.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504785513165.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504785586699.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504786245782.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504786619717.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504786653176.jpg
  [8]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504786697450.jpg