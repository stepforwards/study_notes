---
title: Java基础07 
tags: 常用关键字,代码块,修饰符,常用类,集合
grammar_cjkRuby: true
---
[TOC]
## 常用关键字
### super关键字
> 子类对父类的引用,只能在非静态方法中使用

- 引用父类的成员变量的格式**super.变量名**
- 引用父类非静态方法格式**super.方法名(参数列表)**
- 引用父类的构造方法 **super(参数列表)**
### final
- final修饰类,不能被继承,但是不影响创建对象.如String
- final修饰方法,不能重写,但父类中没有被final修饰方法,子类覆盖后可以加final
- final修饰局部变量,智能体赋值一次;当修饰引用类型时,不能重新赋值,但是可以修改对象的属性
- final修饰成员变量时,必须在定义的时候进行赋值,不能自动初始化或者在构造方法中进行赋值.

### static
![Static内存分析][1]
- 被static修饰的成员变量属于类，不属于这个类的某个对象(多个对象共享一个static修饰的成员变量)
- 被static修饰的成员变量和成员方法建议直接通过类名进行访问,使用对象访问的时候会有警告
- 静态方法中只能使用静态成员变量,不能使用this/super
## 代码块
### 局部代码块
>定义在方法中,用户划分区域的

### 构造代码块
> 定义在类成员变量位置的代码块,每次创建对象的时候都会执行,优先于构造方法.

### 静态代码块
>定义在类的成员变量的用static修饰的代码块

- 它优先于主方法执行,优先于构造代码块执行,当以任意形式第一次使用该类时执行.
- 该类不管创建多少对象 ,静态代码块只能执行一次.
- 可用于给静态变量赋值,用来给类进行初始化.
## 访问修饰符

| --   |  public   |  protected   |  default   |  private   |
| :---: | :---: | :---: | :---: | :---: |
|  同一个类中   | Y    |  Y   |  Y   |   Y  |
| 同一个包中(子类和无关类)    | Y    |   Y  |   Y  |     |
| 不同包中的子类    |   Y  |    Y |     |     |
| 同一个包中无关类    |   Y  |     |     |     |
> 注意对于protected不同包中的子类只能在子类内部进行调用,在外部子类无法调用成员变量和方法.
## 系统常用类介绍

### String
> 属于不可变字符串,是字符串常量

### equal方法
- 比较两个对象的内容是否相同,==是比较两个对象的内存地址是否相同
- 常量都是存在jvm,方法区的常量池当中

``` stylus
	String str1 = "123";
	String str2 = "123";
	System.out.println(str1.equals(str2));//true
	System.out.println(str1 == str2));//true
	String str3 = "123";
	String str4 = "123";
	System.out.println(str3.equals(str4));//true
	System.out.println(str3 == str4));//false
```
### 构造方法

``` stylus
	String s6 = new String(“abc”); //创建String对象，字符串内容为abc
	byte[] bys = new byte[]{97,98,99,100};// 创建String对象，把数组元素作为字符串的内容
	String s2 = new String(bys);//创建String对象，把一部分数组元素作为字符串的内容，参数offset为数组元素的起始索引位置，参数length为要几个元素
	String s3 = new String(bys, 1, 3);
	System.out.println(s2);//abcd
	System.out.println(s3);//bcd//解决请求乱码时需要用到,在此先做了解
	String s4 = new String(s3.getBytes("ISO8859-1"), "UTF-8");
	char[] chs = new char[]{'a','b','c','d','e'};
	String s5 = new String(chs); //创建String对象，把数组元素作为字符串的内容
	String s6 = new String(chs, 0, 3);//创建String对象，把一部分数组元素作为字符串的内容，参数offset为数组元素的起始索引置，参数count为要几个元素
	System.out.println(s5);//abcd
	System.out.println(s6);//abc
```
### 获取字符串长度

``` stylus
	String str = "abcde";
	int len = str.length();
	System.out.println(len);//5
```
## 截取字符串

``` stylus
	String str = "abcde";
	String s1 = str.substring(1); //返回一个新字符串，为指定索引后的内容
	String s2 = str.substring(2, 4);//返回一个新字符串，内容为指定位置开始到指定位置结束所有字符[2,4)
	System.out.println(s1);//bcde
	System.out.println(s2);//cd
```
### 是否以字符开头和结尾

