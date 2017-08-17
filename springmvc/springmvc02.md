---
title: springmvc02
tags: springmvc,框架,Java
grammar_cjkRuby: true
---

# 设置全局异常处理

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

![enter description here][1]


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

![enter description here][2]

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



  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502970415410.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502970837820.jpg