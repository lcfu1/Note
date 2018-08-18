# 增删改查

目录：

## 目录

在main->java中新建如下目录和文件：

![springlist](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/springlist.PNG)

## 数据

打开navicat新建一个连接，在test数据库中新建一个user表，如下：

![sqluser](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/sqluser.PNG)

## 代码

dto/User.java

```
package dto;

public class User {
    private int id;
    private String username;
    private String password;

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

dao/Db.java

```
package dao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.util.Properties;

//数据库连接公共类
public class Db {
    // 类成员变量
    private static Connection connection;

    // 私有构造，单例
    private Db() {
    }

    static {
        Properties p = new Properties();
        try {
            p.load(Db.class.getClassLoader().getResourceAsStream("config/db.properties"));
            //读取属性文件
            String driver = p.getProperty("driver");
            String url = p.getProperty("url");
            String username = p.getProperty("username");
            String password = p.getProperty("password");
            //加载驱动器
            Class.forName(driver);
            //用驱动管理类获得连接
            connection = DriverManager.getConnection(url, username, password);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    //返回连接
    public static Connection getConn() {
        return connection;
    }
}
```

dao/IBaseDao.java

```
package dao;

import java.sql.SQLException;
import java.util.List;
import java.util.Map;

//持久层通用接口
public interface IBaseDao<T> {
    //增加
    boolean add(T t) throws SQLException;
    //修改
    boolean update(T t) throws SQLException;
    //删除
    boolean delete(T t) throws SQLException;
    //查找
    T findById(int id) throws SQLException;
    //模糊查询
    List<T> findByProp(Map map) throws SQLException;
}
```

dao/impl/UserDaoImpl.java

```
package dao.impl;

import dao.Db;
import dao.IBaseDao;
import dto.User;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

//持久层的实现
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

serivce/IBaseService.java

```
package service;

import java.sql.SQLException;
import java.util.List;
import java.util.Map;

//服务层通用接口类
public interface IBaseService<T> {
    //增加
    boolean add(T t) throws SQLException;
    //修改
    boolean update(T t) throws SQLException;
    //删除
    boolean delete(T t) throws SQLException;
    //查找
    T findById(int id) throws SQLException;
    //模糊查询
    List<T> findByProp(Map map) throws SQLException;
}
```

service/impl/UserServiceImpl.java

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
    UserDaoImpl udi=new UserDaoImpl();
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

test/java/TestUserServiceImpl.java

```
import dto.User;
import org.junit.Test;
import service.impl.UserServiceImpl;

import java.sql.SQLException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class TestUserServiceImpl {
    UserServiceImpl usi=new UserServiceImpl();

    @Test
    public void testAdd() throws SQLException{
        User user=new User();
        user.setUsername("li");
        user.setPassword("123456");
        usi.add(user);
    }

    @Test
    public void testUpdate() throws SQLException{
        User user=usi.findById(1);
        System.out.println(user);
        user.setUsername("li");
        user.setPassword("123456");
        usi.update(user);
        user=usi.findById(1);
        System.out.println(user);
    }

    @Test
    public void testDelete() throws SQLException{
        User user=usi.findById(1);
        System.out.println(usi.delete(user));
    }

    @Test
    public void testFindById() throws SQLException{
        System.out.println(usi.findById(1));
    }

    @Test
    public void testFindByProp() throws SQLException{
        Map map=new HashMap();
        map.put("id",1);
        List<User> list = usi.findByProp(map);
        if (list.size() > 0) {
            for (User user : list) {
                System.out.println(user.toString());
            }
        } else {
            System.out.print("查不到相关数据");
        }
    }
}
```

