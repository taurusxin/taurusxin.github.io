---
title: "「Maven」IDEA下 Tomcat 10 Servlet 404 问题踩坑"
categories: [ "后端技术" ]
tags: [  ]
draft: false
slug: "tomcat10-issue"
date: "2021-04-12 17:25:00"
---

### 起因
下午将 Tomcat升级到了 10.0.4 版本，导致原先的 JSP Servlet 全部报了404。
搜寻了互联网，大部分都是配置`web.xml`的`metadata-complete`为`false`，但这样对于我的项目无效。
![404][1]

### 解决方案
IDEA 建立`Java Enterprise`工程，默认的Servlet API是`javax.servlet-api`的`4.0.1`版本

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>
```

**但是！**

Tomcat 在 10.0 的版本之后，修改了所有的 Servlet API 包名
![Tomcat 10 Servlet API][2]

从原先的 javax.servlet 改成了 jakarta.servlet，虽然代码没有错误，但是 Tomcat 并不能搜寻到我们继承的类。

解决方法很简单，修改 pom.xml 中的 Servlet API

可以在 https://mvnrepository.com/artifact/org.apache.tomcat/tomcat-servlet-api 找到，注意对应你的 Tomcat 版本

```xml
<dependency>
    <groupId>org.apache.tomcat</groupId>
    <artifactId>tomcat-servlet-api</artifactId>
    <version>10.0.4</version>
</dependency>
```

然后将你实现 Servlet 接口的类的导包都改成对应的包名即可。

```java
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
```

问题解决


  [1]: https://cdn.rhyland.cn/typecho/2021/04/12/404.png
  [2]: https://cdn.rhyland.cn/typecho/2021/04/12/tomcat10-servlet-api.png