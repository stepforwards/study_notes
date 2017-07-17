---
title: JavaWeb基础第二天
tags: HttpServletRequest，转发，重定向
grammar_cjkRuby: true
---


## HttpServletRequest

> 客户端发送过来的请求通过tomcat内核进行提取,然后将数据封装到HttpServletRequest对象中,在调用servlet的service方法的时候将此对象作为参数传递过来,HttpServlet在service内部调用doGet和doPost方法将此对象作为参数传递过来.

### 获取请求行内容

#### 获取请求方式

``` stylus
	//获取请求方式
	String method = request.getMethod();
	System.out.println(method);//GET
```

#### 获取uri和url路径
- uri:统一资源标识符,通过request获取uri获取的是请求路径中web应用后的字符串不包含QueryString
- url:同一资源定位符,通过request获取的url是带有协议名称应用名称端口号的完全路径

``` stylus
//获取uri,就是请求行中的地址
String uri = request.getRequestURI();
//获取url,获取全路径
StringBuffer url = request.getRequestURL();
System.out.println(uri);///WebTest/tt
System.out.println(url);//http://192.168.6.154:8080/WebTest/tt
```
#### 获取web应用的名称

> 我们以后再写相对服务器的绝对路径就使用这种方式

``` stylus
//获取当前web应用名称
String webName = request.getContextPath();
System.out.println(webName);// /WebTest
```
#### 获取请求的ip地址(了解)

``` stylus
//获取请求的客户端的ip地址
String ipAddress = request.getRemoteAddr();
System.out.println(ipAddress);/192.168.6.171
```
#### 获取get请求表单数据(了解)

``` stylus
//获取get请求网址?后的内容,如果是post获取内容为null
String queryString = request.getQueryString();
System.out.println(queryString);//name=123
```
### 获取请求头的内容

#### 获取指定头的数据

``` stylus
//获取指定header的内容
String header = request.getHeader("User-Agent");
System.out.println(header);
//Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36
```
#### 获取所有头信息(了解)

``` stylus
//获取所有header的名字
	Enumeration<String> names = request.getHeaderNames();
	while(names.hasMoreElements()){
	String str = names.nextElement();
	String value = request.getHeader(str);
	System.out.println(str+"="+value);
}
```

### 获取请求体的内容

> 获取请求体中的提交的表单数据,尽管get请求数据在url后,用此方法同样可以获取


