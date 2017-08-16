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

1.导包
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
