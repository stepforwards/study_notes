---
title: Linux下安装程序
tags: linux,jdk,mysql安装,Tomcat
grammar_cjkRuby: true
---

> 规范起见,统一在/opt目录下创建一个文件夹 Software 文件夹,并在Software,目录下创建3个文件夹, Java , Mysql , Tomcat

# rpm命令

> rpm与软件相关命令相当于window下的软件助手管理软件,通过使用rpm命令可以查看计算机中所有安装的软件

- -q 表示查询 query
- -a 表示查询所有 all
- -i 表示获取软件信息 info
- -l 表示获取软件列表 list
- -e 软件名 表示卸载删除指定软件 earser
- --nodeps 表示不考虑依赖关系强行删除,删除时一般写
- rpm -e --nodeps 软件名

# 安装JDK

- 查看当前Linux系统是否已经安装java rpm -qa | grep java
- 如果有已经安装的java程序,执行卸载命令  rpm -e --nodeps 要卸载的软件
- 上传jdk到 /opt/Software/Java 下
- 进入该目录,解压指定的jdk文件,  tar -xvf jdk-8u141-linux-x64.tar.gz
- 打开/etc/profile文件配置环境变量

``` xml
#set java environment
export JAVA_HOME=/opt/Software/Java/jdk1.8.0_141
export JRE_HOME=/opt/Software/Java/jdk1.8.0_141/jre
export CLASSPATH=.:$JRE_HOME/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
```

