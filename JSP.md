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


> include指令：将一个外部文件嵌入到当前JSP文件中，同时解析这个页面的JSP语句。
> taglib指令：使用标签库定义新的自定义标签，在JSP页面中启用定制行为。