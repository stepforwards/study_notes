---
title: Redis集群搭建
tags: linux,Redis,Java
grammar_cjkRuby: true
---

# 单机版安装Redis
> 源码下载
	从官网下载 
	http://download.redis.io/releases/redis-3.0.0.tar.gz
	将redis-3.0.0.tar.gz拷贝到/usr/local下


> 解压源码
   tar -zxvf redis-3.0.0.tar.gz  
   
   
> 进入解压后的目录进行编译
	cd /usr/local/redis-3.0.0
	make
	
> 安装到指定目录,如 /usr/local/redis
	cd /usr/local/redis-3.0.0 
	make PREFIX=/usr/local/redis install

> redis.conf
redis.conf是redis的配置文件，redis.conf在redis源码目录。
注意修改port作为redis进程的端口,port默认6379。


>拷贝配置文件到安装目录下	
	进入源码目录，里面有一份配置文件 redis.conf，然后将其拷贝到安装路径下 
	cd /usr/local/redis
	mkdir conf
	cp /usr/local/redis-3.0.0/redis.conf  /usr/local/redis/bin


> 安装目录bin下的文件列表

![enter description here][1]


redis3.0新增的redis-sentinel是redis集群管理工具可实现高可用。

配置文件目录：

![enter description here][2]


# redis启动

## 前台启动

> 直接运行bin/redis-server将以前端模式启动，前端模式启动的缺点是ssh命令窗口关闭则redis-server程序结束，不推荐使用此方法。如下图：

![enter description here][3]


## 后台启动

> 修改redis.conf配置文件， daemonize yes 以后端模式启动。

> 执行如下命令启动redis：

``` jboss-cli
cd /usr/local/redis
./bin/redis-server ./redis.conf
```
![enter description here][4]

# 集群搭建

## 搭建集群需要的环境

搭建集群需要使用到官方提供的ruby脚本。
需要安装ruby的环境。

安装ruby
yum install ruby
yum install rubygems

redis集群管理工具redis-trib.rb
[root@bogon ~]# cd redis-3.0.0
[root@bogon redis-3.0.0]# cd src
[root@bogon src]# ll *.rb
-rwxrwxr-x. 1 root root 48141 Apr  1 07:01 redis-trib.rb
[root@bogon src]# 

脚本需要的ruby包：在redis-3.0.0/src的文件夹下

需要上传到linux服务。
安装ruby的包：
gem install redis-3.0.0.gem
[root@bogon ~]# gem install redis-3.0.0.gem 
Successfully installed redis-3.0.0
1 gem installed
Installing ri documentation for redis-3.0.0...
Installing RDoc documentation for redis-3.0.0...

## 集群的搭建

第一步：创建6个redis实例，端口号从7001~7006
第二步：修改redis的配置文件
1、修改端口号

![enter description here][5]

2、修改cluster-enable为如下图片

![enter description here][6]

第三步：把创建集群的ruby脚本复制到redis-cluster目录下。
第四步：启动6个redis实例
第五步：创建集群

``` stylus
./redis-trib.rb create --replicas 1 192.168.159.131:7001 192.168.159.131:7002 192.168.159.131:7003 192.168.159.131:7004 192.168.159.131:7005  192.168.159.131:7006
```

**注意：创建集群不能使用127.0.0.1.否则程序操作集群时，会出现获取不到集群中的值，如果不小心使用了127.0.0.1创建了集群，可以修改每个redis中的nodes.conf文件**

  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506596693371.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506596641999.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506596813212.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506596893766.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506597151900.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506597845668.jpg