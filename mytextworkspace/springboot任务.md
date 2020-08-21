[TOP]

# Springboot任务

## 一：异步任务：

### 使用步骤：

1在主程序上配置@EnableAsync

2使用@Async注解将其配置成异步任务

```java
@Service
public class HelloService {
    @Async
    public  void aa() throws InterruptedException {
        Thread.sleep(10000);
        System.out.println("异步任务启动了");
    }
}
```

3controller调用service。



## 二：定时任务

使用步骤：

1：@EnableScheduling配置到主程序上：允许定时任务

2:配置方法的cron表达式子例如：@Scheduled(cron = "12 * * * * MON-FRI")

```java

    /**
     * second(秒), minute（分）, hour（时）, day of month（日）, month（月）, day of week（周几）.
     * 0 * * * * MON-FRI
     *  【0 0/5 14,18 * * ?】 每天14点整，和18点整，每隔5分钟执行一次
     *  【0 15 10 ? * 1-6】 每个月的周一至周六10:15分执行一次
     *  【0 0 2 ? * 6L】每个月的最后一个周六凌晨2点执行一次
     *  【0 0 2 LW * ?】每个月的最后一个工作日凌晨2点执行一次
     *  【0 0 2-4 ? * 1#1】每个月的第一个周一凌晨2点到4点期间，每个整点都执行一次；
     */
    // @Scheduled(cron = "0 * * * * MON-SAT")
    //@Scheduled(cron = "0,1,2,3,4 * * * * MON-SAT")
    // @Scheduled(cron = "0-4 * * * * MON-SAT")
    @Scheduled(cron = "12 * * * * MON-FRI")
//    @Scheduled(cron = "0/4 * * * * MON-SAT")  //每4秒执行一次
    public void hello(){
        System.out.println("hello ... ");
    }
```



## 三：使用邮箱

1引入**<artifactId>spring-boot-starter-mail</artifactId>**，配置application.properties

```properties
spring.mail.username=2289871369@qq.com
spring.mail.password=wummhaesddixdjje
spring.mail.host=smtp.qq.com
spring.mail.properties.mail.smtp.ssl.enable=true

```



2

```java


@SpringBootTest
class TaskApplicationTests {

    @Autowired
    JavaMailSenderImpl mailSender;
    @Test//发送一个简单的消息
    void contextLoads() {

        SimpleMailMessage message = new SimpleMailMessage();

        //邮件设置
        message.setSubject("通知-今晚开会");
        message.setText("今晚7:30开会");

        message.setTo("chenjun1201@foxmail.com");
        message.setFrom("2289871369@qq.com");

        mailSender.send(message);
    }
  
  //发送一个消息加附件

    @Test
    public void test02() throws  Exception{
        //1、创建一个复杂的消息邮件
        MimeMessage mimeMessage = mailSender.createMimeMessage();
        MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);

        //邮件设置
        helper.setSubject("通知-今晚开会");
        helper.setText("<b style='color:red'>今天 7:30 开会</b>",true);

        helper.setTo("chenjun1201@foxmail.com");
        helper.setFrom("2289871369@qq.com");

        //上传文件
        helper.addAttachment("1.jpg",new File("/Users/mac/Downloads/1.jpg"));
      //  helper.addAttachment("2.jpg",new File("C:\\Users\\lfy\\Pictures\\Saved Pictures\\2.jpg"));

        mailSender.send(mimeMessage);

    }

}
```

