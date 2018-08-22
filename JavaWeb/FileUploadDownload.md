# 上传下载文件

目录：

- [开始](#开始)
- [配置Tomcat虚拟存储路径](#配置tomcat虚拟存储路径)
- [测试](#测试)

## 开始

接着上一篇，添加需要的依赖包：

```
compile group: 'commons-io', name: 'commons-io', version: '2.4'
compile group: 'commons-fileupload', name: 'commons-fileupload', version: '1.3.1'
// https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api
compile group: 'javax.servlet', name: 'javax.servlet-api', version: '3.1.0'
```

在java/com.java.controller下新建一个FileController.java，内容如下：

```
package com.java.controller;

import org.apache.commons.io.FileUtils;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.multipart.MultipartFile;

import javax.servlet.http.HttpServletRequest;
import java.io.File;
import java.net.URLEncoder;
import java.util.List;
import java.util.UUID;

@Controller
public class FileController {
    @RequestMapping("/toFileUpload")
    public String toFileUpload(){
        return "/fileUpload";
    }

    @RequestMapping("/fileUpload")
    public String handleFormUpload(@RequestParam("name") String name,
                                   @RequestParam("uploadfile") List<MultipartFile> uploadfile,
                                   HttpServletRequest request) {
        // 判断所上传文件是否存在
        if (!uploadfile.isEmpty() && uploadfile.size() > 0) {
            //循环输出上传的文件
            for (MultipartFile file : uploadfile) {
                // 获取上传文件的原始名称
                String originalFilename = file.getOriginalFilename();
                // 设置上传文件的保存地址目录
                String dirPath ="D:/upload/";
                File filePath = new File(dirPath);
                // 如果保存文件的地址不存在，就先创建目录
                if (!filePath.exists()) {
                    filePath.mkdirs();
                }
                // 使用UUID重新命名上传的文件名称(上传人_uuid_原始文件名称)
                String newFilename = name+ "_"+ UUID.randomUUID() +
                        "_"+originalFilename;
                try {
                    // 使用MultipartFile接口的方法完成文件上传到指定位置
                    file.transferTo(new File(dirPath + newFilename));
                } catch (Exception e) {
                    e.printStackTrace();
                    return "/error";
                }
            }
            // 跳转到成功页面
            return "/success";
        }else{
            return "/error";
        }
    }

    @RequestMapping("/toDownload")
    public String toDownload(){
        return "/download";
    }
    @RequestMapping("/download")
    public ResponseEntity<byte[]> fileDownload(HttpServletRequest request,
                                               String filename) throws Exception{
        // 指定要下载的文件所在路径
        String path ="D:/upload/";
        // 创建该文件对象
        File file = new File(path+File.separator+filename);
        // 对文件名编码，防止中文文件乱码
        filename = this.getFilename(request, filename);
        // 设置响应头
        HttpHeaders headers = new HttpHeaders();
        // 通知浏览器以下载的方式打开文件
        headers.setContentDispositionFormData("attachment", filename);
        // 定义以流的形式下载返回文件数据
        headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
        // 使用Sring MVC框架的ResponseEntity对象封装返回下载数据
        return new ResponseEntity<byte[]>(FileUtils.readFileToByteArray(file),
                headers, HttpStatus.OK);
    }
    /**
     * 根据浏览器的不同进行编码设置，返回编码后的文件名
     */
    public String getFilename(HttpServletRequest request,
                              String filename) throws Exception {
        // IE不同版本User-Agent中出现的关键词
        String[] IEBrowserKeyWords = {"MSIE", "Trident", "Edge"};
        // 获取请求头代理信息
        String userAgent = request.getHeader("User-Agent");
        for (String keyWord : IEBrowserKeyWords) {
            if (userAgent.contains(keyWord)) {
                //IE内核浏览器，统一为UTF-8编码显示
                return URLEncoder.encode(filename, "UTF-8");
            }
        }
        //火狐等其它浏览器统一为ISO-8859-1编码显示
        return new String(filename.getBytes("UTF-8"), "ISO-8859-1");
    }
}
```

修改resources->config->springmvc.xml，添加上文件上传需要的配置，如下：

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
    <context:component-scan base-package="com.java"/>
    <!--注解驱动配置-->
    <mvc:annotation-driven>
        <mvc:message-converters>
            <!--避免json数据传输乱码-->
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <!--<property name="supportedMediaTypes" value="text/html;charset=UTF-8"/>-->
                <property name="supportedMediaTypes">
                    <list>
                        <value>text/html;charset=UTF-8</value>
                        <value>application/json;charset=UTF-8</value>
                    </list>
                </property>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
    <!--视图解析器配置-->
    <bean id="viewResol" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀属性，/WEB-INF/views/cgi-->
        <property name="prefix" value="/WEB-INF/views"/>
        <!--后缀属性-->
        <property name="suffix" value=".jsp"/>
    </bean>

    <!--文件上传-->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!--设置文件上传编码-->
        <property name="defaultEncoding" value="UTF-8"/>
        <!--设置允许上传文件的最大值(2M)）-->
        <property name="maxUploadSize" value="2097152"/>
        <!--设置缓存中的最大值(2M)）-->
        <property name="maxInMemorySize" value="2097152"/>
    </bean>
</beans>
```

在webapp->WEB-INF->views下新建fileUpload.jsp、download.jsp、success.jsp、error.jsp，如下：

fileUpload.jsp

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>文件上传</title>
    <script>
        function check() {
            //通过标签的id属性获取属性值
            var name = document.getElementById("name").value;
            var file = document.getElementById("file").value;
            if (name == "") {
                alert("上传人未填写");
                return false;
            }
            if (file.length == 0 || file == "") {
                alert("请选择上传文件");
                return false;
            }
            return true;
        }
    </script>
</head>
<body>
<form action="${pageContext.request.contextPath }/fileUpload"
      method="post" enctype="multipart/form-data" onsubmit="return check()">
    上传人：<input id="name" type="text" name="name"/><br/><br/>
    请选择文件：<input id="file" type="file" name="uploadfile"
                 multiple="multiple"/><br/><br/>
    <input type="submit" value="上传"/>
</form>
</body>
</html>
```

download.jsp

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8" %>
<%@page import="java.net.URLEncoder" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>下载页面</title>
</head>
<body>
<%-- <a href="${pageContext.request.contextPath }/download?filename=1.jpg">
    文件下载
</a> --%>

<a href="${pageContext.request.contextPath }/download?filename=<%=
                                   URLEncoder.encode("壁纸.jpg", "UTF-8")%>">
    中文名称文件下载
</a>
</body>
</html>
```

success.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
success
  </body>
</html>
```

error.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
error
  </body>
</html>
```

## 配置Tomcat虚拟存储路径

这里我是在电脑D盘新建一个叫upload的文件夹（可自行新建取名，但使用的时候要用自己新建那个）。

如下图点击进入：

![tomcat1](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/tomcat1.PNG)

然后点击+，选择External  Source...，选择上面我们新建的upload文件夹并确定，还要填写右边Application context，这里我填了/upload，如下：

![tomcat2](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/tomcat2.PNG)

## 测试

**图片上传测试**

在浏览器中输入这个链接：http://localhost:8080/toFileUpload，效果如下：

![fileupload](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/fileupload.PNG)

**图片下载测试**

在浏览器中输入这个链接：http://localhost:8080/toDownload，效果如下：

![filedownload](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/filedownload.PNG)