---
title: HTML基础
tags: HTML,CSS
grammar_cjkRuby: true
---

## HTML
> HTML是超文本标记语言,我们可以这样认为,HMTL就是普通的文本文件,只不过在文本中的内容如果被一些特殊的标签进行包裹就有了特殊的含义,这些被那些标记文本,就成了超文本
> 我们平时在浏览器中浏览的网页就是使用HTML语言进行编写的

## 网页的组成
- 网页的内容组成

1. HTML用于展示需要显示的信息
2. CSS用来美化我们的信息
3. JavaScript是页面具有一定的交互效果

- 根据内容的划分,可以将网页划分为静态页面和动态页面
	- 静态页面就是编写以后再浏览器中不再改变的网页, 如HTML
	- 动态页面就是可以根据不同的情况显示不同的内容,如jsp,php,动态页面也是在HTML的基础上添加的一些内容


## HTML的结构
- HTML不需要编译,直接用浏览器阅读即可
- 扩展名是.html和.htm
- 由标签组成
	- 标签不区分大小写
	- 单标签:以/结尾 如:`<img/>`,也可以不写
	- 双标签:有开始标签有结束标签`<p>`双标签`</p>`
- 内容结构

``` stylus
<!---dtd声明,html5的声明-->
<!DOCTYPE html>
<html lang="en">
	<!--头标签,一般用于引用脚本,样式导入,设置编码,字符编码-->
	<head>
	<!-- 设置编码格式-->
		<meta charset="UTF-8">
		<!--设置网页标题-->
		<title>HelloWorld</title>
	</head>
	
	<body>
		中华人民共和国万岁
	</body>
</html>
```
- 注释格式`<!--注释内容-->`

## HTML中的常用标签

1. h系列标签
>定义标题1-6,由大到小,独占一行

-  格式

``` stylus
	<h1>标题1</h1>
	<h2>标题2</h2>
	<h3>标题3</h3>
	<h4>标题4</h4>
	<h5>标题5</h5>
	<h6>标题6</h6>
	<p>我是p标签</p>
	<h7>标题只到6,如果写成7就和普通文字效果相同</h7>
```

2. p标签

> 段落标签,会独占一行

- 格式

``` stylus
<p>我是p标签,我独占一行</p>
```
3. hr标签

> 分割线标签,会在页面上显示一条分割线
> size属性:水平线的高度

- 格式

``` stylus
<hr size="10px">
```
4. span标签

``` stylus
<span>span标签</span>
```
5. font标签

> 设置文本的大小颜色等信息,不独占一行
> size属性:可以是#xxxxxx表示3原色,也可以是red,blue,green等
> 如果红绿蓝2位取值相同,可省略成1位,例如`#FF1133`简化成`#F13`

- 格式

``` stylus
<font>我是font标签1</font>
<font color="#3284c7">我是font标签1</font>
<font size="100px" color="red">我是font标签2</font>
```
6. b标签

> 出题标签,不会独占一行

- 格式

``` stylus
<b>粗体</b>
```
7. br标签
> 换行标签

- 格式

``` stylus
<font size="100px" color="red">我是font标签2</font><br>
<b>b标签</b> <br>
<i>斜体</i>
```

8. i标签

> 斜体标签,不会独占一行

- 格式

``` stylus
<i>斜体标签</i>
```
9. img标签

> 显示图片的标签,不会独占一行
> src属性:路径,注意路径问题
> alt属性: 图片无法加载的时候显示的文字
> width属性:设置宽,可以是像素值,也可以是百分比
> height属性: 设置高,可以是像素值,也可以是百分比
> title属性: 鼠标移上去后显示名字
> 我们一般会将所有图片放在img文件夹下便于管理

- 格式

``` stylus
<img alt="小清新" src="img/1.jpg">
<img alt="小清新" src="img/2.jpg" title="标题" width="100%" height="500px">
```
10. a标签

> 超链接标签,不会独占一行
> href属性,指定点击后跳转的路径(url),如果需要点击后没有反应,需要写成:`javascript:void(0)`
> target属性: 指定跳转模式,_blank表示新建窗口,_self表示当前页,默认是_self
- 格式

``` stylus
<a href="https://www.baidu.com/" target="_blank">百度一下,你就知道</a>
<!-- 点击不跳转 -->
<a href="javascript:void(0)">不会问度娘</a>
```

11. ol标签

> 有序列表,是组合标签,ol内部嵌套li标签
> type属性: 取值范围 , "A", "a", "I", "i", "1";
> 默认1,2,3
- 格式

``` stylus
	<ol>
		<li>吃得好</li>
		<li>学习好</li>
		<li>身体还要棒</li>
	</ol>
	<ol type="A">
		<li>吃得好</li>
		<li>学习好</li>
		<li>身体还要棒</li>
	</ol>
	<ol type="a">
		<li>吃得好</li>
		<li>学习好</li>
		<li>身体还要棒</li>
	</ol>
	<ol type="i">
		<li>吃得好</li>
		<li>学习好</li>
		<li>身体还要棒</li>
	</ol>
	<ol type="I">
		<li>吃得好</li>
		<li>学习好</li>
		<li>身体还要棒</li>
	</ol>
	<!-- 如果不是定义的type属性值,系统会执行默认属性值 -->
	<ol type="D">
		<li>吃得好</li>
		<li>学习好</li>
		<li>身体还要棒</li>
	</ol>

```
12.ul标签

> 无序列表,是组合标签,ul内部嵌套li标签
> type属性值: 取值范围是,disc(实心圆),circle(空心圆),square(方块)
> type默认属性值是disc
- 格式

``` stylus

```


