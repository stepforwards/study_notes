---
title: HTML基础
tags: HTML,CSS
grammar_cjkRuby: true
---
[TOC]
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

### h系列标签
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

### p标签

> 段落标签,会独占一行

- 格式

``` stylus
<p>我是p标签,我独占一行</p>
```
### hr标签

> 分割线标签,会在页面上显示一条分割线
> size属性:水平线的高度

- 格式

``` stylus
<hr size="10px">
```

###  span标签

``` stylus
<span>span标签</span>
```
### font标签

> 设置文本的大小颜色等信息,不独占一行
> size属性:可以是#xxxxxx表示3原色,也可以是red,blue,green等
> 如果红绿蓝2位取值相同,可省略成1位,例如`#FF1133`简化成`#F13`

- 格式

``` stylus
<font>我是font标签1</font>
<font color="#3284c7">我是font标签1</font>
<font size="100px" color="red">我是font标签2</font>
```
###  b标签

> 出题标签,不会独占一行

- 格式

``` stylus
<b>粗体</b>
```
###  br标签
> 换行标签

- 格式

``` stylus
<font size="100px" color="red">我是font标签2</font><br>
<b>b标签</b> <br>
<i>斜体</i>
```

###  i标签

> 斜体标签,不会独占一行

- 格式

``` stylus
<i>斜体标签</i>
```
###  img标签

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
### a标签

> 超链接标签,不会独占一行
> href属性,指定点击后跳转的路径(url),如果需要点击后没有反应,需要写成:`javascript:void(0)`
> target属性: 指定跳转模式,_blank表示新建窗口,_self表示当前页,默认是_self
- 格式

``` stylus
<a href="https://www.baidu.com/" target="_blank">百度一下,你就知道</a>
<!-- 点击不跳转 -->
<a href="javascript:void(0)">不会问度娘</a>
```

### ol标签

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
### ul标签

> 无序列表,是组合标签,ul内部嵌套li标签
> type属性值: 取值范围是,disc(实心圆),circle(空心圆),square(方块)
> type默认属性值是disc
- 格式

``` stylus
	<ul>
		<li>北京</li>
		<li>上海</li>
		<li>杭州</li>
	</ul>
	<ul type="square">
		<li>北京</li>
		<li>上海</li>
		<li>杭州</li>
	</ul>
	<ul type="circle">
		<li>北京</li>
		<li>上海</li>
		<li>杭州</li>
	</ul>
	<ul type="disc">
		<li>北京</li>
		<li>上海</li>
		<li>杭州</li>
	</ul>
```
### table标签
> 表格标签,是组合标志,内置了很多子标签

- table标签的组成
	- `<table>`父标签,相当于表格容器
	- `<caption>`表格的标题,写在`<table>`内的第一行,用于指定表格的标题,会显示在表格的正上方
	- `<tr>`表示表格一行
	- `<th>`表格每一列的标题,写在`<tr>`内
	- `<td>`表格的每个单元格,写在`<tr>`内

``` stylus
	<table>
	<caption><h2>表格标题</h2></caption>
		<tr >
			<th>标题1</th>
			<th>标题2</th>
			<th>标题3</th>
		</tr>
		<tr align="center">
			<td>1.1</td>
			<td>1.2</td>
			<td>1.3</td>
		</tr>
		<tr align="center">
			<td>2.1</td>
			<td>2.2</td>
			<td>2.3</td>
		</tr>
		<tr align="center">
			<td>3.1</td>
			<td>3.2</td>
			<td>3.3</td>
		</tr>	
	</table>
```
- table标签的属性
	- border:表格边框的宽度
	- width:宽度,可以是像素也可以是百分比
	- height:高度,可以是像素也可以是百分比
	- align:水平对齐方式,常用left, center, right
	- valign:垂直对齐方式,常用 top, middle, bottom
	- cellspacing: 外边距,单元格与单元格之间的距离
	- cellpadding: 内边距,单元格与单元格之间的距离
	- bgcolor: 背景颜色

- table属性注意点
- 宽度和高度可以设置table标签和td标签
	-  1.1  table设置width和height设置表格宽度和高度
	- 1.2 td设置width和height,只会影响当前单元格,不会影响表格的宽度

