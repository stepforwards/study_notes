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


# 级联操作

> 对于上述操作,我们想要将数据存入到数据库中,需要对所有的数据都要进行由瞬时态到持久态的转变,如果不做这些操作的话,数据是无法被写入数据库中的,如果数据多的时候操作就会相当复杂级联操作就是可以在多的一方或者一的一方设置,那么将对其中一方进行操作的时候,另一方也会受到影响通过级联操作可以对 cascade 属性设置3个值分别为
- 1. save-update 保存修改的时候级联操作
-  2. delete 删除的时候级联操作,当删除一方数据的时候,另一方也会被删除
- 3. all 等同于 save-update,delete


## 对speaker进行级联操作(save-update)

> 如果对speaker进行级联设置,那么对于其中的video对象就不用再对他们进行保存,只需保存speaker就可以了

![enter description here][2]


## 对speaker进行级联操作(delete)

> 如果对speaker进行了级联删除设置,那么只要删除speaker对象,video中所有的是这个外键的video对象都会被删除

![enter description here][3]

## 对video进行级联操作(delete)

> 如果对某一个video进行了级联删除设置,那么只要删除video对象,speaker对象就会被删除,其他的video对象如果外键是这个speaker对象,就会变为null

# 双方关系维护特征

> 当我们对刚才的数据进行如下设置,就是为了,建立对象双方的关系维护

![enter description here][4]

![enter description here][5]

运行结果为

![enter description here][6]

- 当保存video1的时候video1会写入数据库,因为video和speaker有级联操作,所以speaker会存入数据库,speaker和video有级联操作,所以video2会写入数据库

``` java
@Test
public void test05(){
	Session session = HibernateUtil.openSession();
	Transaction transaction = session.beginTransaction();
	Video video1 = new Video();
	video1.setVideoTitle("video1");
	
	Video video2 = new Video();
	video2.setVideoTitle("video2");
	
	Video video3 = new Video();
	video3.setVideoTitle("video3");
	
	Speaker speaker = new Speaker();
	speaker.setSpeakerName("李彦伯");
	
	video1.setSpeaker(speaker);
	speaker.getVideoList().add(video2);
	speaker.getVideoList().add(video3);
	//session.save(video1);//会存入video1
	video2 video3 speaker
	//session.save(speaker);//会存入speake
	r video2 video3
	session.save(video3);//会存入video3
	transaction.commit();
	session.close();
}
```

# 外键的维护权管理

## 冗余sql的产生

> 通过如下代码

![enter description here][7]

> 当我们去运行的过程中会发现其结果为

![enter description here][8]

因为对于同一个外键,video会进行维护,speaker也会进行维护,所以对于外键的操作会进行两次

## 放弃外键的维护

![enter description here][9]

设置放弃对外键进行维护

![enter description here][10]

## inverse和cascade区别

> inverse是对外键的维护权
> cascade是强调在操作一个对象的时候级联操作另一个对象

## inverse总结
如果在speaker中设置了inverser,那么使用speaker操作video的时候,如果进行删除和修改的时候是没有效果的

![enter description here][11]

# 多对多
##创建实体
- 创建teacher实体,在其中创建Student类型的set集合

``` java
private Integer id;
private String name;
private Set<Student> students = new HashSet<> ();
setter/getter...
```
- 创建Student实体,在其中创建Teacher类型的set集合

``` java
private Integer id;
private String name;
private Set<Teacher> teachers = new HashSet<>();
setter/getter...
```

# 创建ORM映射文件

- 创建teacher映射文件
	- name属性是指teacher中的属性名
	-  因为是多对多,所以需要生成第三张表,table属性是指定第三张表的名称
	- key column 指的是teacher在第三张表中的外键名称 many-to-many 表示多对多,只另一放的设置内容
		- class 表示另一方的类名
		- column 表示另一方在第三张表中的名称

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<!--
    配置package以后能够在当前文件下省去路径，但是不包含子包
-->
<hibernate-mapping package="top.xiesen.hibernate.model">
<class name="Teacher" table="t_teacher">
    <id name="id">
        <generator class="native"></generator>
    </id>
    <property name="name"></property>

    <set name="setStudent" table="s_t" cascade="save-update,delete">
        <key column="tid"></key>
        <many-to-many class="Student" column="sid"></many-to-many>
    </set>
</class>
</hibernate-mapping>
```


- 创建student映射文件

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<!--
    配置package以后能够在当前文件下省去路径，但是不包含子包
-->
<hibernate-mapping package="top.xiesen.hibernate.model">
<class name="Student" table="t_student">
    <id name="id">
        <generator class="native"></generator>
    </id>
    <property name="name"></property>

    <set name="setTeacher" table="s_t" inverse="true">
        <key column="sid"></key>
        <many-to-many class="Teacher" column="tid"></many-to-many>
    </set>

</class>
</hibernate-mapping>
```

## 创建测试文件

``` java
public void test01(){

	Teacher t1 = new Teacher();
	t1.setName("张老师");

	Teacher t2 = new Teacher();
	t2.setName("李老师");

	Student s1 = new Student();
	s1.setName("张三");
	Student s2 = new Student();
	s2.setName("李四");

	t1.getSetStudent().add(s1);
	t1.getSetStudent().add(s2);
	t2.getSetStudent().add(s1);
	t2.getSetStudent().add(s2);

	s1.getSetTeacher().add(t1);
	s1.getSetTeacher().add(t2);
	s2.getSetTeacher().add(t1);
	s2.getSetTeacher().add(t2);
	session.save(t1);
	session.save(t2);
	/*session.save(s1);
	session.save(s2);*/
}

```



  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504621209879.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504794161626.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504794231286.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504794291497.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504794304976.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504794336625.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504795016474.jpg
  [8]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504795056851.jpg
  [9]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504795092754.jpg
  [10]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504795179592.jpg
  [11]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504795282880.jpg