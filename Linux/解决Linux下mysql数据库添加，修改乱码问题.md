---
title: 解决Linux下mysql数据库添加，修改乱码问题
tags: mysql,linux
grammar_cjkRuby: true
---
![乱码问题][1]
## 原因
> 出现乱码的原因是我们向数据库中写内容时，数据库中的编码格式与我们程序的编码格式不同。可以在mysql数据库中通过命令 `show variables like 'character_set%';` 查看数据库的编码格式

![数据库中默认的编码格式][2]

## 解决方案
> 如果/etc目录下存在my.cnf文件直接修改为如下代码。如果没有文件，将/usr/share/mysql/下面的my-default.cnf文件复制到/etc/下面，并改名为my.cnf  `cp /usr/share/mysql/my-default.cnf /etc/my.cnf`

``` xml
[client]
default-character-set=utf8

[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
user=mysql
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
#default-character-set=utf8
character-set-server=utf8
init_connect='SET NAMES utf8'

[mysql]
no-auto-rehash
default-character-set=utf8

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
```

然后重启mysql `/etc/init.d/mysql restart`


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506403579431.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506403823295.jpg