- 水平对齐
	- 水平对齐可以设置table tr  td
	- table设置align,可以控制表格在水平方向的对齐方式
	- tr 设置align,可以控制当前行所有单元格内容的水平对齐方式
	- td 设置align,设置之前按照tr的对齐方式,设置后是控制当前单元格内容在水平方向的对齐方式

- 垂直对齐
	- 垂直对齐可以设置tr td
	- tr 设置valign,可以控制当前行所有单元格的垂直对齐方式
	- td 设置valign,设置之前按照tr 的对齐方式,设置后是控制当前单元格内容在垂直方向的对齐方式

- 单元格与单元格之间的距离叫外边距
	- 外边距cellspacing 只能给table设置,默认情况下边距是2px

- 单元格内容和单元格之间的距离叫做内边距
	- 内边距cellpadding 只能给table设置,默认是1px

- 背景颜色
	- table tr td 都可以设置
	- table设置整个表格背景颜色,tr设置当前行,td设置单元格
	- 如果都进行设置,就近原则

``` stylus
	<table  width="500px" height="300px" align="center" cellspacing = "1px" cellpadding= "0px" bgcolor="black">
	<caption><h2>表格标题</h2></caption>
		<tr bgcolor="white">
			<th>标题1</th>
			<th>标题2</th>
			<th>标题3</th>
		</tr>
		<tr align="center" bgcolor="white">
			<td>1.1</td>
			<td>1.2</td>
			<td>1.3</td>
		</tr>
		<tr align="center" bgcolor="white">
			<td>2.1</td>
			<td>2.2</td>
			<td>2.3</td>
		</tr>
		<tr align="center" bgcolor="white">
			<td>3.1</td>
			<td>3.2</td>
			<td>3.3</td>
		</tr>	
	</table>

```
- 细线表格
	- 去除边框
	- 设置表格背景颜色为black
	- 设置单元格背景颜色weiwhite
	- 设置外边距为1px


``` stylus
	<table  width="500px" height="300px" align="center" cellspacing = "1px" cellpadding= "0px" bgcolor="black">
	<caption><h2>表格标题</h2></caption>
		<tr bgcolor="white">
			<th>标题1</th>
			<th>标题2</th>
			<th>标题3</th>
		</tr>
		<tr align="center" bgcolor="white">
			<td>1.1</td>
			<td>1.2</td>
			<td>1.3</td>
		</tr>
		<tr align="center" bgcolor="white">
			<td>2.1</td>
			<td>2.2</td>
			<td>2.3</td>
		</tr>
		<tr align="center" bgcolor="white">
			<td>3.1</td>
			<td>3.2</td>
			<td>3.3</td>
		</tr>	
	</table>

```
- 单元格的合并们对于td而言
	- 水平方向上占据的列数`colspan`
	- 垂直方向上占据的行数`rowspan`

``` stylus
	<table  width="500px" height="300px" align="center" cellspacing = "1px" cellpadding= "0px" bgcolor="black">
	<caption><h2>单元格合并</h2></caption>
		<tr bgcolor="white">
			<th>标题1</th>
			<th>标题2</th>
			<th>标题3</th>
		</tr>
		<tr align="center" bgcolor="white">
			<td colspan="2">1.1</td>
			<td>1.2</td>
		</tr>
		<tr align="center" bgcolor="white">
			<td>2.1</td>
			<td rowspan="2">2.2</td>
			<td>2.3</td>
		</tr>
		<tr align="center" bgcolor="white">
			<td>3.1</td>
			<td>3.3</td>
		</tr>	
	</table>
```
## HTML中的表单标签
> 一般用于向服务器提交的时候将form标签中的数据进行提交
> action属性表示请求路径,表单提交到服务器的具体url
> method属性表示请求方式,一般取值为POST和GET,GET是默认值,提交的数据会追加到请求路径上,如.../...servlet?username=tom&password=123,数据以这种格式进行提交,多个数据之间用&链接,因为请求路径长度有限制,所以GET请求提交的数据有限
> POST提交,提交的数据不在网址的后面,我们是看不到的,是放在请求的请求体中,格式还是GET请求的格式

``` stylus
	<form action="#" method="POST">
			用户名:<input type="text" name="username"><br>
			密&nbsp;&nbsp;&nbsp;码:<input type="password" name="password"><br>
			<input type="submit">
		</form>
```
### input标签
> 用来获取用户输入信息的标签

