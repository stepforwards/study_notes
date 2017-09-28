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

## 后台启动


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506596693371.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506596641999.jpg