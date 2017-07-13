---
title: CSS基础
tags: CSS3
grammar_cjkRuby: true
---
[TOC]
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

### 不常用的的选择器
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

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>背景</title>
<style type="text/css">
	*{
		margin: 0px;
		padding: 0px;
	}
	
	.box1{
		width: 100%;
		height:1300px;
		background-color: rgb(255,255,0);
		background-image: url("img/1.jpg");
		background-repeat: no-repeat;
		/* background-position: right(水平) bottom(垂直); */
		background-position: 50px 50px;
		background-attachment:  fixed;
		
		background: red url("img/1.jpg") no-repeat 50px 50px fixed;
	}
</style>
</head>
<body>
<div class="box1"></div>
</body>
</html>
```
## 边框
> border属性,连写形式 宽度 样式 颜色

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>边框相关</title>
<style type="text/css">
	#box{
		width: 200px;
		height: 200px;
		background: red;
		border-radius: 10px;
		border: solid 2px black;
	}
</style>
</head>
<body>
<div id="box"></div>
</body>
</html>
```
## 盒子模型

### padding

> 内容到边框的距离叫做内边距,内边距属性按照的顺序是**逆时针方向--上右下左**

### margin
> margin设置为0 auto就代表距离上方0像素,距离左右两边水平居中

> 1. 元素的宽度: 左边边框 + 左边内边距 + 内容宽度 + 右边内边距 + 右边边框
>  2. 元素的高度: 上边边框 + 上边内边距 + 内容高度 + 下边的内边距 + 下边边框
>   3. 元素空间的宽度: 左边的外边距 + 元素的宽度 + 右边的外边距
>   4. 元素空间的高度: 上边的外边距 + 元素的高度 + 下边的外边距


## 标准流

> 1. HTML标签被分为两种类型,一种是块级标签(独占一行,可以设置宽高),一种是行内标签(不独占一行,不能设置宽高)
> 2. display属性: inline(行内), block(块级), inline-block(行内块级,能够设置宽高), none(隐藏,不占空间)

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>display属性</title>
<style type="text/css">
	span {
	width:100px;
	height:100px;
	background-color: red;
	display: inline-block;
}
</style>
</head>
<body>
<span >xxxxxxxxxxx</span><span style="background-color: blue;">qqqqqqqqqqqq</span><span >wwwwwwwww</span>


</body>
</html>
```
## 浮动流
### float

> 浮动流,设置属性float能让元素向左或向右进行浮动元素会脱离标准流,那么标准流的元素会相应定上来,其次脱离标准流,会在相应的标准流的行号上进行浮动
> 如果向左或向右进行浮动后,左右两边又有其他元素,就会紧贴那个元素浮动流不分行内和块级元素都可以设置宽高

### clear
> 设置clear属性,能够使元素在浮动的过程中不去贴靠其他元素,只能影响自己,不能影响其他元素

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>浮动流</title>
<style type="text/css">
	.box1{
		width: 100px;
		height: 100px;
		background-color: orange;
		float: left;
		
	}
	/* 
		浮动是相对父标签进行浮动的
		浮动流会托标,在标准流的第几行,浮动后也在第几行
		如果浮动了,并且左浮找右浮,进行贴靠
		clear清除自身浮动,清除自身不去贴靠其他
	 */
	.box2{
		width: 100px;
		height: 100px;
		background-color: purple;
		float: left;
		clear: left;
	}
	.box3{
		width: 100px;
		height: 100px;
		background-color: silver;
		float: left;
	}
</style>
</head>
<body>
	<div class="box1"></div>
	<div class="box2"></div>
	<div class="box3"></div>
</body>
</html>
```
## 定位流

### 相对定位
> 不会脱离标准流,相对于在标准流的位置进行偏移,所以下面元素不会顶上来position属性为relative,再结合 top right bottom left 四个属性进行位置的确定

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>相对定位流</title>
<style type="text/css">
	/* 相对定位不脱离标准流 
		top和left 相对于当前标签的位置进行偏移 
	 */
	.box1{
		width: 100px;
		height: 100px;
		background-color: orange;
		
	}
	
	.box2{
		width: 100px;
		height: 300px;
		background-color: purple;
		position: relative;
		top: 10px;
		left: 100px;
		
	}
	.box3{
		width: 100px;
		height: 100px;
		background-color: silver;
	}
	.big{
		width: 500px;
		height: 500px;
		background-color: red;
		
	}
	.small{
		width: 200px;
		height: 200px;
		background-color: gray;
		position: relative;
		left: 150px;
		top: 150px;
	}
