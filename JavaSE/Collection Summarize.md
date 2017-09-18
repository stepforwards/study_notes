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

拿着新建的ArrayList对象，调用添加方法`list.add("aaaa");`添加方法接收一个泛型，返回一个boolean的值，首先调用ensureCapacityInternal方法

``` java

private int size;// 默认是0
public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
```


1. 调用ensureCapacityInternal

``` java
private void ensureCapacityInternal(int minCapacity) { // minCapacit为1
	if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
		minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity); // DEFAULT_CAPACITY变量，默认为10
	}
	ensureExplicitCapacity(minCapacity); // 调用ensureExplicitCapacity方法
}
```

2. 调用ensureExplicitCapacity方法

``` java
protected transient int modCount = 0;

private void ensureExplicitCapacity(int minCapacity) {
        modCount++;
        // 判断是否溢出
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
```
3. 再调用grow方法,进行数据的copy

``` java

private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;// 数组的最大容量
// MAX_VALUE int的最大范围
private void grow(int minCapacity) {
	// overflow-conscious code
	int oldCapacity = elementData.length;
	int newCapacity = oldCapacity + (oldCapacity >> 1);
	if (newCapacity - minCapacity < 0)
		newCapacity = minCapacity;
	if (newCapacity - MAX_ARRAY_SIZE > 0)
		newCapacity = hugeCapacity(minCapacity);
	// minCapacity is usually close to size, so this is a win:
	elementData = Arrays.copyOf(elementData, newCapacity);
}
```
指定数据的索引值，size+1，将指定的元素添加到列表的尾部。到这数据的添加就完成了，整个过程很繁琐，进行了数组的copy，及其耗费性能。

![enter description here][1]

# LinkedList集合的数据结构是双向链表，方便添加、删除操作
LinkedList的添加操作比较简单，调用下列方法即可，具体实现流程如下图
``` java
// LinkedList
public boolean add(E e) {
        linkLast(e);
        return true;
    }
	
void linkLast(E e) {
        final Node<E> l = last;
        final Node<E> newNode = new Node<>(l, e, null);
        last = newNode;
        if (l == null)
            first = newNode;
        else
            l.next = newNode;
        size++;
        modCount++;
    }
```
![enter description here][2]

![enter description here][3]

# Set接口

> Set接口的特点
1.不包含重复元素的集合
2.set集合中是没有索引的
set接口的方法和Collection中的方法完全相同

## HashSet

> 此类实现Set接口,底层的数据结构hash表,此类的特点有
1.无序的
2.不重复
3.底层是HashMap

> HashSet的底层实现是HashMap

``` java
public HashSet() {
		// 创建HashSet实际上是创建了HashMap
        map = new HashMap<>();
}
```
> HashSet添加数据，HashSet利用Map键的唯一性，确保在Hashset中添加的元素唯一不重复，值是一个Object对象

``` java
// PRESENT是一个Object对象
private static final Object PRESENT = new Object();
public boolean add(E e) {
        return map.put(e, PRESENT)==null;
}
```

## HashSet存储自定义对象
1. 重写hashcode方法
2. 重写equals方法

注意 ： 重写hashcode和equals方法，eclipse使用快捷键 `Alt+shift+s，h`

## 哈希表

> 哈希表底层使用的也是数组机制，数组中也存放对象，而这些对象往数组中存放时的位置比较特殊，当需要把这些对象给数组中存放时，那么会根据这些对象的特有数据结合相应的算法，计算出这个对象在数组中的位置，然后把这个对象存放在数组中。而这样的数组就称为哈希数组，即就是哈希表。
特点:
1.查询增删都比较快
2.初始容量是16,加载因子是0.75,当长度达到
16*0.75=12的时候进行数组的扩容,一般不建议修改这
个值



  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505737374188.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505737320906.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505738235303.jpg