---
title: JavaWeb-Jsp
tags: jsp语法,JSTL,EL
grammar_cjkRuby: true
---
## JSP

> 如果有个需求,不同的用户请求同一个界面在页面的最上方需要显示用户的名字我们可以使用
> response.getWriter.write(name),但是如果需要将用户的名字放在html界面中,那么我们就需要去拼接一个html界面,会很麻烦.如果能在html界面中直接获取name的值就会方便很多,所以jsp的实质就是在html界面中嵌入java代码

### jsp脚本

- `<%java代码%>` 相当于写在service方法中的
- `<%=java变量或表达式>` 相当于在service内部写了out.println
- `<%!java代码%>` ,实质上翻译成servlet,出现在成员变量的位置
- `<!--注释内容-->` ,源代码可见,翻译后的java文件可见,最终的html文件可见
- `//单行注释 /*多行注释*/ `源文件可见,翻译后的文件可见,最终
的html文件不可见