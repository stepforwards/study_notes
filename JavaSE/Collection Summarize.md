---
title: Collection Summarize
tags: Collection,ArrayList,LinkList
grammar_cjkRuby: true
---

# ArrayList原理

1. ArrayList是Collection的一个子类，具备Collection的全部特点，有序，有索引，能重复
2. ArrayList的底层实现是一个数组，ArrayList将数据进行了动态的扩容
3. ArrayList查询方便，添加复杂

> 下面我们看一下ArrayList的添加功能的内部实现

``` java
Collection<String> col = new ArrayList<>();
col.add("123");
```

首先创建ArrayList对象,ArratList的构造方法方法如下

``` java
 public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
 }
 
 private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
```
实际上就是创建一个空的数组

拿着新建的ArrayList对象，调用添加方法`list.add("aaaa");`