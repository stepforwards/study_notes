## Java简介
### 概述
- Java是一种面向对象的程序设计语言,有Sun公司的James Gosling等人于1990年开发.它最初被命名为Oak,作为一种小家用电器的编程语言,来解决电视机、电话、闹钟、烤面包机等家用电器。由于这些智能化家电的市场没有预期的高，Sun放弃了该项计划。就在Oak几近夭折时，随着Internet的发展，Sun看到了Oak在计算机网络上的广阔应用前景，于是改造了Oak，在1995年5月以“Java“的名称正式发布了。Java伴随着Internet的迅猛发展而发展，逐渐成为重要的Internet编程语言，2009年4月20日这个曾经高达2000亿美元的Sun公司以74亿美元的价格被Oracle收购。
- 编写什么软件
	-  qq，迅雷，桌面应用软件
	-  淘宝，京东互联网应用软件
	-  可以做手机app
- 最擅长做的就是互联网应用软件
	-  OA（办公自动化）
	-   ERP（企业资源计划）
	-    CRM（客户关系管理）

### Java的三大平台 

- java ME（java的微型版本）应用于移动设备，机顶盒，汽车导航系统
-  Java SE（java的标准版）应用于桌面操作系统
-  Java EE（java的企业版本）应用于互联网大型系统，基于web

### java的特点

-  语言的简单性（类似于c 或者 c++，但是没有比较难理解的指针部分）
* 面向对象（更加方便的取理解，以及继承了面向对象的好处 ，如代码扩展和代码复用）
* 跨平台特性（一次编译，到处执行）
* 成熟的多线程模型（优势在于处理高并发）

### java的运行机制

![enter description here][1]

* 需要我们编写带有后缀名为.java的文件
* 使用命令将我们编写的.java文件进行编译，编译成class文件，即字节码文件
* 使用命令将.class文件交给虚拟机，让虚拟机取执行
* 虚拟机会做相应的合法安全检验判断
* 通过检验后，然后由虚拟机执行为不同平台计算机对应的机器码取执行
* 如果不能通过合法性的检测，虚拟机就会执行相应的异常程序
* 意义在于实现了跨平台

### java的开发环境
* JDK（Java Development kit），java的开发环境
* JRE（Java Runtime Kit）,java 的运行环境，虚拟机就在JRE中，JRE又包含在JDK中
* JVM（Java Virtual Machine）java虚拟机，运行java的工具，jvm又包含早jre中
*  jdk的下载地址 www.oracle.com/cn/index.html









## Java环境搭建
安装JDK 选择安装目录 安装过程中会出现两次 安装提示 。第一次是安装 jdk ，第二次是安装 jre 。建议两个都安装在同一个java文件夹中的不同文件夹中。（不能都安装在java文件夹的根目录下，jdk和jre安装在同一文件夹会出错）
如下图所示
![enter description here][2]
1. 安装jdk 随意选择目录 只需把默认安装目录 \java 之前的目录修改即可
.	
2. 安装jre→更改→ \java 之前目录和安装 jdk 目录相同即可
.	
注：若无安装目录要求，可全默认设置。无需做任何修改，两次均直接点下一步。
![enter description here][3]
![enter description here][4]
3. 安装完JDK后配置环境变量  计算机→属性→高级系统设置→高级→环境变量
![enter description here][5]
- 系统变量→新建 JAVA_HOME 变量 。
- 变量值填写jdk的安装目录（本人是 E:\Java\jdk1.7.0)
- 系统变量→寻找 Path 变量→编辑
- 在变量值最后输入 %JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
（注意原来Path的变量值末尾有没有;号，如果没有，先输入；号再输入上面的代码）
![enter description here][6]
- 系统变量→新建 CLASSPATH 变量,变量值填写   .;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar（注意最前面有一点）系统变量配置完毕
![enter description here][7]
检验是否配置成功 运行cmd 输入 java -version （java 和 -version 之间有空格）若如图所示 显示版本信息 则说明安装和配置成功。
![enter description here][8]

## Java基础

### Dos常用命令

* win + r —>输入 cmd打开命令行
* 注意Windows路径是\,Linux的路径是/
* 常用的命令
* 
	* 进入夏季目录： cd文件名
	* 列出当前文件夹下所有文件和文件夹
	* 在名字不重复的时候按tab键自动提示
	* 返回上以一级目录： cd..
	* 切换盘符 ： D:
	* 切换命令： cd
	* 清屏：cls
	* 按上下键重复上一行或者下一行

	* 测试java是否安装成功
	* 
		* 进入jdk的安装目录，执行java或者javac操作，出现提示，证明安装成功

### HelloWorld编写

1. 显示系统的扩展名
2. 在E盘建立Code文件夹，创建文本文档，命名为HelloWorld后缀名改为.java
3. 编写代码

``` stylus
public class HelloWorld {
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}	
}
```
4. 类名必须和文件名一致
5. 编译文件 : javac HelloWorld.java
6. 运行文件 ： java HelloWorld
### 注释
* 单行注释 // 注释内容
* 多行注释 /注释内容/
* 文档注释 /*注释内容/
* 注释的作用是让自己和让别人能够在最快的时间内明白代码的功能

### 标识符
* 由字母（a-zA-Z），数字（0-9），$和_组成
* 不能以数字开头
* 不能以数字开头
* 不能是系统的关键字
* 严格区分大小写，不限制长度，见名知意
* 建议使用驼峰命名
* 类名每个单词的首字母大写
* 变量和方法除了第一个单词以外，每个单词的首字母小写

### 常量
* 整数类型
* 小数类型
* 布尔类型
* 字符类型
* 字符串类型

### 变量
一般建议定义时赋值
* 变量未赋值不能使用
* 同一作用域中变量不能重复定义
数据类型的转换
* 自动转换 范围小的可以向范围大的进行转换
byte < short < int < long < float <double
* 强制类型转换 大范围的数向小范围的数转换
小范围类型 标识符 = (小范围类型) 大范围变量
定义类型是小写

### 转义字符
* 一些本身就赋予特殊意义的字符
* 输入双引号\”
* s输入回车 \n
* 输入制表符 \t
* 输入反斜杠 \


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071848807.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071003627.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071120638.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071127908.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071154501.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071257408.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071298586.jpg
  [8]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071331963.jpg