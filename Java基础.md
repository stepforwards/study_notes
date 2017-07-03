## Java简介

- Java是一种面向对象的程序设计语言,有Sun公司的James Gosling等人于1990年开发.它最初被命名为Oak,作为一种小家用电器的编程语言,来解决电视机、电话、闹钟、烤面包机等家用电器。由于这些智能化家电的市场没有预期的高，Sun放弃了该项计划。就在Oak几近夭折时，随着Internet的发展，Sun看到了Oak在计算机网络上的广阔应用前景，于是改造了Oak，在1995年5月以“Java“的名称正式发布了。Java伴随着Internet的迅猛发展而发展，逐渐成为重要的Internet编程语言，2009年4月20日这个曾经高达2000亿美元的Sun公司以74亿美元的价格被Oracle收购。
- 编写什么软件
	-  qq，迅雷，桌面应用软件
	-  淘宝，京东互联网应用软件
	-  可以做手机app
- 最擅长做的就是互联网应用软件
	-  OA（办公自动化）
	-   ERP（企业资源计划）
	-    CRM（客户关系管理）




## Java环境搭建
安装JDK 选择安装目录 安装过程中会出现两次 安装提示 。第一次是安装 jdk ，第二次是安装 jre 。建议两个都安装在同一个java文件夹中的不同文件夹中。（不能都安装在java文件夹的根目录下，jdk和jre安装在同一文件夹会出错）
如下图所示
![enter description here][1]
1. 安装jdk 随意选择目录 只需把默认安装目录 \java 之前的目录修改即可
.	
2. 安装jre→更改→ \java 之前目录和安装 jdk 目录相同即可
.	
注：若无安装目录要求，可全默认设置。无需做任何修改，两次均直接点下一步。
![enter description here][2]
![enter description here][3]
3. 安装完JDK后配置环境变量  计算机→属性→高级系统设置→高级→环境变量
![enter description here][4]
- 系统变量→新建 JAVA_HOME 变量 。
- 变量值填写jdk的安装目录（本人是 E:\Java\jdk1.7.0)
- 系统变量→寻找 Path 变量→编辑
- 在变量值最后输入 %JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
（注意原来Path的变量值末尾有没有;号，如果没有，先输入；号再输入上面的代码）
![enter description here][5]
- 系统变量→新建 CLASSPATH 变量,变量值填写   .;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar（注意最前面有一点）系统变量配置完毕
![enter description here][6]
检验是否配置成功 运行cmd 输入 java -version （java 和 -version 之间有空格）若如图所示 显示版本信息 则说明安装和配置成功。
![enter description here][7]

## Java基础


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071003627.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071120638.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071127908.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071154501.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071257408.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071298586.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499071331963.jpg