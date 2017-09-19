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



  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505818844720.jpg