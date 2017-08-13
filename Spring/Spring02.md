---
title: Spring02
tags: Spring,框架,Java
grammar_cjkRuby: true
---
[TOC]
## 注解配置Spring

### 配置步骤

- 导包4+2+1(spring包中的aop包)
- 配置Context的schema约束
- 在application.xml文件中添加一句话`<context:component-scan basepackage="com.zhiyou100.annotation"></context:component-scan>` 会扫描package中设置的包,并且包含其子包中的所有配置注解的类,会将其注入到spring容器中
- 在包下的类中把需要spring容器进行管理的类,添加注解

### 配置到容器中的注解

> 把要配置到容器中的类,在其头上添加注解,spring通过扫描就能将其放在spring的容器中,有四种注解的功能都是一样的,推荐使用后三个

- `@Component` 标准一个普通的spring Bean类。
- `@Service`:用以区分将此类是service层
- `@Controller`:用以区分将此类是web层
- `@Repository`:用以区分此类是dao层

``` stylus
@Repository("user")	
public class User {
private String name;
private int age;
private Car car;
public User() {
super();
}
public Car getCar() {
return car;
} 
public void setCar(Car car) {
System.out.println("setCar");
this.car = car;
} 
public String getName() {
return name;
} 
public void setName(String name) {
System.out.println("setName");
this.name = name;
} 
public int getAge() {
return age;
} 
public void setAge(int age) {
System.out.println("setAge");
this.age = age;} 
@Override
public String toString() {
return "User [name=" + name + ",age=" + age + ", car=" + car + "]";
}
}
```

## @scope配置创建对象模式

- @Scope(scopeName=”singleton”)指定单例模式创建对象,默认是单例
- @Scope(scopeName=”prototype”)指定多例模式创建对象

``` stylus
@Repository("user")
@Scope(scopeName="singleton")
public class User {
private String name;
private int age;
private Car car;
public User() {
super();
} 
public Car getCar() {
return car;
} 
public void setCar(Car car) {
System.out.println("setCar");
this.car = car;
} 
public String getName() {
return name;
} 
public void setName(String name) {
System.out.println("setName");
this.name = name;
} 
public int getAge() {
return age;
} 
public void setAge(int age) {
System.out.println("setAge");
this.age = age;
}
@Override
public String toString() {
return "User [name=" + name + ",
age=" + age + ", car=" + car + "]";
}
}
```
## 属性注入的注解

### 基本数据类型和String
#### @value注入属性值(对于基本数据类型)

> 使用此注解是注入的类型是基本类型包括字符串,可将其放在成员变量上,也可以将其放在set方法上,格式为 @value("xxx")

``` stylus
@Repository("user")
@Scope(scopeName="singleton")
public class User {
@Value("张三")
private String name;
private int age;
private Car car;
public User() {
super();
}
public Car getCar() {
return car;
} 
public void setCar(Car car) {
System.out.println("setCar");
this.car = car;
} 
public String getName() {
return name;
} 
public void setName(String name) {
System.out.println("setName");
this.name = name;
} 
public int getAge() {
return age;
} 
@Value("18")
public void setAge(int age) {System.out.println("setAge");
this.age = age;
} 
@Override
public String toString() {
return "User [name=" + name + ",age=" + age + ", car=" + car + "]";
}
}

```
### 引用类型

#### @Autowired和@Qualifier

> 会自动找spring容器中的相同类型的值进行匹配,如果容器中有多个符合类型的那么得再添加一个@Qualifier("bean的名称") 注解

``` stylus
@Repository("user")
@Scope(scopeName="singleton")
public class User {
@Value("张三")
private String name;
private int age;
@Autowired
@Qualifier("car2")
private Car car;
public User() {
super();
} 
public Car getCar() {
return car;
} 
public void setCar(Car car) {
System.out.println("setCar");
this.car = car;
} 
public String getName() {
return name;
} 
public void setName(String name) {
System.out.println("setName");
this.name = name;
}
public int getAge() {
return age;
}
@Value("18")
public void setAge(int age) {
System.out.println("setAge");
this.age = age;
} 
@Override
public String toString() {
return "User [name=" + name + ",
age=" + age + ", car=" + car + "]";
}
}
```
#### @Resource

> 对于引用类型直接指定注入哪个名称的对象

``` stylus
@Repository("user")
@Scope(scopeName="singleton")
public class User {
@Value("张三")
private String name;
private int age;
@Resource(name="car")
private Car car;
public User() {
super();
}
public Car getCar() {
return car;
} 
public void setCar(Car car) {
System.out.println("setCar");
this.car = car;
} 
public String getName() {
return name;
} 
public void setName(String name) {
System.out.println("setName");
this.name = name;
} 
public int getAge() {
return age;
}
@Value("18")
public void setAge(int age) {
System.out.println("setAge");
this.age = age;
}
@Override
public String toString() {
return "User [name=" + name + ",age=" + age + ", car=" + car + "]";
}
}
```
### 声明周期方法

