

# springboot与rabbitmq #

## **测试rabbitmqtemplate**



````java

@SpringBootTest
class RabbitmqApplicationTests {

    @Autowired
    RabbitTemplate rabbitTemplate;
    @Test
    void contextLoads() {
        //测试传一个对象消息（点对点）
       // rabbitTemplate.convertAndSend("exchange.direct","atguigu.news",new book("第一本书",1));
        // 测试广播交换器
       // rabbitTemplate.convertAndSend("exchange.fanout","",new book("第二本书",1));
        //测试topic交换器
        rabbitTemplate.convertAndSend("exchange.topic","a.news",new book("第三本书",1));
        

    }

    @Test
    public void jj(){
       // rabbitTemplate.convertAndSend("exchange.direct","atguigu.news",new book("第一本书",1));


       //rabbitTemplate.send("exchange.direct","atguigu.news",new book("第一本书",1));
//注意不能
        //接受数据,如何将数据自动的转为json发送出去
       book o = (book)rabbitTemplate.receiveAndConvert("atguigu.news");
        System.out.println(o);
    }

}

````

```java
rabbitTemplate.convertAndSend("exchange.topic","a.news",new book("第三本书",1));//作用：将object发送并自动序列化消息。
//rabbitTemplate默认使用SimpleMessageConverter转换器，看源码可以知道，修改默认的转换器（设置转换为json格式）可以去网上看详解。
```



## //springboot使用Rabbitmq

1.引入依赖，配置apllication配置文件，Redis的地址和密码账号。

2在主程序上添加@EnableRabbitmq

3

```java
/**
 * 自动配置
 *  1、RabbitAutoConfiguration
 *  2、有自动配置了连接工厂ConnectionFactory；
 *  3、RabbitProperties 封装了 RabbitMQ的配置
 *  4、 RabbitTemplate ：给RabbitMQ发送和接受消息；
 *  5、 AmqpAdmin ： RabbitMQ系统管理功能组件;
 *     AmqpAdmin：创建和删除 Queue，Exchange，Binding
 *  6、@EnableRabbit +  @RabbitListener 监听消息队列的内容
 *
 */
@EnableRabbit
@SpringBootApplication
public class RabbitmqApplication {

    public static void main(String[] args) {
        SpringApplication.run(RabbitmqApplication.class, args);
    }

}
```



3编写service使用@RabbitListener 加方法上这样就能监听队列了

```java

@Service
public class BookService {
  
  //当队列收到消息就会掉用该方法
    @RabbitListener(queues = "atguigu.news")
    public void receive(Book book){
        System.out.println("收到消息："+book);
    }


}

```

**测试amqadmin**

````java

    @Test//测试amqpadmin创建交换器和队列并绑定
    public void createExchange(){

		//amqpAdmin.declareExchange(new DirectExchange("amqpadmin.exchange"));
		//System.out.println("创建完成");
	//	amqpAdmin.declareQueue(new Queue("amqpadmin.queue",true));

		//amqpAdmin.declareQueue(new Queue("amqpadmin.queue",true));
     //   创建绑定规则

		amqpAdmin.declareBinding(new Binding("amqpadmin.queue", Binding.DestinationType.QUEUE,"amqpadmin.exchange","amqp.haha",null));


    }
````

![屏幕快照 2020-07-19 16.11.35](/Users/mac/Desktop/mytextworkspace/屏幕快照 2020-07-19 16.11.35.png)

绑定过程如上：