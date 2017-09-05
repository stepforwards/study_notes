---
title: hibernate02
tags: ssh,hibernate,Java,框架
grammar_cjkRuby: true
---

# 对象的三种状态
> 持久化类有三种状态，区分不同的状态，使用两种操作区分：
  第一个 判断对象中是否有oid
  第二个 判断对象是否与session进行关联
  
1. 瞬时态
 没有oid，没有与session关联

``` java
Video v1 = new Video();
v1.setName("video1");
```
 
 2. 持久态
 有oid，与session关联

``` java
Speaker speaker = session.get(Speaker.class, 1);
```

3. 脱管态
 有oid值，没有与session进行关联

``` java
Video v1 = new Video();
v1.setId(2);
v1.setName("video1");
```


