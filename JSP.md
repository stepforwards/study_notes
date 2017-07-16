---
title: JSP
tags: JSP,JSTL,EL
grammar_cjkRuby: true
---

## Jsp指令

### page指令

> page指令 ： 通常位于jsp页面的顶端，同一个页面可以有多个page指令。

``` stylus
<%@ page 属性1=“属性值” 属性2=“属性值	1，属性值2”...属性n=“属性值”%>
```

|   属性  |   描述  |   默认值  |
| :---: | :---: | :---: |
| language    | 指定JSP使用的脚本语言    |   java  |
|  import   | 通过该属性来引用脚本语言中使用到的类文件    |  无   |
|  contentType   |  用来指定JSP页面采用的编码格式   | text/html,ISO-8859-1    |

> include指令：将一个外部文件嵌入到当前JSP文件中，同时解析这个页面的JSP语句。
> taglib指令：使用标签库定义新的自定义标签，在JSP页面中启用定制行为。

## JSP注释

> 在JSP页面的注释

> HTML的注释：`<!--HTML注释-->// 客户端可见`
> JSP的注释：`<%--jsp注释--%>//客户端不可见`
> JSP脚本注释 `// 单行注释；/*多行注释*/`

## JSP脚本
> 在JSP页面中执行的java代码
> 语法：`<% Java代码 %>`

## JSP声明
> 在JSP页面中定义变量或者方法。
> 语法：`<%! Java代码 %>`


