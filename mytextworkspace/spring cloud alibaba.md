

# spring cloud alibaba：

### 一. 为什么使用spring cloud alibaba

很多人可能会问，有了spring cloud这个微服务的框架，为什么又要使用spring cloud alibaba这个框架了，原因spring cloud alibaba 提供更简单，更方便的远程调用方案。

### 二. 注册中心Nacos

nacos是阿里巴巴研发的一个集注册中心与配置中心于一体的管理平台，使用其他非常的简单

启动nacos 参考：

有两种方式启动

[访问链接](http://localhost:8848/nacos)

其中默认的登录名和密码是：nacos/nacos



### spring cloud alibaba 快速入门：

**1将服务配置到nacos中，**

引入相关的依赖：注意springboot版本一定要和spring cloud alibaba版本匹配

[版本控制官方说明](https://github.com/alibaba/spring-cloud-alibaba/wiki/版本说明)

```xml
<!--        注册发现-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-nacos-discovery</artifactId>
        </dependency> 
<!-- 依赖的版本管理，配置后就不用写改依赖的版本-->
<dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.2.0.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```



配置application.yml

```yaml
spring:
  application:
    name: gulimail-coupon  #服务名
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848 #nacos端口

```

使用**@EnableDiscoveryClient**开启发现服务功能

```java
//开启服务注册功能
@EnableFeignClients(basePackages = "com.baidu.cj0718.gulimail_member.feign")
@EnableDiscoveryClient
@SpringBootApplication
public class GulimailMemberApplication {

    public static void main(String[] args) {
        SpringApplication.run(GulimailMemberApplication.class, args);
    }

}
```

启动服务，就会在nacos中多了一个服务的配置



**2使用openfeign进行服务间的远程调用**

引入相关的组件 openfeign

```xml
  <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>
```

例：member 服务远程调用coupon服务的一个方法

1coupon服务编写要被调用的方法

````java

@RestController
@RequestMapping("gulimail_coupon/coupon")
public class CouponController {
    @Autowired
    private CouponService couponService;

@RequestMapping("/member/list")
    public R membercoupons(){
    CouponEntity couponEntity = new CouponEntity();
    couponEntity.setCouponName("满100减10");
    return R.ok().put("coupons",Arrays.asList(couponEntity));
    }

}


````



2编写一个接口告诉springcloud这个接口需要调用远程服务功能

@FeignClient用于指定要调用的服务名，下面的方法就是原方法的无方法体结构，需要被调用的方法，注意requestmapping中的路径一定要全

```java
@FeignClient("gulimail-coupon")
public interface CouponFeignService {
    @RequestMapping("/gulimail_coupon/coupon/member/list")
    public R membercoupons();

}
```





member服务的主程序

@EnableFeignClients(basePackages = "com.baidu.cj0718.gulimail_member.feign")用于指定要扫描的接口的包，（CouponFeignService 的所在包）

```java

/**
 * 使用申明式远程调用功能
 * 1：引入openfeign
 * 
 * 3开启远程调用服务功能
 *
 */


//开启服务注册功能
@EnableFeignClients(basePackages = "com.baidu.cj0718.gulimail_member.feign")
@EnableDiscoveryClient
@SpringBootApplication
public class GulimailMemberApplication {

    public static void main(String[] args) {
        SpringApplication.run(GulimailMemberApplication.class, args);
    }

}
```

membercontroller可以直接通过直接注入 CouponFeignService调用该接口的方法。

```java
package com.baidu.cj0718.gulimail_member.controller;




/**
 * 会员
 *
 * @author chenjun
 * @email 2289871368@qq.com
 * @date 2020-07-20 14:34:26
 */
@RestController
@RequestMapping("gulimail_member/member")
public class MemberController {
    @Autowired
    private MemberService memberService;
    @Autowired
    CouponFeignService feignService;

    //测试远程方法
    @RequestMapping("/coupons")
    public R test(){
        MemberEntity memberEntity=new MemberEntity();
        memberEntity.setNickname("张三");

        R membercoupons = feignService.membercoupons();
        return membercoupons.put("member",memberEntity).put("coupons",membercoupons.get("coupons"));
    }

    

}

```



### 三 springcloud alibaba 配置中心功能



```java
/**
 * 1、如何使用Nacos作为配置中心统一管理配置
 *
 * 1）、引入依赖，
 *         <dependency>
 *             <groupId>com.alibaba.cloud</groupId>
 *             <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
 *         </dependency>
 * 2）、创建一个bootstrap.properties。
 *      spring.application.name=gulimall-coupon
 *      spring.cloud.nacos.config.server-addr=127.0.0.1:8848
 * 3）、需要给配置中心默认添加一个叫 数据集（Data Id）gulimall-coupon.properties。默认规则，应用名.properties
 * 4）、给 应用名.properties 添加任何配置
 * 5）、动态获取配置。
 *      @RefreshScope：动态获取并刷新配置
 *      @Value("${配置项的名}")：获取到配置。
 *      如果配置中心和当前应用的配置文件中都配置了相同的项，优先使用配置中心的配置。
 *
 * 2、细节
 *  1）、命名空间：配置隔离；
 *      默认：public(保留空间)；默认新增的所有配置都在public空间。
 *      1、开发，测试，生产：利用命名空间来做环境隔离。
 *         注意：在bootstrap.properties；配置上，需要使用哪个命名空间下的配置，
 *         spring.cloud.nacos.config.namespace=9de62e44-cd2a-4a82-bf5c-95878bd5e871
 *      2、每一个微服务之间互相隔离配置，每一个微服务都创建自己的命名空间，只加载自己命名空间下的所有配置
 *
 *  2）、配置集：所有的配置的集合
 *
 *  3）、配置集ID：类似文件名。
 *      Data ID：类似文件名
 *
 *  4）、配置分组：
 *      默认所有的配置集都属于：DEFAULT_GROUP；
 *      1111，618，1212
 *
 * 项目中的使用：每个微服务创建自己的命名空间，使用配置分组区分环境，dev，test，prod
 *
 * 3、同时加载多个配置集
 * 1)、微服务任何配置信息，任何配置文件都可以放在配置中心中
 * 2）、只需要在bootstrap.properties说明加载配置中心中哪些配置文件即可
 * 3）、@Value，@ConfigurationProperties。。。
 * 以前SpringBoot任何方法从配置文件中获取值，都能使用。
 * 配置中心有的优先使用配置中心中的，
 *
 *
 */
```

````properties
spring.cloud.nacos.config.server-addr=127.0.0.1:8848
spring.application.name=gulimail-coupon

#spring.cloud.nacos.config.namespace=f3d5767a-42a7-4155-85f4-b34fe9528f27
#修改命名空间和组
spring.cloud.nacos.config.namespace=4f94001d-931d-4de0-a71f-5392f35e4630
#spring.cloud.nacos.config.group=1111

#先在nacos中创建两个yml,将application.yml分成两个配置 ,datasource.yml和other.yml
#在bootstrap.properties中配置如下：可以同时读取两个配置文件的信息》
spring.cloud.nacos.config.ext-config[0].data-id=datasource.yml
spring.cloud.nacos.config.ext-config[0].group=DEFAULT_GROUP
spring.cloud.nacos.config.ext-config[0].refresh=true



spring.cloud.nacos.config.ext-config[1].data-id=other.yml
spring.cloud.nacos.config.ext-config[1].group=DEFAULT_GROUP
spring.cloud.nacos.config.ext-config[1].refresh=true
````