> 注意需要设置容器的close才能看到销毁操作,并且只支持singleton的对象

- @PostConstruct放在初始化方法上面
- @PreDestroy放在销毁的方法上面

``` stylus
@PostConstruct
public void init(){
System.out.println("构造方法调用后调用");
} 
@PreDestroy
public void destory(){
System.out.println("容器关闭前销毁");
}

```
## Spring中的JUnit4测试

- 导包4+3+1(spring包中的test包)
- 在测试的类上方添加注解@RunWith(SpringJUnit4ClassRunner.class) 表示让SpringJUnit4ClassRunner这个类帮我们创建容器
- 然后在类上添加

``` stylus
//配置相关配置文件的路径
@ContextConfiguration("classpath:top/xiesen/annotation/applicationContext.xml")
```
- 在需要获取容器中的对象的时候设置为成员变量,使用Resource或者Autowired进行获取
- 正常使用test进行测试

``` stylus
//使用这个类帮助我们去创建容器
@RunWith(SpringJUnit4ClassRunner.class)
//配置配置文件的路径,必须要写classpath
@ContextConfiguration("classpath:top/xiesen/annotation/applicationContext.xml")
public class TestDemo {
@Resource(name="user")
private User u;
@Test
public void test01(){
System.out.println(u);
}
}
```
## aop(面向切面编程)

> Aspect Oriented Programming是一种编程思想,进行重复的代码进行横向的抽取.

### Spring中两种代理模式

#### 动态代理
> 动态代理是面向接口的，具体实现步骤如下：

- 编写接口

``` stylus
public interface Interface1 {

	void eat();
}

```

- 编写目标class

``` stylus
public class TargetClass implements Interface1 {

	@Override
	public void eat() {
		System.out.println("吃饱了，就开心");
	}

}
```


- 写动态代理

``` stylus
@Test
	public void testDynamiProxy(){
		// 获取系统的类加载器
		ClassLoader loader = ClassLoader.getSystemClassLoader();
		// 需要代理的目标类的接口
		Class []interfaces = {Interface1.class}; 
		// 目标对象
		Interface1 i = new TargetClass();
		
		Interface1 proxy = (Interface1) Proxy.newProxyInstance(loader, interfaces, new InvocationHandler() {
			
			@Override
			public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
				System.out.println("start().....");
				Object ret = method.invoke(i, args);
				System.out.println("over().........");
				return ret;
			}
		});
		proxy.eat();
	}
```


####  cglib代理

cglib是面向继承实现代理的。

``` stylus
public class CGLibProxyTest implements MethodInterceptor{
	
	public Object CGLibProxy() {
		// 创建生成代理工具类
		Enhancer enhancer = new Enhancer();
		// 设置代理对象的父类
		enhancer.setSuperclass(People.class);
		// 调用方法的时候进行回调
		enhancer.setCallback(this);
		// 创建代理对象
		Object proxy = enhancer.create();
		return proxy;
	}
	
	/**
	 * proxyObj代理对象
	 * method原始方法
	 * arg2方法参数 
	 * methodProxy代理方法 
	 */
	@Override
	public Object intercept(Object proxyObj, Method method, Object[] arg2, MethodProxy methodProxy) throws Throwable {
		System.out.println("start()........");
		Object invokeSuper = methodProxy.invokeSuper(proxyObj, arg2);
		System.out.println("end()...........");
		return invokeSuper;
	}

}

```

#### aop中的名词解释

1. Joinpoint(连接点)

> 目标对象中所有可以增强的方法叫做连接点

2. Pointcut(切入点)

>目标对象中要增强的方法

3. Advice(通知/增强)

> 增强的代码

4. Target(目标对象)
> 被代理对象

5.  Weaving(织入)

> 将通知应用到连接点的过程

6.  Proxy(代理)

> 生成的代理对象

7. Aspect(切面)

> 切入点+通知就形成了切面

## aop-xml配置

![aop配置示意图][1]

### 导包

> 导包4+2+1(spring包中的aop包)+1(spring中的aspect包)+1(spring依赖包中aopalliance)+1(spring依赖包中org.spectj包中的weaver包)

![enter description here][2]

![enter description here][3]


### 编写通知代码

> 在spring中通知代码是通过方法的功能来实现的,需要我们写一个类将需要的通知写在不同的方法中

