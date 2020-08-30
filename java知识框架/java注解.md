# java注解

@target用于指定该注解修饰的对应类，

@Inteface相当于实现了Annotation，表明这是一个注解

@Document 用于说明javadoc,可有可无,

@Retention(RetentionPolicy.Runtime):

Retention是一个策略

RetentionPolicy.Runtime 表示这个注解会存在于.，会编译成.class文件,并在程序运行的时候存在。

java常见的注解

@SuppressWarnings 

@Deprecated 

 @Override

上面三个都会对java进行一个编译检查，编译之前进行检查，



(菜鸟教程)[https://www.runoob.com/w3cnote/java-annotation.html]

````java

@Retention(RetentionPolicy.RUNTIME)//表示这个注解会一直存在，会存在于.class文件中
@Documented//暂时不知道有啥用
@Target(ElementType.TYPE)//Type表示可以作用于接口，类中，ElementType是枚举类型，可以点进@Target这个里面去看看
//@interface表示他是一个注解，相当于实现了Annocation接口
public @interface Zhujie {

}
````



```java
@Zhujie
class jj{
  
  
}
```

