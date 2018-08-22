# MyBatis

目录：

- [开始](#开始)
- [测试](#测试)

## 开始

先看一下目录结构：

![mybatis1](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mybatis1.PNG)

新建一个mybatis的项目：

![mybatis](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mybatis.PNG)

在build.gradle中添加以下依赖：

```
//mysql
compile group: 'mysql', name: 'mysql-connector-java', version: '5.1.6'
//mybatis
compile group: 'org.mybatis', name: 'mybatis', version: '3.4.1'
```

在main->resources下新建一个config文件夹，在config下新建一个db.properties文件，内容如下：

```
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/test
username=root
password=Aa111111
```

用的是mysql，在navicat中新建一个test数据库，再建一个user表，如下：

![user](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/user.PNG)

在main->java下新建一个dao包，在dao下新建一个Db.java，内容如下：

```
package dao;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

//数据库连接公共类
public class Db {
    // 类成员变量
    private static SqlSession sqlSession;

    //私有构造
    private Db() {
    }

    //静态代码块
    static {
        String res = "config/mybatis.xml";
        try {
            //以文件流方式读取配置信息
            InputStream inputStream = Resources.getResourceAsStream(res);
            //用SessionFactoryBuilder方式构建SqlSessionFactory
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
            //从工厂中获取会话连接
            sqlSession = sqlSessionFactory.openSession();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //返回连接
    public static SqlSession getSqlSession() {
        return sqlSession;
    }

}
```

在dao下新建一个IBaseDao.java，如下：

```
package dao;

import java.util.List;
import java.util.Map;

//持久层通用接口
public interface IBaseDao<T> {
    boolean add(T t);
    boolean update(T t);
    boolean delete(T t);
    T findOneById(String id);
    T findOneByProp(Map map);
    List<T> findByProp(Map map);
}
```

在main->java下新建一个dto包，在dto下新建一个User.java，如下：

```
package Dto;

public class User {
    int id;
    String username;
    String password;

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

    @Override
    public String toString() {
        return "Dto.User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
```

在dao包下再新建一个IUserDao.java，如下：

```
package dao;

import Dto.User;

public interface IUserDao extends IBaseDao<User> {
}
```

在resources->config下新建一个mybatis.xml文件，如下：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--全局配置-->
<configuration>
    <!--加载数据库属性配置文件，将数据库连接信息解耦-->
    <properties resource="config/db.properties"/>
    <!--环境配置-->
    <environments default="development">
        <environment id="development">
            <!--事务处理配置-->
            <transactionManager type="JDBC"/>
            <!--数据连接配置，POOLED连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--映射-->
    <mappers>
        <!--mapper文件-->
        <mapper resource="mapper/userMapper.xml"/>
    </mappers>
</configuration>
```

还要在resources下新建一个mapper文件夹，在mapper下新建一个userMapper.xml，内容如下：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace对应接口文件的全限定名，要实现哪个文件功能-->
<mapper namespace="dao.IUserDao">
    <!--抽出的共用sql-->
    <sql id="fields">
        *
    </sql>
    <sql id="tablename">
        user
    </sql>
    <!--要实现的具体功能用具体的标签来标注-->
    <!--id指明功能名称或方法名称,parameterType入参类型-->
    <!--User findOneById(string id)-->
    <!--resultType返回类型-->
    <select id="findOneById" parameterType="java.lang.Integer" resultType="dto.User">
--         具体的sql语句的实现
        SELECT
        <include refid="fields"/>
        FROM
        <include refid="tablename"/>
        WHERE id = #{id}
    </select>

</mapper>
```

## 测试

在test->java下新建一个TestSession.java，如下：

```
import dao.Db;
import dto.User;
import org.apache.ibatis.session.SqlSession;
import org.junit.Assert;
import org.junit.Test;

public class TestSession {
    SqlSession sqlSession=Db.getSqlSession();
    @Test
    public void testSe(){
        //断言返回非空
        Assert.assertNotNull(sqlSession);
    }
    @Test
    public void testFindOneById(){
        User user=sqlSession.selectOne("dao.IUserDao.findOneById",1);
        System.out.println(user);
    }
}
```

如果没报错则ok了。