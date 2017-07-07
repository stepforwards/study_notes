---
title: Java基础03
tags: 方法 
grammar_cjkRuby: true
---
[TOC]

## for循环作业

### 格式化输入书的章节

``` stylus
for (int i = 1; i < 6; i++) {
	if (i == 5) {
		break;
	}
	for (int j = 1; j < 5 + i; j++) {
		if (j == 3) {
			continue;
		}
		System.out.println("第" + i + "章,第" + j + "节");

	}
}
```

### 打印乘法表

``` stylus
for (int i = 1; i < 10; i++) {
	for (int j = 1; j < i+1; j++) {
		System.out.print(j + "*" + i + "=" + i*j + "\t");
	}
	System.out.println();
}
```


### 打印水仙花数

``` stylus
		int bai = 0;
		int shi = 0;
		int ge = 0;
		for (int i = 100; i < 1000; i++) {
			bai = i / 100;
			shi = i / 10 %10;
			ge = i % 10;
			if(bai*bai*bai + shi*shi*shi + ge*ge*ge == i){
				System.out.println(i + " 是水仙花数");
			}
			
		}
```


### 打印大小写字母

``` stylus
private static void method2() {
	char litterLetter = 'a';
	char bigLetter = 'A';
	for (int i = 0; i < 26; i++) {
		System.out.println(litterLetter + "---" + bigLetter);
		litterLetter++;
		bigLetter++;
	}
}
private static void method1() {
	int shortNum = 65;
	int bigNum = 97;
	for (int i = 0; i < 26; i++) {
	char shortLetter = (char) shortNum;
	char bigLetter = (char) bigNum;
	System.out.println(shortLetter + "------" + bigLetter);
	shortNum++;
	bigNum++;
	}
}
```


### 求1-100的和

``` stylus
	int sum = 0;
	for (int i = 0; i <= 100; i++) {
		sum +=i;
	}
	System.out.println(sum);
```

### 猜数字游戏

``` stylus
	Random random = new Random();
	int number = random	.nextInt(100) + 1;

	System.out.println("请输入1-100之间的数");
	while(true){
		Scanner scanner = new Scanner(System.in);
		int nextInt = scanner.nextInt();
		if(nextInt==number){
			System.out.println("你真棒");
			break;
		}
		System.out.println(nextInt > number ?  "猜大了":"猜小了");
	}
```

## 数组

###  数组的三种定义方式

``` stylus
	- String [] str = new String[4];
	- String [] str1 = new String[]{"你","我","她"};
	- int [] str2 = {1,2,3,4,5,6}
```
### 数组的默认方式

|  类型   |  初始化的值   |
| --- | --- |
|  byte ，short ，int， long  |  0   |
|  float，double   |   0.0  |
|   char  |  空格   |
|  boolean   |  false   |
|   引用类型  |   null  |


### 取数组中的数据 
	- str[1];  // 取str数组中的1号索引处的值
### 遍历数组
``` stylus
// for循环遍历数组
for (int i = 0; i < intArr.length; i++) {
	System.out.println(intArr[i]);
	}
//  for each,无法控制循环次数,并且没有索引
// 依次从数组中获取内容提供,赋值给element
for (int i : intArr) {
System.out.println(i);
}
```
### JVM构造
> 1.程序计数器: 记录CPU该去执行内存中的那条指令

> 2.本地方法栈: Jvm调用操作系统的方法区域


> 3.方法栈:执行方法,保存局部变量


> 4.方法区(包含静态区):存储了每个类的信息(包括类的名称,方法信息,字段信息),静态变量,常量,方法以及编译器编译后的代码等


> 5.堆:用来存储对象本身以及数组

### 一维数组内存原理

![数组内存分析图][1]

* 1.ArrayDemo.class先进入JVM的方法区
* 2. 代码执行main方法,main方法复制压栈开始执行
* 3. 遇到创建数组的代码,在堆中创建一块内存空间,开辟4个空间,给每个变量有一个默认值0
* 4. 将数组的首地址给栈中的arr变量,所以arr变量就有了对堆得引用
* 5.代码执行到输出语句,就会输出arr所引用的堆中的内存地址
> **注意:** 对引用类型,只要 new一次,就代表在堆中开辟了一块内存空间.

