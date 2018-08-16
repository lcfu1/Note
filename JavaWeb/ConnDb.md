# 连接数据库

目录：

- [目录](#目录)
- [配置](#配置)
- [测试Properties](#测试properties)
- [测试连接数据库](#测试连接数据库)

## 目录

![list](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/list.PNG)

## 配置

在build.gradle中添加所需要的依赖包，如下：

```
group 'com.java'
version '1.0-SNAPSHOT'

apply plugin: 'java'
apply plugin: 'war'

sourceCompatibility = 1.8

repositories {
    mavenCentral()
    maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
}

dependencies {
//    testCompile group: 'junit', name: 'junit', version: '4.11'
    testCompile group: 'junit', name: 'junit', version: '4.12'

//    oracle
    testCompile group: 'com.oracle', name: 'ojdbc6', version: '12.1.0.1-atlassian-hosted'
//    mysql
    compile group: 'mysql', name: 'mysql-connector-java', version: '5.1.26'
}
```

在main->resources包下新建一个config包，再新建一个文件到config包下，命名为`db.properties`，内容如下：

```
##oracle
#driver=oracle.jdbc.driver.OracleDriver
#url=jdbc:oracle:thin:@127.0.0.1:1521:orcl
#username=root
#password=Aa111111

#mysql
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8
username=root
password=Aa111111
```

## 测试Properties

代码如下：

```
import org.junit.Test;

import java.io.IOException;
import java.util.Properties;

public class TestProperties {
    @Test
    public void TestP() throws IOException {
        Properties properties=new Properties();
        properties.load(TestProperties.class.getClassLoader().getResourceAsStream("config/db.properties"));
        String driver=properties.getProperty("driver");
        String url=properties.getProperty("url");
        String username=properties.getProperty("username");
        String password=properties.getProperty("password");
        System.out.println(driver);
        System.out.println(url);
        System.out.println(username);
        System.out.println(password);
    }
}
```

点击TestP()方法左侧的Run TestP()进行测试，如下：

![TestProperties](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/TestProperties.PNG)

如果能正确输出如下结果则证明ok了：

```
com.mysql.jdbc.Driver
jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8
root
Aa111111
```

## 测试连接数据库

```
import org.junit.Test;

public class TestConn {
    @Test
    public void testC(){
        //打印连接情况
        System.out.println(Db.getConn());
    }
}
```

点击testC()方法左侧的Run testC()进行测试，测试结果如下则连接成功了：

```
com.mysql.jdbc.JDBC4Connection@340f438e
```

