---
title: JSP
tags: JSP,JSTL,EL
grammar_cjkRuby: true
---
[TOC]
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
## get与post区别
> get：以明文的方式通过URL提交数据，数据在URL中可以看到。提交的数据最大不超过2kB。安全性较低但效率比post方式高。适合提交数据量不大，安全性不高的数据。比如：搜索、查询等功能。
> post：将用户的信息封装在HTML head内。适合提交数据量大，安全性高的信息。比如：登录、注册、上传等功能
## JSP内置对象
> Jsp内置对象是web容器创建的一组对象，不使用new关键字就可以使用的内置对象。

### out对象
> out对象是JspWriter类的实例，是向客户端输出内容常用的对象。
- 常用方法：
	- void  println()向客户端打印字符串
	- void clear() 清除缓冲区的内容，如果在flush之后调用会抛出异常。
	- void clearBuffer() 清除缓冲区的内容，如果在flush之后调用不会抛出异常。
	- void flush() 将缓冲区内容输入到客户端
	- int getBufferSize() 返回缓冲区以字节数的大小，如不设缓冲区为0
	- int getRemaining() 返回缓冲区还剩余多少可用
	- boolean isAutoFlush() 返回缓冲区满时，是自动清空，还是抛出异常
	- void close() 关闭输出流

### request对象
> 客户端的请求信息被封装在request对象中，通过他才能了解到客户的需求，然后做出相应。他是HttpServletRequest类的实例。request对象具有请求域，即完成客户端的请求之前，该对象一致有效。

- 常用方法：
	- string getParameter(String name) 返回指定参数的参数值
	- String[] getParameterValues(String name) 返回包含name的所有组的数组
	- void setAttribute(String , Object) 存储此请求的属性值
	- object getAttribute(String name) 返回指定属性的属性值
	- String getContentType() 得到请求体的MIME类型
	- String getProtocol() 返回请求用的协议类型及版本号
	- String getServerName() 返回接收请求的服务器主机名



### response对象
> response 对象包含了响应客户端请求的有关信息，但是在JSP中很少直接使用它，它是HttpServletResponse类的实例。response对象具有页面作用域，即访问一个页面时，该页面的response对象只能对这次访问有效，其他页面的response对当前页面无效。

- 常用方法：
	- String getCharacterEncoding() 返回响应用的是何种字符编码
	- void setContentType(String type) 设置响应的MIME类型
	- PrintWriter getWriter() 返回可以向客户端输出字符的一个对象（注意比较：PrintWriter与内置out对象的区别：PrintWriter输出的语句提前于out输出的，解决的办法是，out.flush()）
	- sendRedirect(java.lang.String location) 重定向客户端的请求
#### 请求重定向和转发的区别

> 请求重定向：客户端行为，response.sendRedirect(),从本质上讲等同于两次请求，前一次的请求对象不会保存，地址栏的URL地址会改变。
> 请求转发：服务器行为，request.getRequestDispatcher().forward(req,resp);是一次请求，然后请求对象会保存，地址栏的URL地址不会改变。
> 请求重定向是客户器端行为而请求转发是服务器端行为

### session对象
> session 表示客户端与服务器的一次会话
> Web中的session指的是用户在浏览某个网站时，从进入网站到浏览器关闭所经过的这段时间，也就是用户浏览这个网站所花费的时间，实际上，session是一个特定的时间概念
- session对象是JSP内置对象
- session对象在第一个JSP页面被装载时自动创建，完成会话期管理。
- 从一个客户打开浏览器并连接到服务器开始，到客户关闭浏览器离开这个服务器结束，被称为一个会话。
- 当一个客户访问一个服务器时，可能会在服务器的及格页面之间切换，服务器应当通过某种办法知道这是一个客户，这需要session对象。
- session对象是Httpsession类的实例。

> session对象常用方法
- long getCreationTime()：返回session创建时间
- public String getId()：返回session创建时JSP引擎为它设的唯一ID号
- public Object setAttribute(String name,Object value)：使用指定名称将对象绑定到此会话
- public Object getAttribute(String name)：返回与此会话中的指定名称绑定在一起的对象，如果没有对象绑定在该名称下，则返回null
- String[] getValueNames()：返回一个包含此session中所有可用属性的数组
- int getMaxInactiveInterval()：返回两次请求间隔多长时间此session被取消（单位秒）

#### session的生命周期

