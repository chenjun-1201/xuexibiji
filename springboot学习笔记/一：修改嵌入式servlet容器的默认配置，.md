[TOC]





# 一：修改嵌入式servlet容器的默认配置， #



以Tomcat为例：

1：在application.properties中添加要修改的属性

2如下

```java
 @Bean
    public WebServerFactoryCustomizer<TomcatServletWebServerFactory> webServerFactoryCustomizer(){
        return new WebServerFactoryCustomizer<TomcatServletWebServerFactory>() {
            @Override
            public void customize(TomcatServletWebServerFactory factory) {
                factory.setPort(8083);
            }


        };
```



因此，针对如何修改SpringBoot的默认配置，我们有几种思想：

模式：

1）、SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（@Bean、@Component）如果有就用用户配置的，如果没有，才自动配置；如果有些组件可以有多个（ViewResolver）将用户配置的和自己默认的组合起来；

2）、在SpringBoot中会有非常多的xxxConfigurer帮助我们进行扩展配置

3）、在SpringBoot中会有很多的xxxCustomizer帮助我们进行定制配置

## 注册Servlet三大组件【Servlet、Filter、Listener】

1servlet

步骤1：

1创建一个servlet

2.在配置类中注册 servlet

```java
@Bean
    public ServletRegistrationBean myservlet(){
        return new ServletRegistrationBean(new MyServlet(),"/myservlet");

    }
```

** Filter **

1:创建自己的filter 类

2加入bean

```java
    @Bean
    public FilterRegistrationBean myfiter(){
        FilterRegistrationBean filterRegistrationBean= new FilterRegistrationBean(new Myfilter());
        List<String> list=new ArrayList<String>();
        list.add("/hello");
        filterRegistrationBean.setUrlPatterns(list);
        return filterRegistrationBean;
    }
```



1配置自己的类MyServletContextListenter

2加入ServletListenerRegistrationBean到容器

````java
   @Bean
    public ServletListenerRegistrationBean servletListenerRegistrationBeanAdapter(){
   ServletListenerRegistrationBean s=new ServletListenerRegistrationBean();
   s.setListener(new MyServletContexListen());
   return s;
    }
````

## 使用外置的servlet容器 idea带的Tomcat

 jar包:执行SpringBoot主类的main方法，启动ioc容器，创建嵌入式的Servlet容器;

war包:启动服务器，服务器启动SpringBoot应用【SpringBootServletInitializer】，启动ioc容器;