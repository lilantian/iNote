spring-boot 启动时候 出现异常：The bean 'xxx' could not be injected as a 'xx.xxxx' because it is a JDK dynami...
---

The bean 'xxxService' could not be injected as a 'AaaXxxService' because it is a JDK dynamic proxy that implements: 

Action:  

Consider injecting the bean as one of its interfaces or forcing the use of CGLib-based proxies by setting proxyTargetClass=true on @EnableAsync and/or @EnableCaching.

原因为把@Autowired换成了@Resource

主要问题是名字和类名不一致导致Resource注入失败，但是刚好又有一个xxxService同名的类存在，就会报这个错误。

@Resource 是根据名字找对象，名字是什么的名字呢？

先根据变量名字找，如果找到就直接使用，如果找不到就才根据类的名字找。

这个时候刚好有一个同名的对象存在，直接返回赋值，但是这个时候类型和对象是不一致的，所以就报异常。

解决方法就是 名字改成和类一样

```
@Resource
private AaaXxxService aaaXxxService;
```

[注解@Resource与@Autowired的区别](https://blog.csdn.net/balsamspear/article/details/87936936)
---
注意：Spring容器以name为key存储bean！这里的name可以指定，否则取首字母小写的类名。有相同的就报异常：BeanDefinitionStoreException！

@Resource
@Resource有两个常用属性name、type，所以分4种情况

1. 指定name和type：通过name找到唯一的bean，找不到抛出异常；如果type和字段类型不一致，也会抛出异常
2. 指定name：通过name找到唯一的bean，找不到抛出异常
3. 指定type：通过tpye找到唯一的bean，如果不唯一，则抛出异常：NoUniqueBeanDefinitionException
4. 都不指定：通过字段名作为key去查找，找到则赋值；找不到则再通过字段类型去查找，如果不唯一，则抛出异常：NoUniqueBeanDefinitionException

@Autowired
@Autowired只有一个属性required，默认值为true，为true时，找不到就抛异常，为false时，找不到就赋值为null

@Autowired按类型查找，如果该类型的bean不唯一，则抛出异常；可通过组合注解解决@Autowired()@Qualifier("baseDao")

- 相同点
  1. Spring都支持
  2. 都可以作用在字段和setter方法上

- 不同点
  1. Resource是JDK提供的，而Autowired是Spring提供的
  2. Resource不允许找不到bean的情况，而Autowired允许（@Autowired(required = false)）
  3. 指定name的方式不一样，@Resource(name = baseDao"),@Autowired()@Qualifier("baseDao")
  4. Resource默认通过name查找，而Autowired默认通过type查找

- 使用哪个
都差不多