---
title: 解决Linux下mysql数据库添加，修改乱码问题
tags: mysql,linux
grammar_cjkRuby: true
---


> 如果/etc目录下存在my.cnf文件直接修改。没有文件，将/usr/share/mysql/下面的my-default.cnf文件复制到/etc/下面，并改名为my.cnf `cp my-default.cnf /etc/my.cnf`

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
