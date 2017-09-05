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

``` java
public class Video {
    private Integer id;
    private String name;
    private Speaker speaker;
	setter/getter...
}
```

## 配置ORM映射文件

> hibernate对于外键的维护是双向的，因此，设置对象关系映射时，我们需要将两边的外键的值设置成一样的。

- 配置一的一方的ORM映射,注意如果使用List类型,需要多添加一个标签index用来表示在多的一方中在list中的索引
- name表示一的一方的属性名称集合自标签 <key column="speakerId">
- </key> 表示在多的一方中外键
- 一对多标签表示集合中对应的元素的类型<one-to-many class="Video"/>如果使用的是list需要添加 <index
column="colIndex" type="java.lang.Integer"></index>
- column表示要在多的一方生成的字段的名称,type表示类型,索引的类型为整型


``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<!--
    配置package以后能够在当前文件下省去路径，但是不包含子包
-->
<hibernate-mapping package="top.xiesen.hibernate.model">
<!--
    name:表示model实体类名称
    table：表名，如果表名和实体类名称相同，可省略table属性
-->
<class name="Speaker" table="t_speaker">
    <!--name：实体类名称 column: 数据库字段名，如果设置实体类名称和字段名相同，可省略-->
    <!--主键必须使用id标签-->
    <id name="id">
        <generator class="native"></generator>
    </id>
    <!--其他字段使用property标签-->
    <property name="name"></property>

    <set name="sperakerSet" cascade="delete-orphan">
        <!--key：表示多的一方的外键-->
        <key column="svid"></key>
        <one-to-many class="Video"></one-to-many>
    </set>
</class>

</hibernate-mapping>
```
