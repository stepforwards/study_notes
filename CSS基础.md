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
- 后代选择器`选择器1 选择器2{ }`会找到选择器1下的选择器2的所有后代
- 子元素选择器 `选择器1>选择器2{ }`,选择器1下的所有子标签符合选择器2的条件
- 交集选择器 `选择器1.选择器2{}`
- 相邻兄弟选择器: `选择器1+选择器2{ }`
- 通用兄弟选择器: `选择器1~选择器2{ }`
- 选中同级别的第一个标签: `标签: first-of-type`
- 选中同级别的最后一个标签: `标签:last-child{ }`
- 选中同级别中同类型的最后一个标签,: `标签:first-of-type`
- 选中同级别第几个标签, `标签: nth-child(3){ }`
- 选中同级别中同类型的第几个标签, `标签: nth-of-type(3)`

## 文本相关样式
> 用于设置文本类的相关样式

### font-style
> 设置文字样式,常用取值为italic和normal

``` stylus
font-style : italic;
```
### font-weight
> 设置文字的粗细 lighter,bold和border还可以进行数字取值100,900

``` stylus
font-weight: bold;
```
### font-size
> 设置文字的大小,取值是像素

``` stylus
font-size: 100px;
```
### font-family
> 设置字体"宋体"或者"微软雅黑"

``` stylus
font-family: "宋体";
```
### 连写形式

``` stylus
font: italic bold 100px "楷体";
```
### text-decoration
> 文本装饰属性,常用取值underline(下划线), line-through(删除线), overline(上划线), none(什么都没有) 可以用none去掉a标签的下划线

### text-align
> 对齐方式: left right center

### text-indent
> 缩进方式2em代表两个文字宽度

### color
> 设置颜色"red" 或者 rgb(255,0,0) 或者rgba(255,0,0,1) 或者#十六进制

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>诗词</title>
<style type="text/css">
	body{
		font-family:"楷体";
		background: gray;
	}
	div{
		text-align: center;
	}
	.content{
		font-size: 20px;
	}
	h1{
		font-family: "幼圆";
		color: rgba(255,0,0,0.8);
	}
	.color1{
		color: #177bd0;
	}
	.color2{
		color: #fe9901;
	}
	.color3{
		color: #20bde4;
	}
	.color4{
		color: #f8a4b4;
	}
</style>
</head>
<body>
	<div>
		<h1>登鹳雀楼</h1>
		<h5>王之涣</h5>
		<p class="content color1">白日依山尽,</p>
		<p class="content color2">黄河入海流.</p>
		<p class="content color3">欲穷千里目,</p>
		<p class="content color4">更上一层楼.</p>
	</div>

</body>
</html>
```
## 背景相关样式
> 设置标签的背景颜色

### background-color
> 设置颜色"red" 或者 rgb(255,0,0) 或者rgba(255,0,0,1) 或者#十六进制

### background-image
> 设置背景图片,`background-image: url("img/1.jpg");`会自动平铺

### background-repeat
> 设置平铺方式,有四个值,repeat(默认),no-repeat("不平铺"), repeat-x(水平平铺),repeat-y(垂直平铺)

### background-position
> 设置背景定位格式为: `水平方向数值 垂直方向数值`,水平各有left center right,垂直有 top center bottom,也可以是具体的像素值比如 100px 100px

### background-attachment

> 设置背景的关联方式常用有两个scorll(会随着滚动条的滚动而滚动), fixed(不会随着滚动而滚动)

### 连写形式
> background:   颜色 图片 平铺方式 关联方式 定位方式,任意写都可以