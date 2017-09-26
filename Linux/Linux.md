# 命令解析器

	shell--Unix操作系统
	bash--Linux操作系统
	本质：根据命令的名字，调用对应的可执行程序

# linux快捷键
> 命令和路径补全：Tab
> 主键盘快捷键
		1. 历史命令切换
		    历史命令:history
			向上遍历:ctrl + p
			向下遍历:ctrl + n--> n:next
		2. 光标移到
			向左:ctrl + b
			向右:ctrl + f
			移动到当前行头部:ctrl + a
			移动到当前行尾部:ctrl +　ｅ--> e:end
		3. 删除字符
			删除光标后面的字符:ctrl + d
			删除光标前面的字符:ctrl + h
			删除光标前的所有内容:ctrl + u
			
# Linux目录结构
	
- /root:该目录为系统管理员目录，也成为超级权限者的用户目录。
- /bin:是Binary的缩写，这个目录存放在最常使用的命令。
- /dev:是Devices(设备)的缩写，该目录下存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。
- /etc:这个目录中存放所有的系统管理需要的配置文件和子目录。
- /home:用户所在的目录，在Linux总，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。
- /boot:这里存放的是启动Linux时使用的一些核心文件，包括一些链接文件及镜像文件。
- /lib:这个目录里面存放着系统基本的动态链接共享库，其作用类似于Windows里的DLL文件，几乎所有的应用程序都需要用到这些共享库。
- /lost+found:这个目录一般情况下是空的，当系统非法关机后，这里存放了一些文件。
- /media:Linux系统会自动识别一些设备，例如U盘，当识别后，Linux会把识别设备挂载到这个目录下。
- /mnt:系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载到/mnt/上，然后进入该目录就可以查看光驱里的内容了。
- /opt:这是给主机额外安装软件的目录，比如你安装一个ORACLE数据库，则就可以放到这个目录下，默认是空的。
- /proc:这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。这个目录的内容不在硬盘上而是在内存里，我们可以直接修改里面的某些文件，比如可以通过下面的命令来屏蔽主机的ping命令，使别人无法ping你的机器。

		echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all


- /sbin:就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。
- /selinux:这个目录是RedHat/Centos所特有的目录，SeLinux是安全机制，类似于Windows的防火墙，但是这套机制比较复杂，这个目录就是存放selinux相关的文件的。
-  /srv:该目录存放一些服务启动之后需要提取的数据。
-  /sys:这是Linux2.6内核的一个很大的变化，该目录下安装了2.6内核中出现的一个文件系统sysfs。<br>sysfs文件系统集成了下面3种文件系统的信息，针对进程信息的proc文件系统，针对设备的devfs文件系统系统。<br>该文件系统是内核设备树的一个直观反射。<br>当一个 内核对象被创建的时候，对应的文件和目录页在内核对象系统文件中被创建。
-  /tmp:这个目录是用来存放一些临时文件的。
-  /usr:这是一个非常重要的目录，用户很多应用程序和文件都放在这个目录下，类似于Windows下的program files目录。
-  /usr/bin:系统用户使用的应用程序。
-  /usr/sbin:超级用户使用的比较高级的管理程序和系统守护程序。
-  /usr/src:内核源代码默认的放置目录。
-  /var:这个目录中存放着不断扩充的东西，我们习惯将经常被修改的目录放在这个目录下，包括各种日志文件

**注意：在Linux系统中，有几个目录比较重要的，平时需要注意不要误删或者随意更改内部文件。

- /etc:上面也提到了，这个是系统中的配置文件，如果你更改了该目录下的某个文件可能会导致系统不能启动。
- /bin、/sbin、/usr/bin、/usr/sbin:这些系统预设的执行文件的存放目录，比如ls就是在/bin/ls目录下的。<br>值得提出的是：/bin、/usr/bin是系统用户使用的指令(除root用户外的普通用户),而/sbin、/usr/sbin则是给root使用的指令
- /var:这是一个非常重要的目录，系统上跑了很多程序，那么一个程序都会有相应的日志文件产生，而这些日志都被记录在这个目录下，具体在/var/log目录下，另外mail的预设放置也在这里。


# 用户目录

	1>.绝对路径：从根目录开始写/home/xs/aa
	2>.相对路径:bb 相对于当前的工作目录
		* --> 当前目录
		** --> 当前的上一级目录
		- --> 在临近的两个目录直接切换 cd -


