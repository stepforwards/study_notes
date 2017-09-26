---
title: Redis
tags: Redis,linux
grammar_cjkRuby: true
---
# NoSQL
> NoSQL(Not Only Sql),是一项全新的数据库理念,和我们学过的MySQL和Orcal等这类的关系型数据库相对映,泛指非关系型数据库

# NoSQL优势
> 在目前的互联网项目中,经常面临的3个问题,1.高并发 2.高负载 3.高可用

> 高并发,在同一个时间内应用服务器会收到大量的请求,同样这些请求也要去操作我们的数据库服务器,每次读写操作就要发生磁盘的IO,对于关系型数据库而言,已经无法无法承受了
> 高负载,因为有大量的用户访问门户网站,每个用户访问的以及点击都会产生各种数据,对于数据库而言需要保存大量的数据,那么在关系型数据库中在一张有几亿条记录的表中进行sql查询,效率是及其低的
> 高可用,随着与日俱增的访问量和用户量,当数据库已经达到上线的时候,就需要进行相应的扩展,对于关系型数据库而言,在进行数据的扩展和迁移的时候,往往需要停机维护
> NoSQL数据库的产生,就是为了解决上述难题

# 主流的NoSQL产品

![enter description here][1]

# NoSQL特点
> NoSQL虽说种类繁多,但都具有如下特性,易扩展,高性能,高可用,灵活性数据模型

# Redis
> Redis是用c语言开发的一个开源的键值对数据库,官方提供的数据,对于50个并发,执行10万次请求,读的速度是110000次/s,写的速度的是81000次/s,它通过提供了多种键值类型满足多样化的需求,对于redis而言键的类型一般都为字符串,对应的值有5中类型

- 1.字符串类型(String)
- 2.散列类型(hash)
- 3.列表类型(list)
- 4.集合类型(set)
- 5.有序集合类型(sorted set)

# 使用场景
- 缓存(数据查询,新闻,商品信息)(使用最多)
- 任务队列(秒杀,抢购)
- 分布式集群的 session 的分离


# 安装

> 官方推荐的是在Linux下进行安装的,同样也有windows下的安装版,只不过window下的版本不是官方版本,安装windows版本,解压双击redis­server.exe即可,推荐在linux下进行安装,以下内容就是在linux中redis 
因为Redis是c语言开发的,所以需要c语言的编译环境,先编译后再安装 
启动后默认端口号 6379

- 安装redis编译的c环境, yum install gcc-c++
- 在 /opt/Software/ 目录中创建 Redis 目录
- 上传 redis-3.0.0.tar.gz 到 Redis 目录
- 进入此目录解压文件 tar -xvf redis-3.0.0.tar.gz
- 进入解压后的目录 cd redis-3.0.0
- 执行编译命令 make
- 在当前目录下执行安装命令, `make PREFIX=/opt/Software/Redis install` ,会在Redis目录下生成一个bin目录

# 前端启动
> 这种方式启动redis后,会造成当前终端界面无法退出,如果需要命令操作,需要再开启一个命令窗口,输入相关命令

- 此时进入bin目录可以启动redis  ./redis-server ,但是此时的命令行不能退出,如果退出了就意味着关闭了数据库 
	- 对于上述情况如果想操作redis的话,需要再建立一个连接,进入到 redis 中的 bin 目录 `cd /opt/Software/Redis/bin/`
	- 启动客户端 ./redis-cli -h ip地址 -p 端口号 ,如果连接本机可以写为 ./redis-cli
	- 输入命令  set name aaa 放入键值对, key 为 name , 值为 aaa
	- 输入命令  get name ,就能得到 aaa
	- 按键 ctrl+c 退出当前客户端,或者 exit

# 后台启动
> 对于前端启动,操作会很不方便,所以建议大家使用后台启动,即从后台启动后,不用再去打开新的窗口,进行相关操作

- 进入redis解压后的文件夹中执行命令 `cd /opt/Software/Redis/redis-3.0.0`
- 将 `redis.conf` 拷贝到 bin 目录下 `cp redis.conf ../bin/`
- 进入 bin 目录,编辑文件 `redis.conf` ,输入命令 `vim redis.conf`
- 将 `redis.conf` 文件中的 `daemonize` 从 `no` 修改成 `yes` 表示后台启动
- 启动redis-server `./redis-server redis.conf`
- 然后启动客户端 `./redis-cli -h ip地址 -p 端口号` ,如果连接本机可以写为 `./redis-cli`
- 输入命令  get name ,同样就能得到 aaa ,说明服务已经启动
- 输入 crtl+c 退出客户端
- 此时如果关闭服务器有两种方法 
	- 1.强制关闭,查询pid, `ps -ef | grep redis` ,然后再强行 kill -9 pid
	- 2.正常关闭,首先退出客户端,然后输入命令 `./redis-cli shutdown`
# Redis数据结构

- redis是一种高级的key­value的存储系统,其中value支持5种数据类型 
	- 1.字符串类型(String)
	- 2.散列类型(hash)
	- 3.列表类型(list)
	- 4.集合类型(set)
	- 5.有序集合类型(sorted set)


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506423474007.jpg