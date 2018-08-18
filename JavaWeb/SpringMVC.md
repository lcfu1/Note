# Spring MVC

目录：

- [开始](#开始)
- [添加web.xml](#添加web.xml)
- [在IDEA中配置Tomcat](#在idea中配置tomcat)
- [Project目录结构](#project目录结构)
- [效果](#效果)
- [项目链接](#项目链接)

## 开始

先新建一个项目，这里就命名为springMVC。

在build.gradle中添加以下依赖：

```
//springMVC包
compile group: 'org.springframework', name: 'spring-webmvc', version: '4.3.14.RELEASE'
```

在java/com.java.controller下新建一个UserController.java，内容如下：

```
package com.java.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

//控制层 @controller控制层组件，被扫描后会注册成bean
@Controller
@RequestMapping("/sucess")
public class UserController {
    //请求映射
    @RequestMapping("/add")
    public String add(){
        //视图信息，根据视图解析规则拼接具体页面信息
        return "/add";//     /WEB-INF/views/add.jsp
    }

    @RequestMapping("/update")
    public String update(){
        return "/update";
    }

    @RequestMapping("/delete")
    public String delete(){
        return "/delete";
    }

    @RequestMapping("/findById")
    public String findById(){
        return "/findById";
    }

    @RequestMapping("/findByProp")
    public String findByProp(){
        return "/findByProp";
    }
}
```

在resources/config下新建一个springmvc.xml，内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/mvc
            http://www.springframework.org/schema/mvc/spring-mvc.xsd
            http://www.springframework.org/schema/aop
            http://www.springframework.org/schema/aop/spring-aop.xsd ">
    <!--springmvc的扫描包，识别@Controller，生成bean-->
    <context:component-scan base-package="com.java.controller"/>
    <!--注解驱动配置-->
    <mvc:annotation-driven/>
    <!--视图解析器配置-->
    <bean id="viewResol" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀属性，/WEB-INF/views-->
        <property name="prefix" value="/WEB-INF/views"/>
        <!--后缀属性-->
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

## 添加web.xml

在webapp/WEB-INF下添加web.xml，操作如下：

步骤1：

![mvc7](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvc7.PNG)

步骤2：

![mvc8](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvc8.PNG)

步骤3：

![mvc9](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvc9.PNG)

在webapp/WEB-INF下新建一个views的包，然后在views包下新建add.jsp、delete.jsp、update.jsp、findById.jsp、findByProp.java，如下：

add.jsp

```
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2018/8/18 0018
  Time: 下午 8:57
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
add
  </body>
</html>
```

因为现在只是想实现页面的访问，里面暂时不需要其他内容。

## 在IDEA中配置Tomcat

在IDEA中配置Tomcat，使该项目可以在浏览器中展示，配置步骤如下：

步骤1：

![mvc1](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvc1.PNG)

步骤2：

![mvc2](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvc2.PNG)

步骤3：

![mvc3](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvc3.PNG)

步骤4：

![mvc4](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvc4.PNG)

步骤5：

![mvc5](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvc5.PNG)

步骤6：

![mvc6](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvc6.PNG)

## Project目录结构

整个project的目录结构如下：

![mvc](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvc.PNG)

## 效果

![mvc10](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvc10.PNG)

## 项目链接

- [https://github.com/lcfu1/springMVC](#https://github.com/lcfu1/springMVC)

