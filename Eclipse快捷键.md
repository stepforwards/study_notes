---
title: Eclipse快捷键
tags: Eclipse,快捷键
grammar_cjkRuby: true
---

### 几个最重要的快捷键

代码助手:Ctrl+Space（简体中文操作系统是Alt+/）
快速修正：Ctrl+1
单词补全：Alt+/
打开外部Java文档：Shift+F2

显示搜索对话框：Ctrl+H
快速Outline：Ctrl+O
打开资源：Ctrl+Shift+R
打开类型：Ctrl+Shift+T
显示重构菜单：Alt+Shift+T

上一个/下一个光标的位置：Alt+Left/Right 
上一个/下一个成员（成员对象或成员函数）：Ctrl+Shift+Up/Down
选中闭合元素：Alt+Shift+Up/Down/Left/Right
删除行：Ctrl+D
在当前行上插入一行：Ctrl+Shift+Enter
在当前行下插入一行： Shift+Enter
上下移动选中的行：Alt+Up/Down

组织导入：Ctrl+Shift+O

2 定位 
2.1行内定位 
行末/行首：End/Home
前一个/后一个单词：Ctrl+Right/Left
2.2文件内定位 
跳到某行：Ctrl+L
上下滚屏：Ctrl+Up/Down
上一个/下一个成员（成员对象或成员函数）：Ctrl+Shift+Up/Down
快速Outline：Ctrl+O 
2.3跨文件定位 
打开声明：F3
打开资源：Ctrl+Shift+R
打开类型：Ctrl+Shift+T
在workspace中搜索选中元素的声明：Ctrl+G
在workspace中搜索选中的文本：Ctrl+Alt+G
在workspace中搜索选中元素的引用：Ctrl+Shift+G
打开调用层次结构：Ctrl+Alt+H
快速层次结构：Ctrl+T
反悔：Ctrl+Z
2.4其它 
上一个/下一个光标所在位置：Alt+Left/Right
上一个编辑的位置：Ctrl+Q 

 

3 选中 
3.1行内选中 
选中到行末/行首：Shift+End/Home
选中上一个/下一个单词：Ctrl+Shift+Left/Right
3.2文件内选中 
选中闭合元素：Alt+Shift+Up
恢复到上一个选中：Alt+Shift+Down
选中下一个/上一个元素：Alt+Shift+Right/Left 

 

4 定位/选中/操作同时 
删除行：Ctrl+D
删除下一个/上一个单词：Ctrl+Delete/Backspace
删除到行末：Ctrl+Shift+Delete
在当前行上插入一行：Ctrl+Shift+Enter	
在当前行下插入一行： Shift+Enter
上下移动选中的行：Alt+Up/Down
拷贝选中的行：Ctrl+Alt+Up/Down 

 

5其它的代码编辑类快捷键 
保存：Ctrl+S
保存所有：Ctrl+Shift+S
下一个命中的项（搜索之后）：Ctrl+.
注释：Ctrl+/
添加导入：Ctrl+Shift+M
显示快捷键帮助：Ctrl+Shift+L
变为大/小写：Ctrl+Shift+X/Y

 

6 重构 
显示重构菜单：Alt+Shift+T
重构-改变方法签名：Alt+Shift+C
重构-移动：Alt+Shift+V
重构-重命名：Alt+Shift+R 

 

7 编辑器、视图、透视图切换 
下一个编辑器：Ctrl+F6
下一个视图：Ctrl+F7
下一个透视图：Ctrl+F8
最大化当前视图或编辑器：Ctrl+M
激活编辑器：F12 

 

8 Debug 
F5：Step Into（debug）
F6：Step over（debug）
F7：Step return（debug）
F8：Resume（debug）
F11：debug上一个应用（debug） 

 

