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
### 行内样式

> 直接在标签中写style属性进行赋值,style属性的"" 就相当于内联样式的{},把需要的属性按照上述要求写进去即可

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>好好学习,不要捣蛋</h1>
</body>
</html>
```
### 外部样式
> 单独写一个文件名为xxx.css,将css代码写入文件,在`<head>`z中进行引用

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>


	<!-- 外部样式 -->
	<link rel="stylesheet" type="text/css" href="css/index.css">

</head>
<body>
<h1>好好学习,不要捣蛋</h1>
</body>
</html>
```
> 注意: 如果一个标签有多个css样式,按照就近原则进行覆盖

## 选择器

> 我们要设置某些标志的显示样式,就必须让css找到对应的标签,我们可以通过选择器来找到对应的标签

### 常用选择器

- 标签选择器`标签类型{ }`,直接写标签的名字就行
- id选择器`#id名称{ }`,id不能重复,需要给标签添加一个id属性
- 类选择器`.class名称{ }` class可以重复,需要给标签加class属性
- 并集选择器 `选择器1,选择器2{ }`
- 属性选择器`标签[属性="具体属性值"]{ }` 例如`input[type="text"]{ }`

## 不常用的的选择器