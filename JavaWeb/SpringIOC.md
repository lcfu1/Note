# Spring IOC

目录：

- [开始](#开始)
- [测试](#测试)
- [项目链接](#项目链接)

## 开始

接着上一篇

project的目录结构修改如下：

![springioclist](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/springioclist.PNG)

首先在build.gradle中添加spring的测试依赖包，如下：

```
testCompile group: 'org.springframework', name: 'spring-test', version: '4.3.14.RELEASE'
```

修改java/resources/config/applicationContext.xml，如下：

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
    <!--自动扫描-->
    <context:component-scan base-package="com.java"/>
    <!--&lt;!&ndash;利用默认构造，指定属性进行bean的生成&ndash;&gt;-->
    <!--&lt;!&ndash;要求属性名称正确，且要有setter方法&ndash;&gt;-->
    <!--<bean id="user" class="com.java.dto.User">-->
    <!--<property name="id" value="1"/>-->
    <!--<property name="username" value="2"/>-->
    <!--<property name="password" value="3"/>-->
    <!--</bean>-->

    <!--&lt;!&ndash;自定义构造函数，指定属性进行bean的生成&ndash;&gt;-->
    <!--<bean id="user1" class="com.java.dto.User">-->
    <!--<constructor-arg index="0" value="4"/>-->
    <!--<constructor-arg index="1" value="5"/>-->
    <!--<constructor-arg index="2" value="6"/>-->
    <!--</bean>-->

    <!--<bean id="userDao" class="com.java.dao.impl.UserDaoImpl">-->
    <!--</bean>-->

    <!--<bean id="userService" class="com.java.service.impl.UserServiceImpl">-->
    <!--<property name="udi" ref="userDao"/>-->
    <!--</bean>-->

</beans>
```

修改java/com.java/dao/impl/UserDaoImpl.java，加上@Repository，如下：

```
package com.java.dao.impl;

import com.java.dao.Db;
import com.java.dao.IBaseDao;
import com.java.dto.User;
import org.springframework.stereotype.Repository;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

//持久层的实现
@Repository
public class UserDaoImpl implements IBaseDao<User>{
//    获取连接
    Connection conn= Db.getConn();
//    处理类
    PreparedStatement ps;
//    记录集类
    ResultSet rs;

//    增加
    @Override
    public boolean add(User user) throws SQLException {
//        定义要执行的sql语句
        String sql="INSERT INTO user (username,password) VALUE(?,?)";
//        在连接基础上创建执行
        ps=conn.prepareStatement(sql);
//        植入参数
        ps.setString(1,user.getUsername());
        ps.setString(2,user.getPassword());
//        执行更新操作，并返回受影响的记录数
        int i=ps.executeUpdate();
//        将返回的整型值处理成要返回的逻辑值
        return i>0?true:false;
    }

//    修改
    @Override
    public boolean update(User user) throws SQLException {
        String sql="UPDATE user SET username=?,password=? WHERE id=?";
        ps=conn.prepareStatement(sql);
        ps.setString(1,user.getUsername());
        ps.setString(2,user.getPassword());
        ps.setInt(3,user.getId());
        int i=ps.executeUpdate();
        return i>0?true:false;
    }

//    删除
    @Override
    public boolean delete(User user) throws SQLException {
        String sql="DELETE FROM user WHERE id=?";
        ps=conn.prepareStatement(sql);
        ps.setInt(1,user.getId());
        int i=ps.executeUpdate();
        return i>0?true:false;
    }

//    根据id查找
    @Override
    public User findById(int id) throws SQLException {
        User user=null;
        String sql="SELECT * FROM user WHERE id=?";
        ps=conn.prepareStatement(sql);
        ps.setInt(1,id);
        rs=ps.executeQuery();
        if(rs.next()){
            user=new User();
            user.setId(rs.getInt("id"));
            user.setUsername(rs.getString("username"));
            user.setPassword(rs.getString("password"));
        }
        return user;
    }

//    模糊查询
    @Override
    public List<User> findByProp(Map map) throws SQLException {
        List<User> userList=new ArrayList<User>();
        String sql="SELECT * FROM user WHERE id LIKE '%"+map.get("id")+"%'";
        ps=conn.prepareStatement(sql);
        rs=ps.executeQuery();
        while (rs.next()){
            User user=new User();
            user.setId(rs.getInt("id"));
            user.setUsername(rs.getString("username"));
            user.setPassword(rs.getString("password"));
            userList.add(user);
        }
        return userList;
    }
}
```

还要修改java/service/impl/UserServiceImpl.java，加上@Service和@Autowired，如下：

```
package com.java.service.impl;

import com.java.dao.impl.UserDaoImpl;
import com.java.dto.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.java.service.IBaseService;

import java.sql.SQLException;
import java.util.List;
import java.util.Map;

//服务层实现
@Service
public class UserServiceImpl implements IBaseService<User>{
//    持久层对象
    @Autowired
    UserDaoImpl udi;

//    public void setUdi(UserDaoImpl udi) {
//        this.udi = udi;
//    }

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

完成上面的操作，现在就可以来测试一下bean有没有自动注入了，如下：

test/java/TestSpringIOC.java

```
import com.java.dao.impl.UserDaoImpl;
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import com.java.service.impl.UserServiceImpl;

import java.sql.SQLException;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:config/applicationContext.xml")
public class TestSpringIOC {

    @Autowired
    UserDaoImpl udi;
    @Autowired
    UserServiceImpl usi;

    @Test
    public void test() throws SQLException {
        //断言非空，则成功
        Assert.assertNotNull(udi);
        Assert.assertNotNull(usi);
        //断言"li"和usi.findById(1).getUsername()的值相等，相等则成功
        Assert.assertEquals("li",usi.findById(1).getUsername());
    }
}
```

点击test()方法左侧的Run 'test()'进行测试，如果绿色不报错，则有生成相应的bean了。

## 项目链接

- [https://github.com/lcfu1/SpringTest](https://github.com/lcfu1/SpringTest)