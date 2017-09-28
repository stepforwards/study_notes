---
title: Redis集群搭建
tags: linux,Redis,Java
grammar_cjkRuby: true
---

# 单机版安装Redis

> 1.  将Redis的安装包通过各种工具上传到Linux上，具体使用什么工具，这里就不说了。
> 2.  解压Redis `tar -xvf redis-3.0.0.tar.gz `
> 3.  因为Redis是用C语言书写的，所以需要安装C语言开发环境，对Redis进行编译，安装命令 `yum -y install gcc-c++`
> 4. 使用make命令进行编译
> 5.安装 `make FRIFX=安装路径 install`


![安装完成示意图][1]

## 启动
### 前台启动
> 进入到bin目录下，运行redis-sercer即可`./bin/redis-server`

![启动成功][2]

### 后台启动
> 将redis-3.0.0文件夹内的redis.conf文件复制到bin目录下，修改daemonize yes

![enter description here][3]

后台启动`./redis-server redis.conf`即可

![enter description here][4]

### redis客户端
在bin目录下执行`./redis-cli`打开客户端
在bin目录下执行 `./redis-cli shutdown` 关闭客户端

# 搭建Reids集群
   
 


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506579372821.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506579560706.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506579764600.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506579904818.jpg