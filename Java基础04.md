---
title: Java基础04
tags: 方法,重载 
grammar_cjkRuby: true
---
[TOC]
## 方法的声明


> 方法的圣母那个位置必须写在类中,并且方法的内部与不能再去声明另一个方法，一个雷的方法与方法之间只能是并列关系，不能嵌套

- 方法声明的格式

``` stylus
修饰符 返回值类型 方法名(){
	程序代码
	
	return 返回值;
}
```

## 方法的调用
> 方法声明过后,不能自动执行,需要我们调用执行,因为程序的入口是main,当执行完main方法后程序就结束,所以方法的调用应该放在main方法中

- 方法调用的格式为**方法名(参数1,参数2...);**,调用的时候参数的值必须要和定义方法的时候参数的类型相同
- 在main方法中依次调用上述的4个方法
- 注意返回值的使用


## 方法的重载
> 如果我们需要计算两个数的和,我们计算两个整数,两个小数,一个整数一个小数,按照上述方法我们需要 定义三个求和方法,这就需要我们能够分别记住三个方法名称,这显然不是很方面.
> 
> 对于以上这种情况我们可以将这三个方法

``` stylus
	public static int add(int a , float b){
		return (int)(a + b);
	}
	public static int add(int a , int b){
		return a + b;
	}
	public static int add(int a , int b , int c){
		return a + b + c;
	}
	
	public static int add(int a , int b , int c , int d){
		return a + b + c + d;
	}
```
## 方法重载的目的
方法的重载,解决仅仅参数不同的时候,我们不用再去创建很多名称不一致的方法,方便我们使用方法.
## 方法的内存分析

``` stylus
public class PassParameter {

	// 方法中不能修改基本数据类型参数的值,但是引用类型
	public static void main(String[] args) {
		int a = 100;
		testMethod(a);
		System.out.println(a);
	}
	
	public static void testMethod(int a){
		a = 101;
	}
}
```
- 在main中定义的变量a的值是100
- 调用testMethod(a);此时在内存中testMethod压栈执行,并且将a的值赋值给参数
- 在testMethod中修改a的值.,此时的参数仅仅和main中定义的变量a的值相同,所以修改参数的值,并不影响a的值.


``` stylus
public static void main(String[] args) {
		int [] a = {1,2,3};
		testMethod(a);
		System.out.println(a[0]);
	}
	
	public static void testMethod(int [] arr){
		arr[0] = 101;
	}
```

- 在main中定义引用类型a,初始化值1,2,3
- 调用testMethod(a),将数组a的地址赋值给形参数组arr,此时形参arr和实参a指向同一个地址,testMethod压栈执行,修改arr[0] = 101的值,然后testMethod方法结束,testMethod出栈.
- main方法中的数组a的值已经被testMethod修改,所以数组a中的值被改变.a[0] = 101
- 如果让arr开辟一块新的空间,则不影响main方法中数组的值


## 面向对象

## 面向对象的三大特征
- 封装
	- 提高代码的复用性,便于调试者的使用,提高代码的安全性
- 继承
- 多态

## 类和对象
- 类
>对某一个有相同特征的食物的抽象,例如建造房子时设计的模型.
- 对象
> 表示现实中该类事物的个体,就是类的具体实现,例如已经建好的房子.
- 属性
> 该类事物的共有特征,实际就是我们之前学习的变量,只不过与之前学的变量有点区别
- 方法
> 该类共有的功能

- 声明类的格式

``` stylus
public class 类名{
	// 一个类可以有多个属性
	数据类型 变量名称;
	数据类型 变量名称;
	// 一个类可以有多个方法
	修饰符 返回值类型 方法名(参数列表){
		执行语句;
	}
}
```
- 创建对象的格式  **类名 对象名 = new 类名();**
- 调用成员变量格式 **对象名.成员变量**,如果在当前类中可以直接写成成员变量名称
-  调用方法的格式: **对象名.方法名(具体的参数)**,如果在当前类中,可以直接写方法名.



