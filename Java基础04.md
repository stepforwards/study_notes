---
title: Java基础04
tags: 方法 
grammar_cjkRuby: true
---

### 方法的声明


> 方法的圣母那个位置必须写在类中,并且方法的内部与不能再去声明另一个方法，一个雷的方法与方法之间只能是并列关系，不能嵌套

- 方法声明的格式

``` stylus
修饰符 返回值类型 方法名(){
	程序代码
	
	return 返回值;
}
```

### 方法的调用
> 方法声明过后,不能自动执行,需要我们调用执行,因为程序的入口是main,当执行完main方法后程序就结束,所以方法的调用应该放在main方法中

- 方法调用的格式为**方法名(参数1,参数2...);**,调用的时候参数的值必须要和定义方法的时候参数的类型相同
- 在main方法中依次调用上述的4个方法
- 注意返回值的使用


### 方法的重载
> 如果我们需要计算两个数的和,我们计算两个整数,两个小数,一个整数一个小数,按照上述方法我们需要 定义三个求和方法,这就需要我们能够分别记住三个方法名称,这显然不是很方面.
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