### 二维数组
- 定义初始化

``` stylus
		// 第一种
		int [][] arr = new int[2][3];
		arr[0][0] = 1;
		arr[0][2] = 2;
		arr[0][2] = 3;
		
		// 第二种
		// 大数组中又三个数组,小数组的中元素不确定
		int [][] arr2 = new int[3][];
		arr2[0] = new int[]{1,2,3};
		// 不能这样写
		//arr2[0] = {1,2,3};
		arr2[1] = new int[]{4,5};
		arr2[2] = new int[]{6};
		
		//第三种
		int [][] arr3 = {{1,2,3},{4,5},{6}};
```

- 二维数组遍历

- 二维数组声明 类型 [][] 数组名；
- 二维数组的赋值 数组名 = new 类型[长度][长度]
- 一般建议写成  类型 [][] 数组名 = new 类型[长度][ 长度]
- 赋值

``` stylus
 int [] arr = new int [4];
 arr[0] = 100;
 arr[1] = 200;
 arr[2] = 300;
 arr[3] = 400;
```

- 长度是  **二维数组名.length;**
- 二维数组中一维数组的长度:二维数组名[索引].length;
- 二维数组不定长度的个数

``` stylus
int [][] arr = new int[3][];
arr[0] = new int [] {1,2,3};
arr[1] = new int [] {4,5};
arr[2] = new int [] {6};
```
- 简写形式 **int [ ] [ ] arr = {{1,2,3},{4,5},{6}};**

### 一维数组和二维数组的内存关系

 1.class文件先进入方法区
 2.main 压栈执行
 3.在堆中创建两个数组，并初始化内容为0，各自的地址为数组中首元素的地址
 4.在堆中创建一个数组，数组的长度为2，数组中有两个对之前数组的引用
 5.让arr引用二维数组的地址

![ 二维数组内存分析图][3]

## 作业：
1. 找出数组的最大值

``` stylus
		int [] arr = {1,2,567,786,43,65,34};
		int MAX = arr[0];
		for (int i = 1; i < arr.length; i++) {
			if(MAX < arr[i]){
				MAX = arr[i];
			}
		}
		System.out.println(MAX);
```
2.一维数组累加求和
``` stylus
		int [] arr = {1,2,567,786,43,65,34};
		int sum = 0;
		for (int i = 0; i < arr.length; i++) {
			sum += arr[i];
		}
		System.out.println(sum);
```
3.数组倒序交换位置

``` stylus
	int [] arr = {1,2,567,786,43,65,34};
	for(int beginIndex = 0, endIndex = arr.length - 1;beginIndex < endIndex;beginIndex++,endIndex--)
	{
		arr[beginIndex] = arr[beginIndex] ^ arr[endIndex];
		arr[endIndex] = arr[beginIndex] ^ arr[endIndex];
		arr[beginIndex] = arr[beginIndex] ^ arr[endIndex];
	}
	for (int i : arr) {
		System.out.print(i + "\t");
	}
```
4.数组的元素排序
- 4.1 选择排序
``` stylus

		int [] arr = {1,2,567,786,43,65,34};
		for (int i = 0; i < arr.length - 1; i++) {
			for (int j = i + 1; j < arr.length; j++) {
				if(arr[i] > arr[j]){
					arr[i] = arr[i] + arr[j] - (arr[j] = arr[i]);
				}
			}
		}
		for (int i : arr) {
			System.out.print(i + "\t");
		}
```

- 4.2 冒泡排序


``` stylus
		int [] arr = {1,2,567,786,43,65,34};
		for (int i = 0; i < arr.length -1; i++) {
			for (int j = 0; j < arr.length - i -1; j++) {
				if(arr[j] > arr[j + 1]){
					arr[j] = arr[j] + arr[j + 1] - (arr[j + 1] = arr[j]);
				}
			}
		}
		for (int i : arr) {
			System.out.print(i + "\t");
		}
```



  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499238453222.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499238453222.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499302758743.jpg