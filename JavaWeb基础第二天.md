---
title: JavaWeb基础第二天
tags: HttpServletRequest，转发，重定向
grammar_cjkRuby: true
---


## HttpServletRequest

> 客户端发送过来的请求通过tomcat内核进行提取,然后将数据封装到HttpServletRequest对象中,在调用servlet的service方法的时候将此对象作为参数传递过来,HttpServlet在service内部调用doGet和doPost方法将此对象作为参数传递过来.

### 获取请求行内容