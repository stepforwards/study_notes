---
title: springmvc02
tags: springmvc,框架,Java
grammar_cjkRuby: true
---

# @RequestMapping
> 处理器映射器之所以能够从浏览器地址中将对应的地址映射出来,取决于 @RequestMapping 注解的value属性
> 因为前端控制器的匹配模式是 *.action 所以对于处理器映射器而言,请求能到达处理器映射器,肯定后缀名都是 .action 所以路径中不写的 .action 可以省略规范起见还是加上

# 多个路径匹配

> 其value属性是数组，可以同时匹配多个映射路径

![enter description here][1]

# 简化路径

> 如果当前类中的所有的路径的前缀都是相同的，那么可以使用@RequestMapping配置在类的上方，以下所有的方法都可以省去前面的那个路径。

![enter description here][2]

# 限定请求方法

> 可以通过@RequestMapping限定是GET请求还是POST请求，使用其method属性（是数组，也就是可以同时设置多个方式），属性类型是RequestMethod（枚举类型）

![enter description here][3]

**注意：页面出现405错误表示没有对应请求的set或get方法，如果出现错误，首先排查此项**

# controller方法的返回值

> 对于controller方法的返回值我们可以使用3中类型的返回值

## ModelAndView

> 返回数据和页面，其内部的原理是：数据放在Request域中，页面通过内部转发得到，我们之前一直使用的是这种类型，使用对象的addObject添加数据，使用`serViewName` 设置视图

![enter description here][4]

## void返回值

	> 如果返回值是void，则意味着请求处理完成后不会进行页面跳转，如果想要进行页面跳转，可以使用参数中的request和response（与其原生态的servlet跳转）还不如不将返回值设置为void

``` java
// 1 使用request进行转发request.getRequestD
ispatcher("/WEB-INF/jsp/success.jsp").forward(request,response);
//2 使用response进行重定向response.sendRedi
rect(req.getContextPath()+"/role/roleList.action");
```

> void 的主要用途在于我们以后要学习的ajax请求，因为ajax只需要返回数据，不需要返回视图

## String返回值

> 返回值是String，代表返回视图的名称，会配和前面在配置文件中配置的视图解析器，会加前缀和后缀，如果使用这种方式，可以使用参数中的model或者modelMap进行数据的填充（企业开发中推荐使用这种方式，因为数据模型和视图被分离，程序被解耦）

![enter description here][5]

## 内部转发

> 相对于执行了request的内部转发的操作，model中的数据存在，相当于一次请求
> `return "forward:/role/roleList.action;"`

## 重定向

> 相当于执行了重定向的操作，model中的数据没有`return "redirect:/role/roleList.action?id=" + id;`

**注意：此种方式的重定向是不需要添加web应用的名称，系统会自动为我们添加**

# 返回值是String注意问题

- 如果返回值是单纯的字符串，springmvc会添加前后缀，如果是重定向或者转发不会添加前后缀
- 如果是重定向的话此处的路径不写wen应用的名称，springmvc会默认添加

![enter description here][6]

- 如果我们使用response对象进行重定向，那么必须要写web应用名称，因为上一种是springmvc帮我们进行的重定向，使用response的话springmvc就不会给我们添加
- 如果是重定向的话model中的数据是不能使用的，因为model中的数据是保存在request域中


# 设置全局异常处理
> 系统中异常包括两类：预期异常和运行时异常RuntimeException，前者通过捕获异常从而获取异常信息，后者主要通过规范代码开发、测试通过手段减少运行时异常发生。
> 系统的dao、service、controller出现都通过throws Exception向上抛出，最后由springmvc前端控制器交由异常处理器进行异常处理，如下图

![enter description here][7]

> 所以我们要想处理这些从dao、service、controller抛出的这些异常,就需要我们自己写一个类实现异常处理器接口HandlerExceptionResolver,然后将此实现类配置到springmvc的配置文件中

``` java
    @Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object obj) throws Exception {
		
		if(request.getServletPath().equals("/user/login.action")){
			return true;
		}
		
		if(request.getSession().getAttribute("user") != null){
			return true;
		}
		request.getRequestDispatcher("/index.jsp").forward(request, response);
		return false;
	}
```



``` xml
<mvc:interceptors>
	<mvc:interceptor>
		<mvc:mapping path="/**"/>
		<mvc:exclude-mapping path=""/>
		<bean class="top.xiesen.ssm.intercept.LoginIntercept"></bean>
	</mvc:interceptor>
</mvc:interceptors>
```



1.自定义一个异常类实现 **HandlerExceptionResolver** ，重写 **resolveException** 方法
2.将异常类放到spring容器中，可以在springmvc文件中配置bean，`<bean class="top.xiesen.ssm.util.CustomException"></bean>` 或者直接在异常类上添加注解

``` java
@Controller
public class CustomException implements HandlerExceptionResolver{

	@Override
	public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object obj,
			Exception exception) {
		ModelAndView mav = new ModelAndView();
		mav.addObject("obj", obj);
		mav.addObject("exception", exception);
		mav.setViewName("role/error");
		return mav;
	}

}

```

# Spring图像上传

1.导包 `commons-io` 和 ` commonsfileuplod `
2.在jsp页面上添加上传空间 `<input type="file" name="pic">` name属性名特别注意不能喝model中的名字相同，因为此处上传的路径名不是唯一的，需要我们自己修改一下路径名。注意修改form表单的属性值 `<form action="<c:url value="/role/updateRole.action"/>" method="post" enctype="multipart/form-data">` 。
3.编写Controller实现类

