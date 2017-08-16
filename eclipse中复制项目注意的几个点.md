---
title: eclipse中复制项目注意的几个点
tags: 复制,eclipse,Java,copy
grammar_cjkRuby: true
---

# 复制项目

如果使用了工作集是不能直接进行项目的复制的，需要切换到Project状态
1. 点击需要复制的项目，直接Ctrl+C，Ctrl+V
2. 修改项目的名称为test01
3. 点击复制后的项目，右键选择properties，直接搜索web，选择Web Project Settings，修改Context root

![enter description here][1]

4.修改Context root

![enter description here][2]

![enter description here][3]

![enter description here][4]

5. 修改org.eclipse.wst.common.component 文件

![enter description here][5]


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502845418537.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502845530639.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502845610027.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502845690379.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502845833336.jpg