---
title: CSS基础
tags: CSS3
grammar_cjkRuby: true
---

## CSS
> CSS(Cascading Style Sheets)是层叠样式表的简写,用于美化我们所写的HTML页面

- 格式

``` stylus
<style type="text/css">
	选择器{
		属性1:属性值;
		属性2:属性值;
		属性3:属性值1,属性值2,属性值3;
		.......
	}
</style>
```

> 选择器严格区分大小写,属性和属性值不区分大小写
> 属性与属性之间使用分号隔开,最后一个可以省略
> 如果一个属性有多个属性值,需要使用空格隔开
> type属性可以省略

## CSS样式的种类

### 内部样式
> 在head标签使用`<style>标签`

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

<style type="text/css">
/*内部样式*/
	h1{
		color: blue;
	}
</style>
</head>
<body>
<h1 style="color: red;">好好学习,不要捣蛋</h1>
</body>
</html>
```



