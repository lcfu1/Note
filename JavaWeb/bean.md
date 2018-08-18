# bean

目录：

- [开始](#开始)
- [测试](#测试)
- [效果](#效果)

## 开始

接着上一篇

project的目录结构如下：

![beanlist](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/beanlist.PNG)

添加以下两spring的依赖包：

```
//spring依赖包
    // https://mvnrepository.com/artifact/org.springframework/spring-tx
    compile group: 'org.springframework', name: 'spring-tx', version: '4.3.14.RELEASE'
    // https://mvnrepository.com/artifact/org.springframework/spring-webmvc
    compile group: 'org.springframework', name: 'spring-webmvc', version: '4.3.14.RELEASE'
```

在java->resources->config下新建一个applicationContext.xml

内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:tx="http://www.springframework.org/schema/tx"
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
            http://www.springframework.org/schema/tx
            http://www.springframework.org/schema/tx/spring-tx.xsd
            http://www.springframework.org/schema/aop
            http://www.springframework.org/schema/aop/spring-aop.xsd ">

    <!--利用默认构造，指定属性进行bean的生成-->
    <!--要求属性名称正确，且要有setter方法-->
    <bean id="user" class="dto.User">
        <property name="id" value="1"/>
        <property name="username" value="2"/>
        <property name="password" value="3"/>
    </bean>

    <!--自定义构造函数，指定属性进行bean的生成-->
    <bean id="user1" class="dto.User">
        <constructor-arg index="0" value="4"/>
        <constructor-arg index="1" value="5"/>
        <constructor-arg index="2" value="6"/>
    </bean>

    <bean id="userDao" class="dao.impl.UserDaoImpl">
    </bean>

    <bean id="userService" class="service.impl.UserServiceImpl">
        <property name="udi" ref="userDao"/>
    </bean>

</beans>
```

因为指定属性进行bean的生成，要求属性名称正确，且要有setter方法，所以还要修改一下User.java，如下：

```
package dto;

public class User {
    private int id;
    private String username;
    private String password;

    public User(){
    }

    public User(int id, String username, String password) {
        this.id = id;
        this.username = username;
        this.password = password;
    }

    //右键->Generate->Getter and Setter
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    //右键->Generate->toString()
    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
```

还要修改一下UserServiceImpl.java，为UserDaoImpl加上setter方法，如下：

```
package service.impl;

import dao.impl.UserDaoImpl;
import dto.User;
import service.IBaseService;

import java.sql.SQLException;
import java.util.List;
import java.util.Map;

//服务层实现
public class UserServiceImpl implements IBaseService<User>{
//    持久层对象
    UserDaoImpl udi;

    public void setUdi(UserDaoImpl udi) {
        this.udi = udi;
    }

    @Override
    public boolean add(User user) throws SQLException {
        return udi.add(user);
    }

    @Override
    public boolean update(User user) throws SQLException {
        return udi.update(user);
    }

    @Override
    public boolean delete(User user) throws SQLException {
        return udi.delete(user);
    }

    @Override
    public User findById(int id) throws SQLException {
        return udi.findById(id);
    }

    @Override
    public List<User> findByProp(Map map) throws SQLException {
        return udi.findByProp(map);
    }
}
```

## 测试

测试代码如下：

test/java/TestSpring.java

```
import dto.User;
import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import service.impl.UserServiceImpl;

import java.sql.SQLException;

public class TestSpring {
    //定义一个spring容器
    ApplicationContext ac;

    //得到容器上下文
    @Before
    public void init() {
        ac = new ClassPathXmlApplicationContext("config/applicationContext.xml");
    }

    @Test
    public void test() throws SQLException {
        //将bean注入到此处
        User user = ac.getBean("user", User.class);
        System.out.println(user);

        User user1 = ac.getBean("user1", User.class);
        System.out.println(user1);

        UserServiceImpl usi = ac.getBean("userService", UserServiceImpl.class);
        System.out.println(usi);
    }
}
```

## 效果

看到如下结果则说明有生成bean。

![bean](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/bean.PNG)