``` java
@RequestMapping(value="/role/updateRole.action",method=RequestMethod.POST)
	public String EditRole(RoleVo rv,MultipartFile pic) throws Exception{
		// 产生随机的名字
		String str = UUID.randomUUID().toString().replaceAll("-", "");
		// 获取图像的后缀名
		String extension = FilenameUtils.getExtension(pic.getOriginalFilename());
		// 拼接新起的图片名
		String fileName = str + "." + extension;
		// 设置图像上传后的路径，
		String path = "D:\\Develop\\upload";
		// 拼接成图片访问路径
		pic.transferTo(new File(path + "\\" + fileName));
		// 把文件名给对象
		rv.getRole().setrPic(fileName);
		// 数据库更新
		rs.updateRole(rv.getRole());
		// 跳转页面
		return "redirect:/role/roleList.action";
	}
```
4.在springmvc中配置上传实现类

``` xml
<!-- 这个bean比较特殊，只能使用id进行标识，并且id必须是id="multipartResolver" -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
	<!-- 配置图片最大上传大小 -->
	<property name="maxUploadSize" value="6291456"></property>
</bean>
```


# 自定义分页标签

## 目录结构

![enter description here][8]


## 操作步骤

- 将tld文件夹拷贝到WEB-INF下
- 将NavigationTag和Page文件拷贝到src自定义目录
- 修改tld文件夹下的 commons.tld 文件,注意将 `<tagclass>` 标签改为 NavigationTag 的完整类名


``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE taglib
  PUBLIC "-//Sun Microsystems, Inc.//DTD JSP Tag Library 1.2//EN"
  "http://java.sun.com/dtd/web-jsptaglibrary_1_2.dtd">
<taglib>
	<tlib-version>2.0</tlib-version>
	<jsp-version>1.2</jsp-version>
	<short-name>common</short-name>
	<!-- 名字可以随便写,相当于引入标签库中的uri -->
	<uri>http://top.xiesen.com/common/</uri>
	<!-- 相当于给标签库起的名字 -->
	<display-name>Common Tag</display-name>
	<!-- 给标签库进行的描述 -->
	<description>Common Tag library</description>
	<!-- 在该标签库中定义一个标签 -->
	<tag>
	<!-- 标签的名称 -->
		<name>page</name>
		<!-- 和相关的类进行关联 -->
		<tag-class>top.xiesen.ssm.util.NavigationTag</tag-class>
		<body-content>JSP</body-content>
		<description>create navigation for paging</description>
		<!-- 定义标签中的一个属性名称是bean-->
		<attribute>
			<name>bean</name>
			<!-- 是否可以通过jsp表达式动态的进行赋值 -->
			<rtexprvalue>true</rtexprvalue>
		</attribute>
		<!-- 定义标签中的一个属性名称是number-->
		<attribute>
			<name>number</name>
			<!-- 是否可以通过jsp表达式动态的进行赋值 -->
			<rtexprvalue>true</rtexprvalue>
		</attribute>
		<!-- 定义标签中的属性 -->
		<attribute>
			<name>url</name>
			<!-- 是否是必须的 -->
			<required>true</required>
			<rtexprvalue>true</rtexprvalue>
		</attribute>
	</tag>
</taglib>
```

- 在需要引入分页标签的文件中添加标签库的引入,注意 uri 为之前定义的路径

``` html
<%@ taglib prefix="xs" uri="http://top.xiesen.com/common/" %>
```

- 在页面中添加分页,**注意url属性不能使用c标签,并且只有域中有名称为page且值为page对象的数据的时候,分页才能正常显示,否则分页是不会渲染的**

``` html
<xs:page url="${pageContext.request.contextPath }/user/userList.action"></xs:page>
```

- 在对应的controller中,需要向model中放入名称为page,值为page对象的数据
- page中有4个属性
	- total表示总条数
	- page表示当前页
	- size表示每页显示多少条
	- rows表示存放的数据
- NavigationTag中有3个属性
	- bean表示在jsp页面中放在request域中的key的值,默认是page
	- number表示分页标签中显示多少个页码(有默认值)
	- url表示点击分页后请求的地址(会自动读取请求中的数据进行拼接)
- 在处理分页的时候,一定要注意表单中的数据回填,因为 NavigationTag 会将回填的数据获取到,进行url上的拼接,所以当我们查看分页标签的链接地址的时候会发现其后面会默认拼接进行回填的数据,这样 controller 就能得到请求页和请求查询的数据进行查询

![enter description here][9]

- 创建分页对象的过程应该在service层进行创建

``` java
	@Override
	public Page loadPage(String userKeyWord, String userserarchfield, int currentPage) {
		Page<User> page = new Page<>();
		page.setPage(currentPage);
		page.setTotal(um.findAllUserCount(userKeyWord,userserarchfield));
		page.setSize(10);
		page.setRows(um.findAllUser(userKeyWord, userserarchfield,(currentPage-1)*10));
		return page;
	}
```


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1503062354825.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1503062595414.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1503062754546.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1503063324390.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1503063880822.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1503064386437.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1503066573977.jpg
  [8]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502970415410.jpg
  [9]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502970837820.jpg