[TOC]
## Java集合
Collection接口:定义了存取一组对象的方法,其中子接口Set和List分别定义了存储方式.
Set 中的数据对象没有顺序且不可以重复.
List 中的数据对象有顺序且可以重复.
Map接口定义了存储”键(Key)–值(value)映射“的方法. Map根据键对象找键对象,键值不能重复.

ArrayList : 线程不安全,效率高. 底层实现是数组. 查询比较方便,但是插入,删除效率比较低.

LinkedList : 线程不安全,效率高.底层实现是链表.遍历查询慢,插入比较快.

Vector : 线程安全的,效率低. 底层实现也是数组.

HashMap: 线程不安全,效率高

HashTable:线程安全,效率低

Map集合的底层实现: 数组 + 链表

Collection类对象在调用 remove,contains等方法时需要比较对象是否相等,这会涉及到对象类型的equals方法和hashCode方法;对于自定义的类型,需要重写equals和hashCode方法以实现自定义的对象规则.

注意: java中规定,两个内容相同的对象应该有相等的hashCode.


## Set接口
- Set 接口是Collection的子接口，Set接口没有提供额外的方法，Set接口的特性是容器类中的元素是没有顺序的，而且不可以重复。
- Set容器可以与数学中的“集合”的概念相对应。
- JavaEE API中所提供的Set容器类有HashSet，TreeSet等。

## 泛型

概念:泛型就是参数化类型,使用广泛的类型.
起因:数据类型不明确:

- 装入数据的类型都被当做Object对待,从而"丢失"自己的实际类型.
- 获取数据时往往需要转型,效率低,容易产生错误.

作用:

- 安全:在编译的时候检查类型安全
- 省心:所有的强制转换都是自动和隐式的,提高代码的重用率.

泛型类:定义时使用泛型

-	格式:<>
	

``` stylus
class 类名<字母列表>{
		修饰符 字母 属性;
		修饰符 构造器(字母){
		}
		修饰符 返回值类型 方法(字母){
		}
	}
```
**不能使用在静态属性,静态方法上**

- 使用:指定具体类型
	1. 编译时会进行类型检查;
	2. 获取数据时不需要强制类型转换.
	3. **泛型使用时不能指定基本数据类型**

- 泛型常见字母
	- T Type 表示类型
	- K V 分别代表键值中的Key和Value.
	- E 代表Element.
	- ? 代表不确定的类型 
	
	
- 泛型方法
	- 定义使用:<字母>
		

``` stylus
修饰符<字母> 返回类型 方法名(字母){
}
```
要定义泛型方法,只需要将字母放到返回值的前面

``` stylus
public static <T> void test(T t){
		System.out.println(t);
	}
	
	// extends <==
	public static <T extends Closeable> void test(T... a){
		for (T temp : a) {
			try {
				if(null != temp){
					
					temp.close();
				}
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}
```
**泛型方法<> 返回类型前面**
**只能访问对象的信息,不能修改信息**

擦除:
	- 在使用时没有指定具体的类型
	- 子类继承时没有指定类型


