# 说说@Configuration和@Component的区别

@Configuration 中所有带 @Bean 注解的方法都会被动态代理，因此调用该方法返回的都是同一个实例。

@Component注解并没有通过 cglib 来代理@Bean 方法的调用。

第一个bean类

```java
public class Child {
    private String name = "the child";
｝
```

第二个bean类

```java
public class Woman {
 
 private String name = "the woman";
 
 private Child child;
｝
```

@Configuration和@Component

```java
@Configuration
//@Component
public class Human {
 
 @Bean
 public Woman getWomanBean() {
  Woman woman = new Woman();
  woman.setChild(getChildBean()); // 直接调用@Bean注解的方法方法getChildBean()
  return woman;
 }
 
 @Bean
 public Child getChildBean() {
  return new Child();
 }
}
```

测试类

```java
@Configuration
public class Man {
 
 @Autowired
 public Man(Woman wn, Child child) {
  System.out.println(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>");
  System.out.println(wn.getChild() == child ? "是同一个对象":"不是同一个对象");
 }
}
```

```java
@RestController
public class LogTestController {
 @Autowired
 Woman woman ;
 
 @Autowired
 Child child;
 
 @GetMapping("/log")
 public String log() {
  return woman.getChild() == child ? "是同一个对象":"不是同一个对象";
 }
}
```

使用@Configuration的输出

![configuration](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Java/configuration.jpg)

使用@Component的输出

![component](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Java/component.jpg)

#### 参考资料

[说说 @Configuration 和 @Component 的区别](https://m.toutiao.com/is/SHuHK18/)