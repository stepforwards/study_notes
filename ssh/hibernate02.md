---
title: hibernate02
tags: ssh,hibernate,Java,框架
grammar_cjkRuby: true
---
> hibernate操作数据库的本质就是将瞬时态和托管态转换为持久态,我们今后操作数据库的本质也是需要将对象编程持久态,持久状态的特点是能够自动更新数据库
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

![hibernate状态转换示意图][2]
 
**瞬时态**
  转换成持久态：调用save方法，saveOrUpdate方法实现
  转换成脱管态：设置oid值 ，user.setId(10);

**持久态**
 转换成瞬时态：调用delete方法实现
 转换成脱管态：调用session里面的close方法实现

**脱管态**
  转换成瞬时态：设置oid值 ，user.setId(null)
  转换成持久态：调用update和saveOrUpdate方法实现


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504616997842.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504617225744.jpg