# 查看目录	
- ls -al  ：列出当前所在目录的所有文件和文件夹 
- tree ：以树形的样式列举出文件夹结构
- pwd：显示当前目录
- cd	： change directory的缩写,表示切换目录
- mkdir dirname:创建一个目录
- mkdir  -p aa/bb/cc :创建多级嵌套目录
- rmdir dirname:删除空目录，一般不使用
- rm -rif: r-->递归删除 i-->显示提示  f-->强制删除
- rm filename :删除文件 强烈建议添加参数-i提示删除
- touch 如果文件不存在，创建文件，存在，修改文件的时间
- cp filename : 如果copy的文件不存在，直接copy，如果存在，覆盖原来的文件
- cp dirname : 如果copy的目录不存在，则直接创建copy，如果存在，直接追加到该目录下
- cat : 查看文件-->文件过大，查看不方便
- more ：**（一般不使用，了解）** 可以查看较大文件，按下enter一行一行的切换，按下空格键，一次切换一屏，按下q键，退出查看模式-->缺陷：只能往下看，不能往回看
- less:**（一般不使用，了解）**可以查看较大文件，按下enter一行一行的切换，按下空格键，一次切换一屏，按下q键，退出查看模式，按组合键Ctrl+p可以往回查看，Ctrl+n向下查看,Ctrl+b向前翻页，Ctrl+f向下翻页
- tail -行号 例如使用 tail -10 /etc/sudo.conf 查看sudo.conf的后10行, ctrl+c 结束
> 注意：一般我们使用cat和tail比较多,常用于常看日志文件


## 常用快捷键

	历史命令向上移动:Ctrl+p
	历史命令向下移动:Ctrl+n
	删除光标后面的字符：光标 覆盖的字符：ctrl+d
	删除光标前面的字符：Ctrl+h backspace
	移动光标到行首：ctrl+a
	移动光标到行尾:ctrl+e
	自动补齐：tab

## 命令

- mv ： 移动 格式-->mv 移动的文件 移动到什么位置或修改的名字
	
## 创建快捷方式--软连接
	ln -s filename(文件所在的**绝对路径**)  创建的连接的名字
	注意：创建软连接的路径必须是绝对路径

## 备份--硬链接
	ln filename 创建硬链接的名字
> 1.硬链接像备份文件，但是和备份的文件还是有区别的。Windows上我们修改备份文件，不影响原文件。
> 2.但是Linux上面修改任意一个硬链接文件，都是能改动原文件的。所有的硬链接都是一个独立的文件，删除任意一个硬链接或者原文件，其他硬链接是不受影响的。
> 3.目录是不能创建硬链接的，但是可以创建软连接。

## 文件目录属性(了解)
	
	du : 查看当前目录下目录的使用情况 -h以人类可以看懂的方式显示，-a查看目录下的所有文件的使用情况，不仅仅是目录

	df:查看文件系统磁盘的使用情况 -h以人类可以看懂的方式显示.

	which : 查看命令所在的位置，如果which查不到命令所在的位置，则说明该命令是内嵌的，例如：cd

## 文件权限、用户、用户组

![权限示意图][1]

	1>.查看当前登录用户：whoami-->who am i 我是谁
	2>.修改文件权限
		1).文字设定法：chmod [who] [+|-|=] [mode]
			who:
				文件所有者 u -->user
				文件所属组 g -->group
				其他人 o -->other
				所有的人 a -->all 默认值
			+ :  添加权限
			- ： 减少权限
			= ： 覆盖原来的权限
			mode ：
				r : 读
				w : 写
				x ： 执行
		2).数字设定法：
			- ： 没有权限
			r : 4
			w : 2
			x : 1
			765
				7--rwx--文件所有者
				6--rw--文件所属组
				5--rx--其他人
	3>.改变文件或目录的所有者或所属组

	chown 所有者名称 文件名称-->修改文件所有者权限	
	chown 所有者名称：所属组名称 文件名称 --> 修改文件所有者权限和文件所属组权限
		
	4>.改变文件或目录的所属组

	chgrp 文件所属组名称 文件名称 -->修改文件所属组权限
	
### 查找和检索

	1>.按文件属性查找
		1).文件名 find + 查找的目录 + -name + "文件的名字"
		2).文件大小 find + 查找目录 + -size + +10k
		eg: find ~ -size +10M -size -100M 查找10M ~ 100M的文件
		3).文件类型 find + 查找目录 + -type 
			d --> 目录
			f --> 普通文件
			c --> 字符
			s --> 套接字
			b --> 块
			p --> 管道
			l --> 链接

	2>.按文件内容查找
		grep -r "查找的内容" 查找的路径

