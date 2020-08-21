# Gateway

## spring cloud  网关:

所有流量的入口，常用的功能有路由转发和数据校验，等等

简单入门：

1创建一个springboot项目引入gateway,引入

```xml
<!--        注册发现-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-nacos-discovery</artifactId>
        </dependency>


   <!--        配置中心来做配置管理-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>


    </dependencies>

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

​	

2配置application.yml

```yml
spring:
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848
    gateway:
      routes:
        - id: test
          uri: https://www.baidu.com
          predicates:
            - Query=url,baidu

        - id: qq_rote
          uri: https://www.qq.com
          predicates:
            - Query=url,qq
  application:
    name: gulimail-gateway

server:
  port: 88

```

3配置bootstrap.properties

```properties
spring.cloud.nacos.server-addr=127.0.0.1:8848
spring.application.name=gulimail-gateway
#需要在nacos中配置一个命名空间
spring.cloud.nacos.discovery.group=117dc82d-9d70-4b9e-8924-eb67ebb3ddaa
```

4测试：

[测试链接](https://localhost:88/hello?url=baidu)

[测试链接2](https://localhost:88/hello?url=qq)

工作流程:

1将请求route到服务前先断言，判断是否满足条件。

2：将用户的请求发送给各个服务前会在nacos上找各个服务（必须和其他服务保持在同一个命名空间下，不然找不到，一般注册发现的服务都放到默认的名称空间下）。然后通过服务的id找到对应的服务。

