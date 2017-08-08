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

## Spring

> Spring是于 2003 年兴起的一个轻量级的 Java 开发框架.Spring 的核心是控制反转（IOC）和面向切面（AOP）。Spring 是一个分层的 JavaSE/EEfull-stack(一站式) 轻量级开源框架。

### 一站式框架

- web层:可以使用Spring提供的SpringMVC框架进行请求的处理
- service层:可以使用Spring的IOC进行对象的创建
- dao层:可以使用Spring提供的JDBC模板进行处理
- spring不但能够自身完成web应用的开发,并且能够完美兼容其他开源框架,因为spring是一个容器

### Spring环境搭建以及HelloWorld案例

- 下载Spring相关jar包[下载地址][3]

![enter description here][4]

![Spring 包结构][5]
	
- 导包(4+2)

![Spring 模块划分][6]

![Spring lib文件夹中的四个jar包][7]

![依赖包中的日志包][8]

![依赖中的log4j][9]

- 创建普通的类,让Spring帮我们创建

``` stylus
public class People {
	private String name;
	private int age;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public void eat() {
		System.out.println("吃饭了");
	}

	@Override
	public String toString() {
		return "People [name=" + name + ", age=" + age + "]";
	}
}
```

- 创建xml的配置文件,将我们写的类配置到对应的文件中,建议放在src下,建议命名为applicationContext.xml,可以认为是spring的规范,如果不按照此规范也不会出错
- 设置schema约束,是需要按照对应的命名空间来决定标签中的顺序的,也可以认为配置后书写标签能够有提示
- 在配置文件写 `<bean name="people"class="com.zhiyou100.demo01.People"></bean>`
- 创建一个测试类,写一个测试方法

``` stylus
	@Test
	public void test01(){
		//创建spring容器对象,加载src下的配置文件
		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		//从容器中通过name获取对象
		People person = (People) context.getBean("people");
		//调用对象的方法
		person.eat();
	}

```
## 配置schema约束步骤

![步骤一][10]

![步骤二][11]

![步骤三][12]

![步骤四][13]

![步骤五][14]

![步骤六][15]

![步骤七][16]

![步骤八][17]

## schema讲解

> 配置约束是为了在xml文件中进行配置的时候,能够按照约束进行标签的校验

- 配置一个xml文件最多只能有一个匿名的命名空间,所以我们将bean这个命名空间配置为匿名,如果将其配置为xx,那么当使用bean中的标签的时候需要加 `<xx:bean></xx:bean>` ,在配置spring的时候,我们习惯于将bean标签配置为匿名的,可以认为是一种规范
- 因为要使用schemaLocation来进行配置键值对匹配,所以我们先配置了别名为xsi的`xmlns:xsi="http://www.w3.org/2001/XMLSchemainstance"` 命名空间
- 右配置了一个匿名的命名空间 `xmlns="http://www.springframework.org/schema/beans"`
- 通过xsi的schemaLocation,将键值对进行对应

``` stylus
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-4.2.xsd "
```
- 将来我们引用其他的命名空间的时候也是使用相同的方法进行配置	

## IOC控制反转
> IOC控制反转(Inversion of Control)将对象的创建权交给了Spring,我们通过Spring,就不用每次去创建对象,只需要通过Spring容器调用getBean(“name属性或id属性”)获取对应的对象

- ApplicationContext实现类会在创建的时候,将内部配置的所有对象加载到容器中
- ClassPathXmlApplicationContext是ApplicationContext的接口实现类,是从classpath的目录下加载配置文件

### bean元素的配置和创建

> IOC控制反转(Inversion of Control)将对象的创建权交给了Spring,我们通过Spring,就不用每次去创建对象,只需要通过Spring容器调用getBean(“name属性或id属性”)获取对应的对象

- ApplicationContext实现类会在创建的时候,将内部配置的所有对象加载到容器中
- ClassPathXmlApplicationContext是ApplicationContext的接口实现类,是从classpath的目录下加载配置文件

### bean元素的配置和创建

> 凡是交给spring容器管理的对象,都是使用bean元素进行配置

### name属性和class属性

- name属性给管理的对象起名字,根据该名称能获取对象
- class属性是获取该对象的完整类名
- id属性跟name属性的功能和用法一模一样,和name属性相比,id属性唯一不重复,并且不能含有特殊字符,如果只设置name属性,那么name属性在容器中就必须是唯一的

### bean元素的创建

> 默认调用空参构造方法进行创建对象,如果没有空参构造方法就会报错误

- 空参构造方法创建

``` stylus
<!--空参构造方法-->
<bean name="people1" class="com.zhiyou100.demo01.People"></bean>
```

- 静态工厂方式创建

``` stylus
<!--静态工厂-->
<bean name="people2" class="com.zhiyou100.create.PeopleFactory" factory-method="createPeople"></bean>
```

- 实例工厂方式创建

``` stylus
<!--实例工厂-->
<bean name="factoryBean" class="com.zhiyou100.create.PeopleFactory"></bean>
<bean name="people3" factory-bean="factoryBean" factory-method="CreatePeople2"></bean>
```

### scope属性

> 设置bean元素的创建对象的方式

- singleton单例模式创建,当前对象在容器中只会创建一个对象

``` stylus
<bean name="people1" class="com.zhiyou100.demo01.People" scope="singleton"></bean>
```
- prototype多例模式创建,当前对象在容器中每次调用getBean返回的是新的对象

