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


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505818844720.jpg