</style>
</head>
<body>
	<div class="box1"></div>
	<div class="box2"></div>
	<div class="box3"></div>
	
	<div class="big">
		<div class="small"></div>
	</div>
</body>
</html>
```
### 绝对定位

> 会脱离标准流,所以下面元素不会顶上来,相对于在标准流的位置进行偏移设置postion为absolute,通过top right bottom left定位如果祖先元素没有定位流(相对定位,绝对定位,固定定位),绝对定位相对于body定位,如果祖先元素是定位流,绝对定位相对于祖先元素(就近原则)绝对定位不分行内和块级元素都可以设置宽高

``` stylus
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>绝对定位</title>
<!-- 
	绝对定位脱离标准流,不分行内和块级都能设置宽和高
	绝对定位的相对位置,会找到它的父标签,直到找到相对定位或者绝对定位的标签,如果没有找到,则找body
	一般设置"子绝父相"
 -->
<style type="text/css">
	 *{
	 	margin: 0px;
	 	padding: 0px;
	 }
	.box1{
		width: 100px;
		height: 100px;
		background-color: orange;
		
	}
	
	.box2{
		width: 100px;
		height: 300px;
		background-color: purple;
		position: absolute;
		top: 10px;
		left: 100px;
		
	}
	.box3{
		width: 100px;
		height: 100px;
		background-color: silver;
	}
	
	.big{
		width: 500px;
		height: 500px;
		background-color: red;
		position:absolute; 
		
	}
	.small{
		width: 200px;
		height: 200px;
		background-color: gray;
		position: absolute;
		top: 150px;
		left: 150px;
	}
	
	span{
		width: 100px;
		height: 100px;
		position:absolute;
		background-color: orange;
	}
</style>
</head>
<body>
<!-- 	<div class="box1"></div>
	<div class="box2"></div>
	<div class="box3"></div>
	
	<div class="big">
		<div class="small"></div>
	</div> -->
	
	<span>aaaaaaaaaa</span>
</body>
</html>
```
> 注意一般定位的时候注意准则为**子绝父相**

### 固定定位
> 固定定位脱离标准流
> 设置postion为fixed,通过top right bottom left定位
> 不区分行内和块级
> 和绝对定位相同,唯一不同就是不会随着滚动条滚动而滚动

``` stylus
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>固定定位</title>
	<style type="text/css">
	 div{
	 	width: 100px;
		height: 100px;
	 }
	.box1{
		background-color: orange;
	}
	
	.box2{
		height: 300px;
		background-color: purple;
		position: fixed;
		z-index: 1;
	}
	.box3{
		width: 100px;
		height: 2000px;
		background-color: silver;
		position: absolute;
		left: 0px;
		top: 0px;
	}
	
	</style>
</head>
<body>
	<div class="box1"></div>
	<div class="box2"></div>
	<div class="box3"></div>
</body>
</html>
```
## z-index
> z- index改变定位流中的前后顺序
> 默认情况下定位流会覆盖标准流
> 默认情况下后来的覆盖之前的
> 定位流中设置z-index,谁的值大谁在前面
> 如果父元素设置z-index,子元素的z-index就失效,谁的父元素大,谁就会显示在上方(从父原则)

``` stylus
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>固定定位</title>
	<style type="text/css">
	 div{
	 	width: 100px;
		height: 100px;
	 }
	.box1{
		background-color: orange;
	}
	
	.box2{
		height: 300px;
		background-color: purple;
		position: fixed;
		z-index: 1;
	}
	.box3{
		width: 100px;
		height: 2000px;
		background-color: silver;
		position: absolute;
		left: 0px;
		top: 0px;
	}
	
	</style>
</head>
<body>
	<div class="box1"></div>
	<div class="box2"></div>
	<div class="box3"></div>
</body>
</html>
```
