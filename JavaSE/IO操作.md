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

> 输出流和输入流是相对于我们的程序而言的，当我们向磁盘中写入文件的时候，对于我们的程序而言，需要将数据从程序输出并写入文件，所以这种流叫做输出流，当我们程序需要读取某个文件的时候，就需要将磁盘中的文件读取到程序当中，这种刘叫做输入流

## OutputStream 
> outputStream叫做字节输出流，每次操作的是1个字节(8位)，因为计算机中最小的存储单位是字节，所以OutputStream可以写入任何文件，OutputStream是抽象类，是表示输出字节的所有类的超类。操作的数据都是字节，定义了输出字节流的基本公共性功能方法。

![enter description here][3]

- `writer(int b)` 写入一个字节
- `writer(byte[] b)`写入字节数组
- `writer(byte[] b,int off,int len)`斜土字节数组,off表示从数组中那个索引开始写，len表示写多少个长度
- `close()` 关闭对象释放相关资源，有时如果不写close就会造成文件一致被占用无法删除

## FileOutputStream

> FileOutputStream类，即文件输出流，是用于将数据写入File的输出流

### 写入流程

- 创建FileOutputStream指定输出地址`FileOutputStream fos = new FileOutStream("d:/a,txt");`
- 如果路径中含有中间目录是无法写入成功的
- `fos.close(100);`将100转换成二进制写入文件，会参照对应ASCII码表
- 关闭输出liu`fos.close();`

### 写入方式

- 写入一个字节 `fos.writer(100);`
- 写入一个字节数组 `byte[] bytes = {65,66,67};fos.writer(bytes,0,2);`
- 如果要写入一个字符串，可以通过字符串调用`“大家好”.getBytes()` 方式将字符串转换成一个字节数组的方式
- 如果需要多次写入并且进行换行操作的话，需要添加`\r\n`,因为在windows中默认的换行符`\r\n`
- 如果需要每次写入内容是当前文件的尾部添加。而不是进行覆盖，就需要创建FileOutputStream的时候使用`FileOutputStream fos = new FileOutputStream("d:/a.txt",true);`

## InputStream
> InputStream叫做字节输入流,每次操作的是1个字节(8位),因为计算机中最小的存储单位是字节,所以InputStream可以读取任何文件InputStream此抽象类,是表示输入字节流的所有类的超类。操作的数据都是字节，定义了读取字节流的基本共性功能方法。

![enter description here][4]

- `int read()` : 读取一个字节并返回，没有字节返回-1.
- `int read(byte[])`:读取一定量的字节数，并存储到自己数组中，返回读取到的字节数。
- `void close()`:关闭输入流

### FileInputStream

> FileInputStream类，即文件输出流，是用于将数据读取到程序的输入流。

### 构造方法

![enter description here][5]

![enter description here][6]

- 读取单个字节

> read的返回值代表读取的内容 
> 文件末尾的值为 -1

``` java
File file = new File("d:\\a.txt");
FileInputStream fis = new FileInputStream(file);
int len = 0;
while((len = fis.read()) != -1){
System.out.print((char)len);
} f
is.close();
```
- 读取到字节数组

> read的返回值代表读取到了多少个有效字节
> 读取的内容放在byte中

``` java
File file = new File("d:\\a.txt");
FileInputStream fis = new FileInputStream(file);
byte [] b = new byte[2];
int len = 0;
while((len = fis.read(b)) != -1){
String str = new String(b, 0, len);
System.out.print(str);
}
```


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505818844720.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505820804886.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505822543939.jpg
  [4]: http://markdown.xiaoshujiang.com/img/spinner.gif "[[[1505823793772]]]"
  [5]: http://markdown.xiaoshujiang.com/img/spinner.gif "[[[1505824033750]]]"
  [6]: http://markdown.xiaoshujiang.com/img/spinner.gif "[[[1505824072537]]]"