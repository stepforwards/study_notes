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

- 配置多的一方的ORM映射文件
	- 多对一使用 <many-to-one>
	- 属性name表示属性
	- 属性class表示一的一方的类型
	- 属性column表示自己的外键的名称

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
<class name="Video" table="t_video">
    <!--name：实体类名称 column: 数据库字段名，如果设置实体类名称和字段名相同，可省略-->
    <!--主键必须使用id标签-->
    <id name="id">
        <!--主键生成策略，现在先选择native，后面细说-->
        <generator class="native"></generator>
    </id>
    <property name="name"></property>
    <!--
        column：外键
        name: 多的一方的属性值
        class：一的一方的类的全路径
    -->
    <many-to-one name="speaker" class="Speaker" column="svid"></many-to-one>
</class>

</hibernate-mapping>

```

## 创建测试文件

> 需要对双方建立维护关系,并且要改变双方的状态都要是持久化状态才能写入数据库

``` java
@Test
public void test01(){
	Session session = HibernateUtil.getSession();
	Transaction tx = session.beginTransaction();
	// 创建讲师对象
	Speaker s = new Speaker();
	s.setName("张三");
	// 创建视频对象
	Video v1 = new Video();
	v1.setName("video1");
	// 创建视频对象
	Video v2 = new Video();
	v2.setName("video2");
	// 建立 speaker -- video 关系
	s.getSperakerSet().add(v1);
	s.getSperakerSet().add(v2);
	// 建立 video -- speaker 关系
	v1.setSpeaker(s);
	v2.setSpeaker(s);
	// 保存数据
	session.save(s);
	session.save(v1);
	session.save(v2);
	tx.commit();
	session.close();
}
```
- 在生成的数据库文件中,hibernate会添加外键约束

![外键约束][1]

# 级联操作

> 级联操作的目的是为了在操作的时候简化操作，实现数据库之间跨表操作

# inverse cascade 外键的维护之间的关系

级联操作外键之间关系的维护是双向的，两边都进行维护，因此就引起了代码的冗余。为了解决这个问题，我们引出了inverse来控制关系双方的维护关系，来减少代码的冗余操作。
inverse：true，表示放弃维护的权利，默认是false，维护外键之间的关系
cascade：是强调在操作一个对象的时候级联操作另一个对象




  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504621209879.jpg