# lambda表达式



为何需要lambda表达式 ：

1不能用函数作为参数传入，以及不能将其作为返回值返回

2javascript是一个函数式语言，很多情况将函数作为参数。。。。。**类比lambda表达式**

## 语法

基本结构：

(param1,param2,param3...)->{

}

**其中()代表传入的接口的参数，如果没有就使用（），只有一个参数则使用参数，不用括号。**

关于函数式接口：

1如果一个接口只有一个抽象方法，那么这个接口就是函数式接口

2如果在某个接口上加上了functioninterface注解，那么这个接口就是函数式接口

3函数式接口可加Functionalinterface注解，也可以不加

例：

```java

public class test {

    public static void main(String[] args) {
//        Interface01 interface01=() ->{ };
//        System.out.println(interface01.getClass().getInterfaces()[0]);
//
//        Interface02 interface02=() ->{ };
//        System.out.println(interface02.getClass().getInterfaces()[0]);
        //对于上面两个函数式接口，通过上下文找到对应的函数式接口。跟函数式接口里面的方法名没什么关系

//Runnble是一个简单的函数式接口
      
        new Thread(()->{
            System.out.println("hello world");
        });

        List<String>list= Arrays.asList("zhansan","lisi","wangwu");

        //之前的方法将里面的元素toUpcase
//        for (String i:list
//             ) {
//            System.out.println(i.toUpperCase());
//        }

        //使用lambda表达式

//        list.forEach(item -> System.out.println(item.toUpperCase()));
//
        //把list中的元素遍历添加到list2中
        List list2=new ArrayList<>();
//
//        list.forEach(item ->list2.add(item.toUpperCase()));
//        list2.forEach(item -> System.out.println(item));

        // 使用流一步完成上面的两步操作操作
        list.stream().map(item ->list2.add(item)).forEach(item -> System.out.println(item));




    }


    public interface  Interface01{

        void test01();


    }


    //FunctionalInterface指明是一个函数式接口
    @FunctionalInterface
    public  interface Interface02{
        void test02();
    }
}

```







常见的函数式接口:

consumer

Runnble

jdk8新特性：泛型里面可以不加类名<>又叫(钻石语法)