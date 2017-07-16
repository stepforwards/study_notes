---
title: JSP
tags: JSP,JSTL,EL
grammar_cjkRuby: true
---

## Jsp指令

### page指令

> page指令 ： 通常位于jsp页面的顶端，同一个页面可以有多个page指令。

``` stylus
<%@ page 属性1=“属性值” 属性2=“属性值	1，属性值2”...属性n=“属性值”%>
```

|   属性  |   描述  |   默认值  |
| :---: | :---: | :---: |
| language    | 指定JSP使用的脚本语言    |   java  |
|  import   | 通过该属性来引用脚本语言中使用到的类文件    |  无   |
|  contentType   |  用来指定JSP页面采用的编码格式   | text/html,ISO-8859-1    |

``` stylus
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
```


> include指令：将一个外部文件嵌入到当前JSP文件中，同时解析这个页面的JSP语句。
> taglib指令：使用标签库定义新的自定义标签，在JSP页面中启用定制行为。

## JSP注释

> 在JSP页面的注释

> HTML的注释：`<!--HTML注释-->// 客户端可见`
> JSP的注释：`<%--jsp注释--%>//客户端不可见`
> JSP脚本注释 `// 单行注释；/*多行注释*/`

## JSP脚本
> 在JSP页面中执行的java代码
> 语法：`<% Java代码 %>`

``` stylus
<%
	/* 这是jsp脚本  */
	out.println("美好的一天开始了");
%>
```


## JSP声明
> 在JSP页面中定义变量或者方法。
> 语法：`<%! Java代码 %>`

``` stylus
<%! 
	String s = "张三";	// 声明一个字符串变量
	int add(int a ,int b){	// 声明一个返回int的函数，实现两个整数的求和
		return a + b;
	}
%>
```

## JSP表达式
> 在JSP页面中执行的表达式
> 语法： `<%=表达式 %> // 注意：表达式不以分号结束` 

``` stylus
	你好，<%=s %><br>
	x + y = <%=add(10,20) %>
```

## JSP页面生命周期
> jspService()方法被调用来处理客户端的请求。对每一个请求，jsp引擎创建一个新的线程来处理该请求。如果有多个客户端同时请求该jsp文件，则jsp引擎会创建多个线程。每个客户端请求对应一个线程。以多线程方式执行可以大大降低对系统的资源需求，提高系统的并发量及响应时间。但是也需要注意多线程带来的同步问题，由于该Servlet始终在内存中，所以响应是非常快的。

![JSP生命周期][1]

## JSP内置对象


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1500174427823.jpg