``` stylus
	String str = "zhiyou";
	boolean b2 = str.startsWith("zhi");
	boolean b3 = str.endsWith("you");
```
### 索引和包含

``` stylus
	String str = "abcde";//返回指定字符串的索引值,不包含则返回-1
	int index = str.indexOf(“bcd”);//判断是否包含指定字符串，包含返回true，不包含返回false
	boolean b2 = str.contains("bcd");
```
### 转换字符数组和字节数组

``` stylus
	String str = "abcde";
	char[] chs = str.toCharArray();
	byte[] bytes = str.getBytes();
```
### 分割字符串

``` stylus
	String s5 = "1-2-3-4-5";//{"1","2","3","4","5"}
	System.out.println(s5.split("-").length);
	String s6 = "www.zhiyou.com";//{"www","zhiyou","com"}
	//注意使用.进行分割时候需要使用转义字符,使用\\.
	System.out.println(s6.split("\\.").length);
```
### 转换大小写字符串

``` stylus
	String str = "hElLo";
	String big = str.toUpperCase();//HELLO
	String small = str.toLowerCase();//hello
```
### 将字符串指定索引转换成char

``` stylus
	char ch = str.charAt(i);
```
### String的API

``` stylus
boolean equals(Object obj) 判断两个字符串中的内容是否相同
boolean equalsIgnoreCase(String str) 判断两个字符串中的内容是否相同, 忽略大小写
boolean contains(String str) 判断该字符串中是否包含给定的字符串
boolean startsWith(String str) 判断该字符串是否以给定的字符串开头
boolean endsWith(String str) 判断该字符串是否以给定的字符串结尾
boolean isEmpty() 判断该字符串的内容是否为空的字符串 ""
int length() 获取该字符串的长度
char charAt(int index) 获取该字符串中指定位置上的字符
String substring(int start) 从指定位置开始，到末尾结束，截取该字符串，返回新字符串
String substring(int start,int end) 从指定位置开始，到指定位置结束，截取该字符串，返回新字符串
int indexOf(int ch ) 获取给定的字符，在该字符串中第一次出现的位置
int indexOf(String str) 获取给定的字符串，在该字符串中第一次出现的位置
int indexOf(int ch,int fromIndex) 从指定位置开始，获取给定的字符，在该字符
byte[] getBytes() 把该字符串 转换成 字节数组
char[] toCharArray() 把该字符串 转换成 字符数组String replace(char old,char new) 在该字符串中，将给定的旧字符，用新字符替换
String replace(String old,String new) 在该字符串中， 将给定的旧字符串，用新字符串替换
String trim() 去除字符串两端空格，中间的不会去除，返回一个新字符串
String toLowerCase() 把该字符串转换成 小写字符串
String toUpperCase() 把该字符串转换成 大写字符串
int indexOf(String str,int fromIndex) 从指定位置开始，获取给定的字符串，在该字符串中第一次出现的位值
```
### StringBuffer
> StringBuffer叫做字符串缓冲区,是一个容器,里面存放了很多字符串.

``` stylus
	StringBuffer sb = new StringBuffer();
	sb.append("haha"); //添加字符串
	sb.insert(2, "huhu");//索引为2的地方,插入字符串
	sb.delete(1, 4);//删除索引为[1,4)
	sb.replace(1, 4, "heihei");//替换范围[1,4)的内容
```
### StringBuilder
> 此类提供一个与 StringBuffer 兼容的 API，但是属于线程不安全的,所以效率较高,推荐使用

## List接口
> 可以看做是一个容器,只能存储引用,特点:有序,带索引,内容可以重复
### ArrayList
> ArrayList是List接口的一个实现类是一个容器,容器存放的是引用类型,他能够实现动态扩容,内部实现是依靠数组实现的,初始化容量是10,当要超出容量的时候,扩容变成原来的1.5倍创建的格式一般使用多态的形式 **List<String> li= new ArrayList<String>();**

``` stylus
	List<String> list = new ArrayList<String>();//添加
	list.add("李雷");
	list.add("韩梅梅");
	//插入 插入前["李雷","韩梅梅"]
	list.add(1, "隔壁老王"); //插入元素后的集合["李雷","隔壁老王","韩梅梅"]
	//删除
	list.remove(2);// 删除元素后的集合["李雷","隔壁老王"]
	//修改
	list.set(1, "隔壁老张");// 修改元素后的集合["李雷","隔壁老张","韩梅梅"]
	//通过索引获取
	String str = list.get(0);//获取李雷
```

