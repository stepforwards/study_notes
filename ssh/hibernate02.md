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
![hibernate状态转换示意图][1]
 
**瞬时态**
  转换成持久态：调用save方法，saveOrUpdate方法实现
  转换成脱管态：设置oid值 ，user.setId(10);

**持久态**
 转换成瞬时态：调用delete方法实现
 转换成脱管态：调用session里面的close方法实现

**脱管态**
  转换成瞬时态：设置oid值 ，user.setId(null)
  转换成持久态：调用update和saveOrUpdate方法实现

# Hibernate的一级缓存
## 什么是缓存
> 把数据不放到文件系统中，放到系统的内存中，可以直接从内存中获取数据，提高获取数据的效率。

## Hibernate的缓存
- **Hibernate的一级缓存**
  在hibernate中的一级缓存默认就是打开的，一级缓存使用范围是session范围的。从session创建，到session关闭的过程，如果session关闭了，一级缓存内容没有了。

- **Hibernate的二级缓存**
  在hibernate中的二级缓存默认不是打开的，手动设置才可以使用。二级缓存使用范围是sessionFactory范围的二级缓存。

## 验证一级缓存存在
> 查询三次，只打印出一条查询语句，并且三个对象指向的是同一个地址
``` java
 @Test
public void test03(){
	Session session = HibernateUtil.getSession();
	Transaction tx = session.beginTransaction();

	User user = session.get(User.class, 3);
	User user1 = session.get(User.class, 3);
	User user2 = session.get(User.class, 3);

	System.out.println(user == user1);
	System.out.println(user1 == user2);

	tx.commit();
	session.close();
}
```

![验证一级缓存执行结果][2]

## 一级缓存的快照区（副本）
> 1.首先根据id查询用户数据，返回user是持久态对象。
2. 首先 把返回的持久态user对象放到一级缓存中。另外 把user对象复制一份，再放到一级缓存对应的快照区
设置了持久态对象里面的值时候 （修改了user里面的值），执行之后首先 同步更新一级缓存中对应的内容
其次 但是不会更新对应的快照区的内容
3.提交事务之后，实现比较一级缓存和快照区的内容是否相同，如果不相同，把一级缓存中的内容更新到数据库里面。
一级缓存使用java的集合存储，使用map集合


``` java
@Test
public void test03(){
	Session session = HibernateUtil.getSession();
	Transaction tx = session.beginTransaction();
	User user = session.get(User.class, 3);
	user.setName("张三丰");
	session.update(user);
	tx.commit();
	session.close();
}
```


![一级缓存的快照区执行结果][3]

# 查询


## HQL查询
> Hibernate Query Language 面向对象的查询,在查询语句中是没有任何数据库相关内容,只会出现类名和属性名

- 如果查询的对象的所有属性的时候时候省略select *
- 表名改为完整类名,如果工程中没有重名的类,可以省略掉包名
- 对于唯一的结果调用 uniqueResult() 方法
- 对于结果是集合类型调用调用 list() 方法
- 对于结果是对象类型调用调用 uniqueResult() 方法
- 对于绑定 ? 一般使用 setParameter(索引,Object) ,索引从0开始

- 查找所有

``` java
String hql = "from User";
Query<User> query = session.createQuery(hql);
List<User> list = query.list();
System.out.println(list);
```

- 查询指定id的

``` java
User uu = (User)session.createQuery("fromUser where id = ?").setParameter(0, 9).uniqueResult();
System.out.println(uu);
```


- 命名占位符

``` java
User uu = (User)session.createQuery("fromUser where id = :userid").setParameter("userid",9).uniqueResult();
```

- 分页查询

``` java
List<User> list = (List<User>)session.createQuery("from User").setFirstResult(0).setMaxResults(5).list();
```

## QBC查询





  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504617225744.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504618380053.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1504618986931.jpg