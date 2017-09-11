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





  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1505127945210.jpg