``` stylus
public class MyAdvice {

	/*
	* 1.前置通知
	* 2.后置通知(出现异常不会调用)
	* 3.环绕通知
	* 4.异常拦截通知
	* 5.后置通知(无论是否出现异常都会调用)
	*/
	
	public void beforeAdvice(){
		System.out.println("before()...");
	}
	
	public void AfterAdvice(){
		System.out.println("AfterAdvice()...");
	}
	public void AfterRuturningAdvice(){
		System.out.println("AfterRuturningAdvice()...");
	}
	
	public void aroundAdvice(ProceedingJoinPoint pjt) throws Throwable{
		System.out.println("roundAdvice start...");
		pjt.proceed();
		System.out.println("roundAdvice end...");
	}
	public void ExceptionAdvice(){
		System.out.println("ExceptionAdvice()...");
	}
}

```
### spring中通知的类型

- 前置通知:目标方法之前进行调用
- 后置通知(出现异常不会调用):目标方法之后调用
- 环绕通知:目标方法前后调用
- 异常拦截通知:出现异常的时候调用
- 后置通知(无论是否出现异常都会调用):目标方法之后调用

### 编写目标对象


``` stylus
public class UserServiceImpl implements UserService {

	@Override
	public void addUser() { 
		System.out.println("addUser()......");
	}

	@Override
	public void deleteUser() {
		System.out.println("deleteUser()......");
	}

	@Override
	public void updateUser() {
		System.out.println("updateUser()......");
	}

	@Override
	public void selectAllUser() {
		System.out.println("selectAllUser()......");
	}

}
```
### 进行文件配置

``` stylus
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd">
<context:property-placeholder location="classpath:db.properties"/>
<bean name="userService" class="top.xiesen.spring.service.impl.UserServiceImpl"></bean>

<bean name="myAdvice" class="top.xiesen.spring.aop_annotation.MyAdvice"></bean>
<aop:config>
<!--配置切入点-->
	<aop:pointcut expression="execution(* top.xiesen.spring.service.impl..*ServiceImpl.*())" id="pt"/>
	
<aop:config>
	<!-- 
		配置切入点
		id：表示给切入点起的名字
		expression是表达式 格式为：execution()
		切入点表达式具体方法 public void top.xiesen.spring.service.impl.UserServiceImpl.addUser()
		修饰符可以不写 void top.xiesen.spring.service.impl.UserServiceImpl.addUser()
		返回值类型不限 * top.xiesen.spring.service.impl.UserServiceImpl.addUser()
		方法民称不限 * top.xiesen.spring.service.impl.UserServiceImpl.*()
		方法参数不限 * top.xiesen.spring.service.impl.UserServiceImpl.*(..)
		以ServiceImpl结尾 * top.xiesen.spring.service.impl.*ServiceImpl.*()
		包含impl子包中符合条件的 * top.xiesen.spring.service.impl..*ServiceImpl.*()
		
	 -->
	<aop:pointcut expression="execution(* top.xiesen.spring.service.impl..*ServiceImpl.*())" id="pt"/>
	<!-- 
		配置切面,ref表示通知
		pointcut-ref 表示切入点
		aop:before 前置通知
		aop:after 后置通知
		aop:after-returning 后置通知，出现异常被阻断
		aop:around 环绕通知
		aop:after-throwing异常拦截通知
		aop:after后置通知,忽略异常
	 -->
	<aop:aspect ref="myAdvice">
		<aop:before method="beforeAdvice" pointcut-ref="pt"/>
		<aop:after method="AfterAdvice" pointcut-ref="pt"/>
		<aop:after-returning method="AfterRuturningAdvice" pointcut-ref="pt"/>
		<aop:after-throwing method="ExceptionAdvice" pointcut-ref="pt"/>
		<aop:around method="aroundAdvice" pointcut-ref="pt"/>
	</aop:aspect>
</aop:config>
```
- 在通知类中进行注解

``` stylus
// 表示该类是一个通知类
@Aspect
public class MyAdvice {
	@Pointcut("execution(* top.xiesen.spring.service.impl..*ServiceImpl.*())")
	public void pointCut(){}

	@Before("MyAdvice.pointCut()")
	public void beforeAdvice(){
		System.out.println("before()...");
	}
	@After("MyAdvice.pointCut()")
	public void AfterAdvice(){
		System.out.println("AfterAdvice()...");
	}
	@AfterReturning("MyAdvice.pointCut()")
	public void AfterRuturningAdvice(){
		System.out.println("AfterRuturningAdvice()...");
	}
	@Around("MyAdvice.pointCut()")
	public void aroundAdvice(ProceedingJoinPoint pjt) throws Throwable{
		System.out.println("roundAdvice start...");
		pjt.proceed();
		System.out.println("roundAdvice end...");
	}
	@AfterThrowing("MyAdvice.pointCut()")
	public void ExceptionAdvice(){
		System.out.println("ExceptionAdvice()...");
	}
}

```


  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/aop.png "aop"
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502258771065.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1502258859570.jpg