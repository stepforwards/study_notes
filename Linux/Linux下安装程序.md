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
- 输入命令 source /etc/profile 重新加载profile文件
- 在终端的任何地方输入 java -version ,如有相关jdk版本信息代表安装成功
> 注意如果配置失败 尝试安装此插件 yum install glibc.i686


# 安装Tomcat

- 上传tomcat到/opt/Software/Tomcat目录下
- 进入到当前目录进行解压操作, tar -xvf apache-tomcat-9.0.0.M26.tar.gz
- linux默认端口是不对外开发的,也就是默认情况下,外部是无法访问8080端口的,所以需要开放Linux的对外访问的端口8080, 
先执行命令 `/sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT`
然后保存设置 `/etc/rc.d/init.d/iptables save`
- 对于此问题,我们同样可以进行关闭防火墙的操作
- 进入tomcat的bin目录,进行启动 ./startup.sh
- 关闭tomcat的命令是 ./shutdown.sh