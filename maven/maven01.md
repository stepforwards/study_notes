---
title: maven01
tags: maven,ssm,Java,框架
grammar_cjkRuby: true
---

# Maven

> maven是apache下的一个开源项目,只能用maven来管理java工程,maven的主要作用是管理我们项目中所需要的jar包

# 下载安装 

- 下载地址是 [Apache maven project][1]
- 解压即安装,注意解压到一个没有中文和空格的目录最终

![enter description here][2]

- 配置环境变量
	- 电脑上需要安装jdk1.7以上的jdk,需要配置JAVA_HOME
	- 配置MAVEN_HOME

	![enter description here][3]

	- 将 `%MAVEN_HOME%/bin` 配置到环境变量path中

	![enter description here][4]
	
- 在cmd窗口输入 mvn -v 查看

![enter description here][5]

# maven的仓库

> maven是将用到的所有jar包,保存到仓库中,仓库分为三种,本地仓库,远程仓库,中央仓库

- 本地仓库是我们自己进行管理,在本机的电脑中存的
- 远程仓库(私服)有的公司会自己搭建一个属于公司内部的仓库
- 中央仓库,有maven团队进行管理,维护着大概两个亿的jar包
- 本地仓库没有所需要的jar包,会一级一级进行下载,如果没有远程仓库,本地仓库就会直接从中央仓库进行下载

# 配置本地仓库
- 将仓库进行解压到

![enter description here][6]

- 打开maven中的conf文件夹,编辑 `settings.xml`
- 配置本地仓库路径为

![enter description here][7]

- 在 `<profiles></profiles>` 标签中添加 jdk1.8 的支持, 默认是jdk1.5

``` xml
<profile>  
    <id>jdk18</id>  
    <activation>  
        <activeByDefault>true</activeByDefault>  
        <jdk>1.8</jdk>  
    </activation>  
    <properties>  
        <maven.compiler.source>1.8</maven.compiler.source>  
        <maven.compiler.target>1.8</maven.compiler.target>  
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>  
    </properties>   
</profile> 
```
![enter description here][8]

# maven的目录结构

> maven的目录结构具有一定的目录规范

- `src/main/java` 存放项目的.java文件
- `src/main/resources` 存放项目的资源文件.如spring和mybatis中的xml文件
- `src/main/webapp` web工程存放web资源
- `src/main/webapp/WEB-INF` 存放web.xml文件
- `src/test/java` 存放单元测试的.java文件.如Junit测试类
- `src/test/java` 存放单元测试的.java文件.如果没有会从main中寻找
- `taeget` 项目输出位置，编译后的class文件，打成的jar和war
- `pom` maven项目的核心配置文件

# maven 常用命令

> maven 本身集成了很多插件，例如有jdk插件和tomcat插件，所以可以在没有开发工具的前提下对项目进行清理，编译，运行，打包等操作，以下操作需要进入maven工程下

![enter description here][9]

## compile编译命令

> 通过mvn compile 可以编译 src/main/java 下的文件生成class文件输出到target文件夹，不会编译test的文件
> ![enter description here][10]

## test编译命令

> 通过 mvn test 可以编译src/test/java下的单元测试类
> ![enter description here][11]

## clean命令

> 通过 mvn clean 可以清除target下的文件

## package命令

> 通过 mvn package 可以将web项目打包成war,java项目打包成jar,放在target目录下,使用此命令会执
行 compile,test

## install命令

> 通过 mvn install 可以将对应的应用打包成war和jar并发布到本地仓库中,对于war包发布到本地仓库是没有用的,因为我们要进行依赖管理只会管理jar包,内部会执行 compile,test,package

## tomcat :run 命令

> 通过 mvn tomcat:run 可以使用内置的tomcat插件运行web应用,注意结束窗口使用 ctrl+v
> ![enter description here][12]

## 命令同时执行

> 可以通过例如 mvn clean install 同时执行两个命令

## maven项目构建

- eclipse需要安装m2e插件,我们的eclipse中已经集成了m2e插件,所以我们不需要再去下载m2e的插件
- eclipse中默认带了maven管理,我们可以使用我们下载的版本进行相关操作

![enter description here][13]

- 加载默认的配置文件,从我们配置的文件中加载

![enter description here][14]

- 新建maven项目

![enter description here][15]

- 跳过骨架操作，如果不跳过骨架,创建的工程的目录是不完整的

![enter description here][16]

- 设置坐标和打包方式

![enter description here][17]

	- jar表示java工程
	- war表示web工程
	- pom表示父工程(分模块开发的时候需要)

- 默认没有web.xml文件需要进行添加web.xml文件

![enter description here][18]

- 建立索引

![enter description here][19]

![enter description here][20]