9 Up/Down/Right/Left类快捷键 
Ctrl
前一个/后一个单词：Ctrl+Right/Left
上下滚屏：Ctrl+Up/Down
Alt
上一个/下一个光标的位置：Alt+Left/Right
上下移动选中的行：Alt+Up/Down
Shift
选中上一个/下一个字符：Shift+Left/Right
选中上一行/下一行（从当前光标位置开始）：Shift+Up/Down
Ctrl+Shift
上一个/下一个成员（成员对象或成员函数）：Ctrl+Shift+Up/Down
选中上一个/下一个单词：Ctrl+Shift+Left/Right
Alt+Shift
选中闭合元素：Alt+Shift+Up
恢复到上一个选中：Alt+Shift+Down
选中下一个/上一个元素：Alt+Shift+Right/Left
拷贝选中的行：Ctrl+Alt+Up/Down
Ctrl+Alt
拷贝选中的行：Ctrl+Alt+Up/Down 

 

10 F类快捷键 
F2：显示提示/重命名
F3：打开选中元素的声明
F4：打开选中元素的类型继承结构
F5：刷新
F5：Step Into（debug）
F6：Step over（debug）
F7：Step return（debug）
F8：Resume（debug）
F11：debug上一个应用（debug）
F12：激活编辑器

- ctrl+shift+r：打开资源

这可能是所有快捷键组合中最省时间的了。这组快捷键可以让你打开你的工作区中任何一个文件，而你只需要按下文件名或mask名中的前几个字母，比如applic*.xml。美中不足的是这组快捷键并非在所有视图下都能用。
- ctrl+o：快速outline

如果想要查看当前类的方法或某个特定方法，但又不想把代码拉上拉下，也不想使用查找功能的话，就用ctrl+o吧。它可以列出当前类中的所有方法及属性，你只需输入你想要查询的方法名，点击enter就能够直接跳转至你想去的位置。

- ctrl+e：快速转换编辑器

这组快捷键将帮助你在打开的编辑器之间浏览。使用ctrl+page down或ctrl+page up可以浏览前后的选项卡，但是在很多文件打开的状态下，ctrl+e会更加有效率。

- ctrl+2，L：为本地变量赋值

开发过程中，我常常先编写方法，如Calendar.getInstance()，然后通过ctrl+2快捷键将方法的计算结果赋值于一个本地变量 之上。 这样我节省了输入类名，变量名以及导入声明的时间。Ctrl+F的效果类似，不过效果是把方法的计算结果赋值于类中的域。

-  alt+shift+r：重命名

重命名属性及方法在几年前还是个很麻烦的事，需要大量使用搜索及替换，以至于代码变得零零散散的。今天的Java IDE提供源码处理功能，Eclipse也是一样。现在，变量和方法的重命名变得十分简单，你会习惯于在每次出现更好替代名称的时候都做一次重命名。要使 用这个功能，将鼠标移动至属性名或方法名上，按下alt+shift+r，输入新名称并点击回车。就此完成。如果你重命名的是类中的一个属性，你可以点击alt+shift+r两次，这会呼叫出源码处理对话框，可以实现get及set方法的自动重命名。

- alt+shift+l以及alt+shift+m：提取本地变量及方法

源码处理还包括从大块的代码中提取变量和方法的功能。比如，要从一个string创建一个常量，那么就选定文本并按下alt+shift+l即可。 如果同 一个string在同一类中的别处出现，它会被自动替换。方法提取也是个非常方便的功能。将大方法分解成较小的、充分定义的方法会极大的减少复杂度，并提 升代码的可测试性。

- shift+enter及ctrl+shift+enter

Shift+enter在当前行之下创建一个空白行，与光标是否在行末无关。Ctrl+shift+enter则在当前行之前插入空白行。

- Alt+方向键

这也是个节省时间的法宝。这个组合将当前行的内容往上或下移动。在try/catch部分，这个快捷方式尤其好使。

- ctrl+m

大显示屏幕能够提高工作效率是大家都知道的。Ctrl+m是编辑器窗口最大化的快捷键。

- ctrl+.及ctrl+1：下一个错误及快速修改

ctrl+.将光标移动至当前文件中的下一个报错处或警告处。这组快捷键我一般与ctrl+1一并使用，即修改建议的快捷键。新版Eclipse的 修改建 议做的很不错，可以帮你解决很多问题，如方法中的缺失参数，throw/catch exception，未执行的方法等等。


