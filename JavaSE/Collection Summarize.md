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



# LinkedList集合的数据结构是双向链表，方便添加、删除操作