- 创建servlet
- 添加jar的依赖, servlet-api 和 jsp-api

## maven的依赖管理

> 通过maven的依赖管理对项目中的jar包进行统一的管理

- <dependencies> 标签表示当前项目所依赖的jar包,其内部是 <dependency> 标签
- <dependency> 表示具体的依赖其内部有4个标签
- <groupId> 表示组织名+项目名
- <artifactId> 模块名称
- <version> 表示版本号
- <scope> 表示依赖范围
	- compile 表示编译时依赖,是默认值,会在编译,测试,运行的时候都要使用
	- provided 只在编译和测试时依赖,运行的时候不依赖,如 servlet-api 因为tomcat已经提供,所以运行的时候如果使用就会出现错误,所以设置为 provided
	- runtime 运行和测试的时候需要,编译的时候不需要,如jdbc驱动设置为runtime
	- test 测试的时候依赖,编译和运行的时候不依赖,如Junit
	- system 依赖范围和provided相同,但是需要提供一个磁盘路径,不推荐使用

	
``` xml
<dependencies>
	 <dependency>  
			<groupId>javax.servlet</groupId>  
			<artifactId>servlet-api</artifactId>  
			<version>2.3</version>  
			<scope>provided</scope>  
		</dependency>  
	</dependencies>
```

- `<scope>` 表示依赖范围

|  依赖范围   | 对于编译classpath有效    | 对于测试有效    |  对于运行时classpath有效   |  例子   |
| :---: | :---: | :---: | :---: | :---: |
|  compile   |  y   |  y   |   y  |  spring-core   |
|  test   |  -   |  y   |  y   |  Junit   |
|  provided   | y    |  y   |   -  |  servlet-api   |
|  runtime   |   -  |   y  |  y   |   JDBC驱动  |
| system    |   y  |   y  |   -  |  本地的，Maven仓库之外的类库   |

	- compile 表示编译时依赖,是默认值,会在编译,测试,运行的时候都要使用
	- provided 只在编译和测试时依赖,运行的时候不依赖,如 servlet-api 因为tomcat已经提供,所以运行的时候如果使用就会出现错误,所以设置为 provided
	- runtime 运行和测试的时候需要,编译的时候不需要,如jdbc驱动设置为runtime
	- test 测试的时候依赖,编译和运行的时候不依赖,如Junit
	- system 依赖范围和provided相同,但是需要提供一个磁盘路径,不推荐使用
``` xml
<dependencies>
	<dependency>
		<groupId>javax.servlet</groupId>
		<artifactId>servlet-api</artifactId>
		<version>2.5</version>
		<scope>provided</scope>
	</dependency>
</dependencies>
```
# maven的坐标查找

## 网页查找
如果我们需要使用jar包,就需要知道对应jar包的坐标,可以通过 [mvn repository][21] 进行查找

![enter description here][22]


## 搜索本地仓库

> 本地搜索需要先构建索引,前面已经介绍

![enter description here][23]

![enter description here][24]


## 配置插件

> maven中也可以配置很多插件,如jdk版本和tomcat的插件

- `<build>`  项目构建配置,配置编译,运行插件等
- `<plugins>`  内部放 `<plugin>` ,是管理的插件
- `<plugin>`  插件标签,内部配置该插件内部有 `<configuration>`
- `<configuration>` 表示配置信息
- 处理编译器版本

``` xml
<plugins>
	<plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-compiler-plugin</artifactId>
		<version>3.5.1</version>  
		<configuration>
				<source>1.7</source>
				<target>1.7</target>
				<encoding>UTF-8</encoding>
		</configuration>
	</plugin>
</plugins>
```
- tomcat插件

``` xml
 <plugin>
		<groupId>org.apache.tomcat.maven</groupId>
		<artifactId>tomcat7-maven-plugin</artifactId>
		<version>2.2</version>
		<configuration>
			<path>/hello</path>
			<port>8081</port>
		</configuration>
</plugin> 
```


  [1]: http://maven.apache.org/download.cgi
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502971217439.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502971340559.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502971420446.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502971469375.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502971656862.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502971769673.jpg
  [8]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502971857994.jpg
  [9]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502973301408.jpg
  [10]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502973457032.jpg
  [11]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502979428111.jpg
  [12]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502979724833.jpg
  [13]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502979809421.jpg
  [14]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502979850464.jpg
  [15]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502979919783.jpg
  [16]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502979961620.jpg
  [17]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502980092574.jpg
  [18]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502980182927.jpg
  [19]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502980252810.jpg
  [20]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502980694621.jpg
  [21]: https://mvnrepository.com/
  [22]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1503057025112.jpg
  [23]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1503057209747.jpg
  [24]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1503057309100.jpg