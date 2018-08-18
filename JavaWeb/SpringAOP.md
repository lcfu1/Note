# Spring AOP

## 开始

接着上一篇

project的目录结构修改如下：

![springaoplist](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/springaoplist.PNG)

首先在build.gradle中先添加Spring AOP需要的依赖包，如下：

```
//切面需要的依赖包
    compile group: 'org.aspectj', name: 'aspectjweaver', version: '1.6.6'
    compile group: 'org.aspectj', name: 'aspectjrt', version: '1.6.6'
```

在com.java下新建一个aop包，在aop包中再新建两个文件，ServiceAdvisor和UserService，如下：

UserService.java

```
package com.java.aop;

import org.springframework.stereotype.Repository;

@Repository
public class UserService {
    public void add(){
        System.out.println(this.getClass().getSimpleName()+":add");
    }
    public void add(int i){
        System.out.println(this.getClass().getSimpleName()+":"+i);
    }
    public void findById(){
        System.out.println(this.getClass().getSimpleName()+"findById");
    }
    public void findByProp(){
        System.out.println(this.getClass().getSimpleName()+":findByProp");
    }
    public void findByName(){
        System.out.println(this.getClass().getSimpleName()+":findByName");
    }
}
```

ServiceAdvisor.java

```
package com.java.aop;

import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Repository;

@Aspect
@Repository
public class ServiceAdvisor {
    /**
     * pointcut
     * 声明一个切入点
     * 切入点是一个特殊方法，没有返回值，没有入参，没有实现
     * 在execution中说明，切入的方法有什么特点，*指方法返回类型。
     */

    //* 在add前面表示任意返回类型，add(..)中的..指参数也任意
    @Pointcut("execution(* add(..))")
    private void addPointcut() {}

    //advice通知
    @Before(value = "addPointcut()")
    public void doSthBefore(){
        System.out.println("Before。。。");
    }

    //find*查找find开头的方法
    @Pointcut("execution(* find*(..))")
    private void findPointcut() {}

    //事后要做的事情
    @After(value = "findPointcut()")
    public void doSthAfter(){
        System.out.println("After。。。");
    }
}
```

注：

要把java/com.java/service/impl/UserServiceImpl.java中UserDaoImpl udi;上面的@Autowired暂时注释掉，不然测试时会出错，如下：

```
package com.java.service.impl;

import com.java.dto.User;
import com.java.dao.impl.UserDaoImpl;
import com.java.service.IBaseService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.sql.SQLException;
import java.util.List;
import java.util.Map;

//服务层实现
@Service
public class UserServiceImpl implements IBaseService<User>{
//    持久层对象
//    @Autowired
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

在test/java/下新建一个TestSpringAOP.java，内容如下：

```
import com.java.aop.UserService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

//spring集成junit
@RunWith(SpringJUnit4ClassRunner.class)
//用注解方式读取容器全局配置文件
@ContextConfiguration(locations = "classpath:config/applicationContext.xml")
public class TestSpringAOP {
    @Autowired
    UserService userService;

    @Test
    public void test() {
        userService.add();
        userService.add(123);
        userService.findById();
    }
}
```

## 效果

![aop](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/aop.PNG)