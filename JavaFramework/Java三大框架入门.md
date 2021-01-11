**一、基本概念**

了解框架的定位可以帮助我们更好的找到学习的切入点

**1.Spring：**

- 依赖注入（DI）又称为控制反转（IOC），通常来说，当某个角色需要另一个角色才能正常运行时，通常是由调用者来创建被调用者的实例。但是在Spring框架中，创建被调用者的任务交给了Spring框架。
- 面向切片编程（AOP）是面向对象编程（OOP）的延续，AOP中代码的编写顺序不再影响代码的执行顺序，目的是解耦业务代码和公共服务代码（如日志，安全，事务等）。

**2.Struts：**

Struts的关键是M(MODEL)-V(VIEW)-C(CONTROL)

- 模型（M）：用于封装与业务逻辑相关的数据和数据处理方法。
- 视图（V）：用于数据的展现
- 控制器（C）：负责相应请求，协调Model和View

**3.Hibernate：**

Hibernate的关键是ORM，即Object Relation Mapping（对象关系映射）。

ORM 用来把对象模型表示的对象映射到基于SQL的关系模型数据库结构中去。这样使得我们在具体的操作实体对象的时候，不需要再去和复杂的SQL语句打交道，只需简单的操作实体对象的属性和方法。ORM技术是在对象和关系之间提供了一条桥梁，前台的对象型数据和数据库中的关系型的数据通过这个桥梁来相互转化 。 

Hibernate 核心接口一共有5个： 

分别为:Session、 SessionFactory、Transaction、Query和Configuration。

- Session：负责执行被持久化对象的CRUD操作(CRUD的任务是完成与 数据库的交流，包含了很多常见的SQL语句。（非线程安全）
- SessionFactory：负责初始化Hibernate。它充当数据存储源的代理，并负责创建Session对象。（非轻量级）
- Query：负责执行各种数据库查询。它可以使用HQL语言或SQL语句两种表达方式。
- Transaction：负责事务相关的操作。它是可选的，开发人员也可以设计编写自己的底层事务处理代码。
- Configuration：负责配置并启动Hibernate，创建SessionFactory对象。

**二、如何学习**

以下我从网上搜集了一些学习框架的地址，记录下来备用：

Spring：要学习Spring框架的技术内幕，必须事先掌握一些基本的Java知识，正所谓“登高必自卑，涉远必自迩”。以下几项Java知识和Spring框架息息相关，不可不学 

[1]Java反射知识–>Spring IoC :http://www.iteye.com/topic/1123081 

[2]Java动态代理–>Spring AOP :http://www.iteye.com/topic/1123293 

[3]属性编辑器，即PropertyEditor–>Spring IoC:http://www.iteye.com/topic/1123628 

[4]XML基础知识–>Spring配置:http://www.iteye.com/topic/1123630 

[5]注解–>Spring配置:http://www.iteye.com/topic/1123823 

[6]线程本地变更,即ThreadLocal–>Spring事务管理:http://www.iteye.com/topic/1123824 

[7]事务基础知识–>Spring事务管理:http://www.iteye.com/topic/1124043 

[8]国际化信息–>MVC:http://www.iteye.com/topic/1124044 

[9]HTTP报文–>MVC:http://www.iteye.com/topic/1124408

实验楼上的三门课程： 

Struts框架教程 https://www.shiyanlou.com/courses/32 

Hibernate框架教程 https://www.shiyanlou.com/courses/34 

Spring框架入门教程 https://www.shiyanlou.com/courses/578