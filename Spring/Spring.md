---
title: Spring
tags: Spring,框架,Java
grammar_cjkRuby: true
---

## 注解

> 我们可以认为注解Annotation是一种属于代码级别的说明,是一种具有功能的说明.是从jdk1.5版本以后添加的新特性,其格式为 @注解名称 .对比注释而言,注释是写给人看的,是没有功能的文字.注解是写个JVM看的,具有功能的代码

## 注解的作用

> 在目前的主流应用在替代配置文件

- 对比servlet2.5,Servlet3.0新加了很多的新特性,其中就有提供了注解的支持,也就是不需要使用web.xml可以进行路径的配置
- 优点:开发效率高
- 确定:耦合性太强,不便于维护


## 常见注解

- `@Override` ：告知编译器此方法是覆盖父类的
- `@Deprecated` ：标注过时
- `@SuppressWarnings` ：压制警告

## 自定义注解(了解)

- 使用关键字@interface表示注解
- 注解中定义的属性格式为 `修饰符 数据类型 属性名称() default 具体的值;`
	- 修饰符只能是public abstract,和接口相同会默认添加
	- 数据类型可以是: `基本类型 , String , 枚举类型 , 注解类型 , Class类型 , 以上类型的一维数组类型`
	- default是默认值可以不写,如果不写在使用注解的时候就必须进行设置,如果有默认值就可以不用进行设置
- 注意：如果属性的名字是value，并且注解的属性设置的时候只需设置这一个 那么在使用注解时可以省略value

## 使用注解(掌握)

> 在类,方法,字段 上面是 `@注解名称(属性名=属性值,属性名=属性值)`

## 元注解

> 可以认为是注解的注解

### @Retention

> 代表注解的可见范围

- SOURCE: 注解在源码级别可见(默认是source级别)
- CLASS：注解在字节码文件级别和源码级别可见
- RUNTIME：注解在整个运行阶段以及之前的两个级别都可见
- **注意**:要想解析使用了注解的类 ， 那么该注解的Retention必须设置成Runtime

### @Target

> 代表注解修饰的范围：类上使用，方法上使用，字段上使用

![反射机制][1]

- FIELD:字段上可用此注解
- METHOD:方法上可以用此注解
- TYPE:类/接口上可以使用此注解
- PARAMETER:参数上可以使用
- CONSTRUCTOR:构造函数上可以使用
- LOCAL_VARIABLE:局部变量上可以使用

``` stylus
	/**
	 * 指示在注释元素（和包含在注释元素中的所有程序元素中）应被抑制命名的编译器警告。
	 * 注意，警告在给定元素抑制是警告抑制所有包含元素的一个超集。
	 * 例如，如果您对一个类进行注释以抑制一个警告和注释一个抑制另一个警告的方法，
	 * 这两个警告将被抑制在该方法中。
	 */
	@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
	@Retention(RetentionPolicy.SOURCE)
	public @interface SuppressWarnings {
	 String[] value();
	}
```
## 通过反射的方式进行获取(了解)

``` stylus
		Class ac = AnnotionTest.class;
		Method[] methods = ac.getMethods();
		for (Method me : methods) {
			if (me.isAnnotationPresent(MyAnno.class)) {
				MyAnno annotation = me.getAnnotation(MyAnno.class);
				System.out.println(annotation.height() + "--" + annotation.value());
			}
		}
```
## Junit单元测试

> 在我们编写代码的过程中,可能会写很多的方法,当写完某个方法的时候我们往往需要测试一下某个方法是正确的,这个时候我们就可以使用Junit单元测试帮助我们完成Junit是java语言的单元测试框架,属于第三方的一个工具,一般情况都需要导入相应的jar包才能使用,不过多数的java开发环境已经帮我们集成了这个工具,在我们的eclipse中同样集成了这个测试工具

### 使用junit步骤和要求

- 方法必须是 public void 修饰
- 方法必须是无参的方法
- 在方法上添加注解@Test导入并导入相应的jar包
- run as选中Junit进行测试
- **注意:如果一个类中有多个@Test的测试,那么会一起执行,如果需要执行具体某一个,可以选中方法后run as**

### Junit的其他测试相关注解

- **@Test**：把一个方法标记为测试方法
- **@Before**：每一个测试方法执行前自动调用一次(需要配合@Test使用)
- **@After**：每一个测试方法执行完自动调用一次(需要配合@Test使用)
- **@BeforeClass**：所有测试方法执行前执行一次，在测试类还没有实例化就已经被加载，所以用static修饰(需要配合@Test使用)
- **@AfterClass**：所有测试方法执行完执行一次，在测试类还没有实例化就已经被加载，所以用static修饰(需要配合@Test使用)
- **@Ignore**：暂不执行该测试方法(需要配合@Test使用)

``` stylus
public class Test {

	// 测试程序
	@org.junit.Test
	public void test01(){
		System.out.println("test01()");
	}
	
	// 测试静态方法,执行测试程序之前执行
	@BeforeClass
	public static void testBeforeClass(){
		System.out.println("testBeforeClass()");
	}
	//测试静态方法,执行测试程序之后执行
	@AfterClass
	public static void testAfterClass(){
		System.out.println("testAfterClass()");
	}
	// 测试程序之前执行
	@Before
	public void testBefore(){
		System.out.println("testBefore()");
	}
	// 测试程序之后执行
	@After
	public void testAfter(){
		System.out.println("testAfter()");
	}
}
```
懒的解释了，直接看结果

![测试运行结果示意图][2]

### @Test属性(了解)

- expect属性,用来测试异常相关,其格式为 @Test(expect =xxxException.class) ,如果出现异常测试成功,如果未出现异常测试失败

``` stylus
@Test(expected = ArithmeticException.class)
	public void test011() {
		System.out.println("test02");
		System.out.println(1 / 0);
	}
```

- ti
meout属性,是用来测试超时操作的单位是毫秒其格式为**@Test(timeout=毫秒值)** ,如果运行时间在设置之内,测试通过,如果超出测试失败

``` stylus
	@Test(timeout = 1)
	public void test01() {
		System.out.println("test01");
		for (int i = 0; i < 10000; i++) {
			System.out.println("sssss");
		}
	}
```
### 断言判断(了解)

- 判断结果是否是预期的结果格式为 assertEquals(期望结果,实际结果);
- 断言的包的导入 import static org.junit.Assert.*;

> 使用了JDK5中的静态导入，只要在import关键字后面加上static关键字，就可以把后面的类的static的变量和方法导入到这个类中，调用的时候和调用自己的方法没有任何区别。


	

  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6.png "反射机制"
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502111809979.jpg