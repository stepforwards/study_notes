---
title: IO操作
tags: IO,InputStream,OutputStream,Java
grammar_cjkRuby: true
---

# IO(Input&Output)
> 当需要把内存中的数据存储到持久化设备上这个动作称为输出（写）Output操作。
> 当把持久设备上的数据读取到内存中的这个动作称为输入（读）Input操作。

# 路径问题

- 在windows中的路径的分隔符 `\`
	- 使用java代码写的时候,因为 `\` 就是转义字符,所以
	- 应该写成 `\\` 也可一写成 `/`
- 在linux中的路径的分隔符是 `/`

# 构造方法

![file Constructor][1]

``` java
@Test
	public void test01() throws Exception{
		// 创建文件时，如果没有中间目录，系统会出现错误
		// 如果文件存在，创建文件的结果返回值也是false
		// 如果不指定路径，默认在当前工作空间下创建文件
		File file = new File("d:/a/a.txt");
		System.out.println(file.createNewFile());
		
		File file2 = new File("d:/a/","a.txt");
		File file3 = new File(new File("d:/a/"), "a.txt");
	}
```
# 创建删除目录和文件

- 创建文件
``` java
/**
 * 创建文件夹
 * 1. mkdir--只能创建一级目录，多级目录报错
 * 2. 创建多级目录，使用mkdirs
 */
File file4 = new File("d:/a/b");
boolean b = file4.mkdirs();
System.out.println(b);
```
- 删除

``` java
/**
 * 删除文件或文件夹
 * 1. 如果中间有目录，删除会失败
 * 2. 删除不会到回收站
 * 
 */

File file4 = new File("d:/a/b");
boolean b = file4.delete();
System.out.println(b);
```

# file的获取功能

> file.getName() 通过路径的 split("/") 数组的最后一个元素
> file.getPath() 得到创建file的路径,创建file时的路径是什么得到的就是什么
> file.getAbsolutePath() 获取绝对路径,如果是file中的路径是相对路径,得到的是相对于当前项目的绝对路径
> file.getAbsoluteFile() 返回的是文件
> file.length() 获取的是文件的字节数,long类型
> file.getParent() 得到的是父目录,返回值是String类型
> file.getParentFile() 得到的是父目录,返回值是file类型

![enter description here][2]

# File判断功能
**判断路径是否存在:** 

``` java
File file = new File("d:/abc/bcd/a.txt");
boolean exists = file.exists();
```


**判断是不是文件夹:** 

``` java
File file = new File("d:/abc/bcd");
boolean exists = file.isDirectory();
```

**判断是否是文件:** 

``` java
File file = new File("d:/abc/bcd/a.txt");
boolean exists = file.isFile();
```
# 遍历目录
	
``` java
@Test
public void test02(){
	File file = new File("D:\\Develop\\eclipse");
	// listFiles 打印的是绝对路径
	File[] list = file.listFiles();
	for (File string : list) {
		System.out.println(string);
	}
	System.out.println("-------------");
	// list打印的是相对路径
	String[] list2 = file.list();
	for (String string : list2) {
		System.out.println(string);
	}
	// 打印的是盘符
	File[] roots = File.listRoots();
	for (File file2 : roots) {
		System.out.println(file2);
	}
}
```

# 文件过滤器

``` java
@Test
public void test03(){
	File file = new File("D:\\Develop\\eclipse");
	// 文件过滤实现FileFilter接口即可
	File[] listFiles = file.listFiles(new FileFilter() {
		@Override
		public boolean accept(File pathname) {
			// 返回后缀名是.exe的文件
			return pathname.getName().toLowerCase().endsWith(".exe");
		}
	});
	for (File file2 : listFiles) {
		System.out.println(file2);
	}
}
```

- file.listFiles() 每次获取到一个文件或者文件夹的时候,就会调用对应接口的 accept 方法,将得到的这个文件或者文件夹作为参数进行传递
- accept() 方法如果返回值是true,就将此文件或者文件夹放入对应的数组中,所以我们可以用我们的代码进行控制返回值是 true 还是 false

# 输出流和输入流

> 输出流和输入流是相对于我们的程序而言的，当我们向磁盘中写入文件的时候，对于我们的程序而言，需要将数据从程序读取到某个文件的时候，对于我们的程序而言，需要将数据从程序写出并写入文件，所以这种流叫做输出流，当我们程序需要读取某个文件的时候，就需要将磁盘中的文件读取到程序当中，这种刘叫做输入流

## 



  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505818844720.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505820804886.jpg