# 软件安装

### 在线安装
	
	1>.apt-get
		1).安装 sudo apt-get install tree
		2).移除 sudo apt-get remove tree
		3).更新 sudo apt-get update -- 更新软件列表
		4).清除所有软件安装包 sudo apt-get clean 
			实际上清理的是：/var/cache/apt/archives 目录下的.deb文件
	2>.aptitude
		1).安装 sudo aptitude instal tree
		2).重新安装 sudo aptitude reinstall tree
		3).更新 sudo apt-get update
		4).移除 sudo aptitude remove tree
		5).显示状态 sudo aptitude show tree

### deb包安装
	
	1>.安装 sudo dpkg -i xxx.deb
	2>.删除 sudo dpkg -r xxx

### 源码安装

	1>.解压源代码包
	2>.进入安装目录 : cd dir
	3>.检测文件是否缺失，创建Makefile,检测编译环境: ./configure
	4>.编译源码,生成库和执行程序:make
	5>.把库和可执行程序，安装到系统目录下 ：sudo make install
	6>.删除和卸载软件：sudo make distclean
	7>.上述安装步骤并不是绝对的，应该先查看附带的 Readme文件

### U盘的挂载

	1>.默认情况下，U盘自动挂载在/media目录下
	2>.手动挂载  mount 设备的名字 挂载点
		sudo fdisk -l 查看设备的名字

	卸载U盘 umount 挂载点

## 压缩包管理

	1>.屌丝版
		1).gzip --.gz格式压缩包,不能压缩目录，不能保留原文件
			压缩：gzip *.txt
			解压缩：gunzip *.gz  
		2).bzip2 -- .bz2格式的压缩包,不能压缩目录，添加参数-k可以保留原文件
			压缩：bzip2 *.txt
			解压缩：bunzip *.bz2
	2>.高富帅版
		1). tar -- 不使用z/j参数，该命令只能对文件或目录打包
			参数：
				c--创建--压缩
				x--释放--解压缩
				v--显示提示信息--压缩解压缩 --可以省略
				f--指定压缩文件的名字
				
				z--使用gzip的方式解压缩文件--.gz
				j--使用bzip2的方式压缩文件--.bz2
			压缩：
				tar zcvf 生成的压缩包的名字(xxx.tar.gz) 要压缩的文件或目录
				tar jcvf 生成的压缩包的名字(xxx.tar.bz2) 要压缩的文件或目录
			解压缩：
				tar jxvf 压缩包的名字(解压到当前目录)
				tar zxvf 压缩包的名字(解压到当前目录)
		2). rar -- 必须手动安装软件
			参数：
				压缩：a
				解压缩：x
			压缩：
				rar a 生成的压缩文件的名字(temp) 压缩的文件或目录
			解压缩：
				rar x 压缩文件名 (解压目录)
		3).zip
			参数：
				压缩目录要加参数 -r
			压缩：
				zip 生成的压缩包的名字 压缩的文件和目录
			解压缩：
				unzip 压缩包的名字
				unzip 压缩包的名字 -d 解压目录

		总结：相同之处：
		tar/rar/zip 参数 生成的压缩文件的名字 压缩的文件或目录 -- 压缩的时候的语法
		tar/rar/zip 参数 压缩包的名字 参数(rar没有参数) 解压缩目录--解压缩语法

## 进程管理

	1>.查看当前在线用户的情况：who
	2>.查看当前系统内部运行的进程状况：ps aux | grep bash
	3>.终止进程：
		1).查看信号编码：kill -l
		2).杀死进程：kill -9 pid
	4>.查看当前进程的环境变量：env
		查看PATH:env | grep PATH
	linux 下的环境变量的格式。 key-value
	5>.任务管理器：top

## 网路管理

	查看IP地址：ifconfig
	测试两台主机是否能通信：ping
	查看域名对应的ip地址：nslookup www.baidu.com

## 防火墙设置

> centos6和7中设置设置内容不同

- CentOS6.x 关闭防火墙: `service iptables stop`
- CentOS7.x 关闭防火墙 `systemctl stop firewalld`
- CentOS6.x 永久关闭防火墙(开机不自启): `chkconfig iptables off`
- CentOS7.x 永久关闭防火墙(开机不自启): `systemctl disable firewalld`
- CentOS6.x 查看防火墙当前状态: `service iptables status`
- CentOS7.x 查看防火墙当前状态: `systemctl status firewalld`


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506342708042.jpg