- 某次坏话当中通过 超链接打开的页面属于同一次会话。
- 只要当前会话页面没有全部关闭，重新打开新的浏览器窗口访问同一个项目资源时属于同一次会话。
- 除非本次会话的所有页面都关闭后再重新访问某个JSP或者Servlet将会创建新的会话。

> 注意：原有会话还存在，只是这个旧的sessionId仍然存在于服务器，只不过再也没有客户端会携带它然后交于服务器校验。

session的销毁：
- 1. 调用了session.invalidate()方法
- 2. session过期(超时)
- 3. 服务器重新启动

![session的生命周期][2]

### application对象
- 和JavaSE中的static相似。
- application对象实现了用户间数据的共享，可存放全局变量。
- application开始于服务器的启动，终止于服务器的关闭。
- 在用户的前后链接或不同用户之间的链接中，可以对application对象的同一个属性进行操作。
- application对象是ServletContext类的实例。

> 常用方法

- public void setAttribute(String name,Object value)使用指定名称将对象绑定到此会话。
- public Object getAttribute(String name) 返回与此会话中指定名称绑定在一起的对象，如果没有对象绑定在该名称下，则返回null
- Enumeration getAttributeNames() 返回所有可用属性名的枚举
- String getServerInfo() 返回JSP（servlet）引擎名和版本号

### Page对象

> page对象就是指向当前JSP页面本身，有点像类中的this指针，它是java.lang.Object类的实例。常用方法如下：

- class getClass() 返回此类Object的类
- int hashCode() 返回此Object的hash码
- boolean equals(Object obj) 判断此Object是否与指定的Object对象相等
- void copy(Object obj) 把此Object拷贝到指定的Object对象中
- Object clone()克隆Object对象
- String toString() 把此Object对象转换成String类的对象
- void notity() 唤醒一个等待的线程
- void notityAll() 唤醒所有等待的线程
- void wait(int timeout) 使一个线程处于等待直到timeout结束或被唤醒
- void wait() 使一个线程处于等待直到被唤醒

### pageContext对象

- pageContext对象提供了对JSP页面内所有的对象及名字空间的访问
- pageContext对象可以访问到本页面所在的session，也可以取本页面所在的application的某一属性值
- pageContext对象相当于页面中所有功能的集大成者
- pageContext对象的本类名也叫pageContext

>常用方法

- JspWriter getOut() 返回当前客户端响应被使用的JspWriter流(out)
- HttpSession getSession() 返回当前页中的HttpSession对象(session)
- Object getPage() 返回当前页的Object对象(Page)
- ServletRequest getRequest() 返回当前页的ServletRequest对象(request)
- ServletResponse getResponde() 返回当前页的ServletResponse对象(Response)
- void setAttribute(String name,Object attribute)设置属性及属性值
- Object getAttributeScope(String name,int scope)在指定范围内取属性的值
- void forward(String realtiveUrlPath) 使当前页面重导到另一个页面
- void include(String realtiveUrlPath)在当前位置包含另一个文件

### exception对象
> exception对象是一个异常对象，当一个页面在运行过程中发生了异常，就产生这个对象。如果一个JSP页面应用此对象，就必须把isErrorPage设置为true，否则无法编译。它实际上是java。lang。Throwable的对象，常用方法如下：

- String getMessage() 返回描述异常的消息
- String toString() 返回关于异常的简短描述信息
- void printStackTrace() 显示异常及其栈轨迹
- Throwable FillInStackTrace() 重写异常的执行轨迹

### config对象
> config 对象是在Servlet初始化时，JSP引擎向它传递信息用的，此信息包括Servlet初始化时所要用到的参数(通过属性名和属性值构成)以及服务器的有关信息（通过传递一个ServletContext对象），常用方法如下：

- ServletContent getServletContext() 返回含有服务器相关信息的ServletContext对象
- StringgetInitParameter(String name) 返回初始化参数的值
- Enumeration getInitParameterNames() 返回Servlet初始化所有参数的枚举

## JavaBean
> JavaBeans就是符合某种特定的规范的Java类。使用JavaBean的好处就是解决代码重复编写，减少代码冗余，功能区分明确，提高了代码的维护性。

- JavaBean设计原则

![JavaBean设计原则][3]

- Jsp动作元素
 
> JSP动作元素(action elements)，动作元素是为请求处理阶段提供信息。动作元素遵循XML元素的语法，有一个包含元素名的开始标签，可以有属性、可选的内容、与开始标签匹配的结束标签。

  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1500174427823.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1500195279179.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1500301307794.jpg