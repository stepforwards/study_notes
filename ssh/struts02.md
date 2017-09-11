---
title: struts02
tags: struts2,ssh,Java,框架
grammar_cjkRuby: true
---

# struts2架构

![struts2架构示意图][1]

- 因为将过滤器配置成 /* ,所以所有的请求都会都会经过filter
- filter会将这个请求给ActionMapper,这个类会将这个请求中访问的信息进行提取,返回一个 ActionMapping (可以认为提取命名空间,提取actionName的对象)对象返回给filter
- filter将 ActionMapping 对象给 ActionProxy
- ActionProxy 会让 ConfigurationManager 对象读取配置文件ConfigurationManager 会读取struts2的核心配置文件,并将结果返回给 ActionProxy
- ActionProxy 通过配置信息和 ActionMapping 信息汇总后创建一个 action 对象,并将这个 action 对象交给ActionInvocation ,还会给 ActionInvocation 一个 interceptors 集合,集合中放了20个拦截器,拦截器中就是struts帮我们封装的20个功能
- ActionInvocation 按照 ActionProxy 要求依次调用20个拦截器的前处理方法和 action 方法,通过 action方法之后,获取 result ,result通过内部转发或者重定向到jsp,然后又经过拦截器后处理方法,形成响应,返回给客户端

> 通过struts架构拦截器为我们提供了丰富的功能,并支持可插拔功能,并且体现了aop思想,我们也可以自定义拦截器

# Action的生命周期

> servlet中当请求第一次到达的时候创建一个servlet对象，当服务器关闭的时候servlet销毁，Action每次请求都会创建一个对象

## 获取表单参数

### 属性驱动

- 方法1，在成员变量位置声明变量，变量名称和提交参数名称一致，需要生成setter/getter
- 方法2，在成员变量位置声明模型对象，提供模型对象的setter/getter，需要在表单中的name属性设置为模型对象名称.属性名
- 注意：提交的参数是字符串，在struts中可以自动进行数据类型转换，数据类型转换支持8大基本数据类型以及其包装类，还可以将字符串转换成Date类型，但是类型必须是(yyyy-MM-dd)

### 模型驱动

> 前台页面不用再使用**模型对象名称.属性名**，action中定义成员变量为model类型，不用再写setter/getter

![enter description here][2]

- action实现ModelDriven<User>接口，并实现抽象方法，抽象方法的返回值是User
- 在成员变量位置创建一个User对象
- 将User对象作为抽象方法的返回值

# 复杂类型
## List类型

- list前台页面

![enter description here][3]

- Action页面

![enter description here][4]


## Map类型

- 前台页面
![enter description here][5]

- Action页面
![enter description here][6]

# OGNL表达式语法
> 对象视图导航语言(Object-Graph Navigation Language)是一种表达式语言，struts2整合了OGNL语言其提供了强大功能，OGNL和EL表达式相似，EL是从11个域中获取数据，OGNL表达式是从OGNLContext中读取值

# OGNL Context

> OGNLContext由两个部分构成，ROOT和CONTEXT，OGNL表达式就是从这两个部分获取值

![enter description here][7]

ROOT部分可以放置的是Map类型的键值对，且必须只能放键值对

# API

- 准备Root中的数据`User rootUser = new User("kkkk","pppp");`
- 准备Context中的数据

``` java
Map<String, User> map = new HashMap<> ();
map.put("user1", new User("张三", "123"));
map.put("user2", new User("李四", "234"));
```
- 创建OGNLContext `OgnlContext oc = new OgnlContext();`
- 向oc的root中放值 `oc.setValues(map);`

- 获取root中的对象: Object root = oc.getRoot();
- 获取context中的对象: Map context = oc.getValues();
- 获取root中对象的name属性 String name = (String)Ognl.getValue("name", oc, oc.getRoot());
- 获取context中user1的pwd属性属性 String pwd = (String)Ognl.getValue("#user1.pwd", oc, oc.getRoot());
- 设置root中对象的name属性 Ognl.getValue("name='hello'", oc, oc.getRoot());
- 设置context中user1的pwd属性并且获取 String pwd = (String)Ognl.getValue("#user1.pwd='pp',#user1.pwd",oc, oc.getRoot());
- 调用root对象的get方法 String name =(String)Ognl.getValue("getName()", oc,oc.getRoot());
- 调用context中user1的setName方法,并获取 String pp =(String)Ognl.getValue("#user1.setName('ioio'),#user1.getName()",oc, oc.getRoot());
- 调用其他类的静态方法 Double t = (Double)Ognl.getValue("@java.lang.Math@random()", oc,oc.getRoot());
- 获取类的静态成员变量 Double t1 = (Double)Ognl.getValue("@java.lang.Math@PI", oc,oc.getRoot());
- 创建list对象,并获取长度 Integer size =(Integer)Ognl.getValue("{'tom','李雷','韩梅梅'}.size()", oc, oc.getRoot());
- 取出数组中指定索引的内容 String content =(String)Ognl.getValue("{'tom','李雷','韩梅梅'}[0]",oc, oc.getRoot()); 或者写 String content(String)Ognl.getValue("{'tom','李雷','韩梅梅'}.get(0)", oc, oc.getRoot());
- 创建map,并获取长度 Integer mapSize =(Integer)Ognl.getValue("#{'name':'李雷','age':18}.size()", oc, oc.getRoot());
- 获取map中指定key的值 String mapContent =(String)Ognl.getValue("#{'name':'李雷','age':18}['name']", oc, oc.getRoot()); 或者 String mapContent= (String)Ognl.getValue("#{'name':'李雷','age':18}.get('name')", oc, oc.getRoot());

# 值栈
> Struts2为我们提供了OGNLContext，并且在root部分和contxt部分放置了很多数据，我们需要使用OGNL表达式获取对应的OGNLContext中的数据，struts2把这个OGNL队形起名为ValueStack，也就数我们所说的struts中的值栈本质就是一个OGNLContext对象

- 值栈 中的root部分就是一个集合，是栈的结构，其本质就是一个ArrayLiat
- 值栈中的context部分就是我们介绍的ActionContext(其本质就是一个Map)
- 获取值栈对象，可以通过`ValueStack valueStack = ActionContext.getContext().getValueStack();`

# 栈结构

- 栈是一种存储的结构
- 栈的特点是**先进后出，后进先出**
- 对于栈的操作一般都是有两个方法**push方法** 和 **pop方法**
- 我们可以通过一个ArrayList来实现栈的结构，当调用push方法的时候，将索引为0的位置放入数据，调用pop方法的时候将索引为0的内容删除并返回就实现了压栈和弹栈的过程
- 通过查看源代码可以实现值栈中的root部分就是按照上述内容进行实现的
- 对于ValueStack，因为root部分是一个栈的结果，当我们通过OGNL表达式调用某个属性的时候，他会从栈顶依次向栈底查找对应的数据，如果发现了就停止

# 通过debug标签查看值栈

- 在转发后的jsp页面导入标签库struts2的标签库`<%@ tablib prefix="s" uri="struts-tags">
- 在界面上书写<s:debug></s:debug>`标签，会生成debug标签
- 栈中默认放置的就是当前访问的action对象

#表单参数接收原理

＞将表单中的数据交给我们的是由一个名叫param的拦截器去具体实现的

## 属性驱动

- 当将数据提交到表单的时候,struts2实际上使用的是OGNL表达式,向值栈中添加数据,提交表单的时候数据格式为 name=123 通过使用OGNL表达式进行操作,此时的表达式为 name=123 ,而值栈中的root元素刚好是对应的action,所以就对action的属性name赋值成功
- 当使用属性驱动的第二种形式的时候,数据格式为 u.name=123 anction中的成员变量名称是u,所以OGNL相当于给root中的属性进行赋值

## 模型驱动

- 在值栈中的root中,在赋值之前,将接收参数的对象放入栈顶,然后再由param拦截器对对User赋值,如果我们想自己实现这个过程,可以通过 prepare 拦截器去实现此功能

# 配置文件中使用OGNL表达式

- 内部转发和重定向到指定页面携带参数
	- 在路径后添加 /aa.jsp?name=${name}&amp;pwd=${pwd} ,注意 `&amp` 代表在xml文件中的 $ 的转义字符 ${name} 和${pwd} 表示使用OGNL表达式从root中寻找属性为name和pwd的值

- 内部转发和重定向到指定action携带参数
	- 添加 <param name="name">${name}</param> 和 <param name="pwd">${pwd}</param> 代表使用OGNL表达式从root中获取指定属性的值
	- 只要param中的name属性不是struts中的属性的时候,就会被当作请求的参数进行请求

# struts中的访问流程
## 路径排除(了解)

![enter description here][8]

如果我们需要排除某些路径,不经过struts2来访问的时候可以配置常量, `<constant name="struts.action.excludePattern" value="/res/.*,/css/.*,/images/.*,/js/.*,/services/.*"/>` ,注意 struts.action.excludePattern 的值是用逗号（，）隔开的正则表达式。比如： /res/.* 表示以 /res/ 为开头的路径。 .* 表示零个或多个任意字符

## 静态资源问题

> 对于静态资源我们一般是不用设置的,因为在struts2中,有专门处理静态资源的方法,他会判断,如果是静态资源,就会将静态资源进行放行

![enter description here][9]

## 值栈的生命周期(重要)

> 通过查看源码我们可以发现,值栈的创建在于一次请求,每次请求都会创建一个ActionContext对象,并且创建一个值栈值栈的生命周期和ActionContext和Request生命周期相同,存活于同一个请求

## 包装request(重要)

> 通过对原生的request对象进行包装,生成了一个 StrutsRequestWrapper 的类,在此类中重写了getAttribute 方法

![enter description here][10]

![enter description here][11]

## 查找顺序

> 注意查找当我们调用 request.getAttribute() 方法的时候,查找顺序是以下顺序所以当我们在jsp页面中输入 ${name} 它实际调用的就是经过包装的 request.getAttribute() 方法

1. 查找原生request中的内容
2. 查找值栈中的root中的内容
3. 查找值栈中的context中的内容

## 放置内容

> 我们之前讲过想要往request域中放入数据的时候,需要调用ActionContext的put方法,推荐也是调用上述方法去放值所以我们之前调用的actionContext中的put方法的时候,在界面能够找到原因就是值栈中的context部分就是ActionContext,在界面中使用el表达式就能够进行获取操作

# 拦截器
> 拦截器是struts2实现功能的核心

## 生命周期

> 项目启动的时候创建拦截器,项目销毁的时候拦截器销毁

## 创建方式
> 拦截器的创建方式有3种

1. 实现Interceptor接口,实现3个方法,init方法,destory方法,intercept方法
2. 继承AbstractInterceptor,实现抽象方法 intercept ,其本身也是实现 Interceptor 接口
3. 继承 MethodFilterInterceptor 实现抽象方法 doIntercept ,一般我们使用此方法进行创建

## 拦截过程

- 拦截器的方法中有个参数 ActionInvocation invocation ,他就代表的使我们的action对象,通过对象调用 `invocation.invoke();` 表示action方法的调用,也表示放行操作
- 类似于spring中的环绕通知,写在它前面的代码叫做拦截器的前处理
- 写在其后面的代码叫做其后处理程序对于返回值,一般用于不放行跳转到结果界面,如果是方向就将 `invocation.invoke();` 的返回值赋值给它
## 配置拦截器

> 当我们将拦截器配置在package中的时候,默认是会对package中所有的action都拦截,也可以单独配置到某个action中(比较少用)

![enter description here][12]

- 注册拦截器
- 注册拦截器栈
- 指定默认拦截器栈

## 配置拦截器的排除
> 默认情况下是会拦截package中所有的action

- 在对应的拦截器栈中的自定义的拦截器上添加 `<param name="excludeMethods">add,delete</param>` 标签,指定排除规则

![enter description here][13]

- 也可以指定某些方法拦截 `<param name="includeMethods">add,delete</param>`

# 配置全局异常界面

``` xml
<global-exception-mappings>
	<exception-mapping result="error" exception="java.lang.Exception"></exception-mapping>
</global-exception-mappings>
```

# 配置全局result

``` xml
<global-results>
	<result name="error">/error.jsp</result>
</global-results>
```



  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505127945210.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505128855650.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505129038262.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505129064232.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505129141225.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505129169059.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505129524255.jpg
  [8]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505132432012.jpg
  [9]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505132559881.jpg
  [10]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505132639365.jpg
  [11]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505132660448.jpg
  [12]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505132883647.jpg
  [13]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505132958683.jpg