### 遍历(和数组相同)

``` stylus
	for(int i = 0; i < li.size(); i++){
		System.out.println(li.get(i));
	}
	for(String str : li){
		System.out.println(str);
	}
```


## Map接口
>Map是集合容器,存放的元素由键和值两部分组成,通过键可以找到对应的值,键和值必须是引用类型,键唯一不能重复,没有顺序
>
![Map内存分析][2]
### HashMap
> HashMap是Map的实现类
``` stylus
	//创建Map对象
	Map<String, String> map = new HashMap<String,String>();//给map中添加元素
	map.put("1", "Monday");
	map.put("7", "Sunday");
	System.out.println(map); //
	//当给Map中添加元素，会返回key对应的原来的value值，若key没有对应的值，返回null
	System.out.println(map.put("1", "Mon")); // Monday
	System.out.println(map); // {星期日=Sunday, 星期一=Mon}
	//根据指定的key获取对应的value
	String en = map.get("1");
	System.out.println(en); // Sunday
	//根据key删除元素,会返回key对应的value值
	String value = map.remove("1");
	System.out.println(value); // Sunday
	System.out.println(map); // {1=Mon}
```
### map的遍历

``` stylus
	Map<String, String> map = new HashMap<String,String>();
	//给map中添加元素
	map.put("邓超", "孙俪");
	map.put("李晨", "范冰冰");
	map.put("刘德华", "柳岩");
	//获取Map中的所有key
	Set<String> keySet = map.keySet();
	for(String key : keySet){
		System.out.println(map.get("key"));
	}
```
## Date
### 构造函数

``` stylus
	Date date1 = new Date();
	System.out.println(date1);//当前日期
	Date date2 = new Date(12354356000000L);
	System.out.println(date2);//指定日期
```
### 日期和毫秒的转换

``` stylus
	Date date = new Date();
	long time = date.getTime();
	System.out.println(time);//1499169262875
```
### DateFormat
> 格式化日期类,通常用于日期和String的转换

|  标志   |  含义   |
| :---: | :---: |
|  G   |  年代标识符   |
|   y  |   年  |
|   M  |  月   |
|   d  |   日  |
|   h  | 时 在上午或下午(1-12)    |
|   H  | 时 在一天中(0-23)    |
|   m  |  分   |
|   s  |  秒   |
|   S  |  毫秒   |
|  E   |  星期   |
|  D   |  一年中的第几天  |
|   F  |  一月中第几个星期几   |
|  w   |  一年中第几个星期几   |
|  W   |  一个月中第几个星期   |
|  a   |  上午/下午 标记符   |
|   k  |  时 在一天中(1-24)   |
|   K  |  时 在上午或下午(0-11)  |
|   z  |  时区   |

``` stylus
	Date date2 = new Date();//DateFormat抽象类,SimpleDateFormat是DateFormat实现类
	DateFormat df = new SimpleDateFormat("yyyy-MM-dd E HH:mm:ss a");
	String str = df.format(date2);
	System.out.println(str);//2017-07-04 星期二 20:04:43 下午
```
### 日期字符串之间的抓换

``` stylus
	Date date = new Date(12354356000000L);
	DateFormat df = new SimpleDateFormat(“yyyy年MM月dd日”);
	String str = df.format(date);
	String str = ”2020年12月11日”;
	DateFormat df = new SimpleDateFormat(“yyyy年MM月dd日”);
	Date date = df.parse(str);
```

## Calendar

### 创建对象

``` stylus
Calendar c = Canlendar.getInstance();
```
## 获取年月日等信息

``` stylus
Calendar c = Calendar.getInstance();
int year = c.get(Calendar.YEAR);
int month = c.get(Calendar.MONTH);//从0开始算起，最大11；0代表1月，11代表12月
int date = c.get(Calendar.DATE);
int hour = c.get(Calendar.HOUR);
int min = c.get(Calendar.MINUTE);
int sec = c.get(Calendar.SECOND);//2017--6--4--8--17--43
System.out.println(year+"--"+month+"--"+date+"--"+hour+"--"+min+"--"+sec);
```


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499770593364.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499771798511.jpg