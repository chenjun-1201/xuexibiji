## spring boot 检索elsticsearch

###  1使用步骤 ###

下载镜像：docker pull elsticsearch

启动elsticsearch: 

```shell
docker run -e ES_JAVA_OPS="-Xms257m -Xmx512m" -d -p 9300:9300   -p 9200:9200 --name myelsticsearch 镜像id

```





访问：http://192.168.0.8:9200







快速入门：

[参考文档](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_analytics.html)





http://192.168.0.8:9200/megacorp/employee/_search



## springboot 整合 elasticresearch  ##

1.引入相关的依赖，根据自己的spring data elasticresearch版本找到对应的elasticresearch下载。[版本对应关系](https://blog.csdn.net/TrueNight90/article/details/105943925/)

2以springboot2.1.16为例。



新建实体类Article：

搜索的对象就是文章

```java
package com.example.el.entity;

import org.springframework.data.elasticsearch.annotations.Document;

@Document(indexName = "blog",type = "article")
public class Article {
    private String name;
    private int id;
    private  String title;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }
}

```

对ES进行操作默认的接口

````java

````

