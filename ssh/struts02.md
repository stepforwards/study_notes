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

# 











  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505127945210.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505128855650.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505129038262.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505129064232.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505129141225.jpg
  [6]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505129169059.jpg
  [7]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505129524255.jpg