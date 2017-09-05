---
title: hibernate03
tags: ssh,hibernate,Java,框架
grammar_cjkRuby: true
---

# hibernate多表之间的关系

> 在数据库中表间关系有3种类型,一对多,多对多,一对一,对于一对一的表我们可以使用一张表进行完成,所以在此讨论一对多和多对多的表间关系

## 一对多(多对一)

1. 创建实体
- 创建实体类在一的一方添加set或者list集合,推荐使用set,因为使用的时候需要在集合中添加元素,所以在声明的时候直接进行初始化操作

``` java
public class Speaker {
    private Integer id;
    private String name;
    private Set<Video> sperakerSet = new HashSet<>();
	setter/getter...
}
```


- 在多的一方添加一的一方的类类型
- 