``` stylus
<bean name="people1" class="com.zhiyou100.demo01.People" scope="prototype"></bean>
```
> 以后绝大多数的对象都使用默认值就行,在Struts2中的action要配置成prototype


### 生命周期属性

- init-method初始化方法
- destroy-method销毁方法
- 不能和prototype一起使用,如果是多例的话,spring容器将对象创建完成后就不再负责管理这个对象了,只有是singleton对象容器才会负责去管理它

``` stylus
<bean name="people1" class="com.zhiyou100.demo01.People" scope="singleton" init-method="init" destroy-method="destory"></bean>
```

## DI依赖注入

> 依赖注入(Dependency Injection),需要有 IOC 的环境,Spring 创建这个类的过程中,Spring 将类的依赖的属性设置进去.

### 注入方式

> 如果是基本类型使用value,如果是引用类型就需要使用ref

#### set方法注入

> 类中必须要有set方法,否则属性无法正常注入

``` stylus
<bean name="user" class="com.zhiyou100.di.User">
<bean name="car" class="com.zhiyou100.di.Car">
<property name="price" value="8000"></bean>
```
#### 构造方法注入

> 通过构造方法创建对象,并传递相应的参数给具体的属性

``` stylus
<!--
构造方法注入属性
name 表示参数名称
value 给参数赋的具体值
index 表示参数的索引
type 表示参数的类型
以上所有的属性并不是全部都要设置
-->
<bean name="user2" class="com.zhiyou100.di.User">
<constructor-arg name="name" value="韩梅梅"index="1" type="java.lang.String"></constructor-arg>
<constructor-arg name="car" ref="car"></constructor-arg>
</bean>
```
#### p名称空间注入

``` stylus
xmlns:p="http://www.springframework.org/schema/p"
<bean name="user3" class="com.zhiyou100.di.User" p:name="李雷" p:age="18" p:car-ref="car"></bean>
```
#### SPEL表达式注入

> spring expression language(表达式语言注入),必须在前面基础上完成,比如解决类类似配置一个bean的属性是另一个bean的属性值的时候,需要使用spel注入

``` stylus

<bean name="user4" class="com.zhiyou100.di.U
ser">
<property name="age" value="#{car.price > 300000 ? 35 : 20}"></property>
<property name="name" value="#{'zhangSan'.toUpperCase()}"></property>
<property name="car" ref="car"></property>
</bean>
```

## 复杂类型注入

- 数组类型
- list类型
- map类型

``` stylus
public class ComplexTypeObject {
	Object[] arr;
	List<Object> list;
	Map<Object, Object> map;
	public Object[] getArr() {
	return arr;
} 
public void setArr(Object[] arr) {
this.arr = arr;
} 
public List<Object> getList() {
return list;
} 
public void setList(List<Object> list) {
this.list = list;
} 
public Map<Object, Object> getMap() {
return map;
}
public void setMap(Map<Object, Object> map) {
this.map = map;
} 
@Override
public String toString() {
return "ComplexTypeObject [arr=" + Arrays.toString(arr) + ", list=" + list + ",map=" + map + "]";
}
}
<bean name="ct" class="com.zhiyou100.di.ComplexTypeObject">
<property name="arr">
	<array>
	<value>休息</value>
	<value>开会</value>
	<ref bean="user4"/>
	</array>
</property>
<property name="list">
	<list>
	<value>北京</value>
	<value>上海</value>
	<ref bean="car"/>
	</list>
</property>
<property name="map">
	<map>
	<entry key="driverClass" value="com.mysql.jdbc.Driver"/>
	<entry key="car" value-ref="car"/>
	<entry key-ref="user4" value-ref="car"/>
	</map>
</property>
</bean>
```

## Spring 的分模块配置文件的开发

> 有时为了简介和便于阅读,会将配置文件按照不同的功能模块分成多个xml文件,在进行引入的时候可以使用标签<import resource="相对路径"> 进行引入,注意是相对路径,相对于当前引入到的xml文件的路径,不建议使用”/”开头

- 注意是相对路径,如果a.xml中引用b.xml,那么路径应该从a.xml文件所在的文件夹为起始路径


## 安装STS

> spring tool suite是spring开源的基于eclipse的插件,能够方便我们的spring开发

- 查看eclise的版本

![enter description here][18]


![enter description here][19]

[下载地址][20]找到对应版本的sts插件,进行安装

![enter description here][21]

![enter description here][22]

![enter description here][23]

![enter description here][24]



  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6.png "反射机制"
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502111809979.jpg
  [3]: http://repo.springsource.org/libs-release-local/org/springframework/spring/
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502113837906.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114002673.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502113953603.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114069245.jpg
  [8]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114101844.jpg
  [9]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114166865.jpg
  [10]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114546056.jpg
  [11]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114589620.jpg
  [12]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114617581.jpg
  [13]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114656578.jpg
  [14]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114692838.jpg
  [15]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114740407.jpg
  [16]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114772296.jpg
  [17]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502114810497.jpg
  [18]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502196200340.jpg
  [19]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502196242757.jpg
  [20]: https://spring.io/tools/sts/all
  [21]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502196522243.jpg
  [22]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502196627342.jpg
  [23]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502196695486.jpg
  [24]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502196736578.jpg