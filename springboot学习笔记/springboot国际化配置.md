#  springboot国际化配置 #

步骤：

1从html中提取需要国际化的字段。

2在resource下配置三个properties，login.properties,login_en_US,login_zh_CN.properties并将需要国际化的字段配置不同的值，写入properties中

3在html代码中使用themleaf中的#{属性名}

4即可，



**原理**

springboot默认底层帮我们配好了国际化配置，通过客户端请求头中的accept-language判断使用的什么语言，在自动转换。



扩展，根据自己需求选择不同的语言

```java
package com.example.resultfultest.util;

import org.springframework.web.servlet.LocaleResolver;
import org.thymeleaf.util.StringUtils;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Locale;

public class MylocalReasver implements LocaleResolver {
    @Override
    public Locale resolveLocale(HttpServletRequest httpServletRequest) {
        Locale locale=null;
//        Local local =null;
        String l=httpServletRequest.getParameter("l");

        if(!StringUtils.isEmpty(l)){
            String[] ss = l.split("_");
            locale=new Locale(ss[0],ss[1]);


        }else {
          //如果没有传入参数返回默认的locale（浏览器请求头方式判断）
             locale= Locale.getDefault();
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

    }
}

```





当request传入en_US时

```html
	<a class="btn btn-sm" th:href="@{/hello(l='zh_CN')}">中文</a>
			<a class="btn btn-sm" th:href="@{/hello(l='en_US')}">English</a>
```

