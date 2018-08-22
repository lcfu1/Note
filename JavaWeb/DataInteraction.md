# 数据交互

目录：

- [开始](#开始)
- [测试](#测试)
- [项目链接](#项目链接)

## 开始

接着上一个SpringMVC的项目，并添加以下依赖：

```
//json
compile group: 'com.alibaba', name: 'fastjson', version: '1.2.47'
compile group: 'com.fasterxml.jackson.core', name: 'jackson-databind', version: '2.9.5'
compile group: 'com.fasterxml.jackson.core', name: 'jackson-core', version: '2.9.5'
compile group: 'com.fasterxml.jackson.core', name: 'jackson-annotations', version: '2.9.5'
```

在main->java->com.java->controller下新建一个testController.java

```
package com.java.controller;

import com.alibaba.fastjson.JSON;
import com.java.dto.User;
import com.java.dto.UserVO;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Controller
@RequestMapping("/success")
public class testController {
    //id查询
    @RequestMapping("/id")
    public String id(Integer id) {
        System.out.println("id=" + id);
        return "/test";
    }

    //username查询
    @RequestMapping("/username")
    public String username(String username) {
        System.out.println("username=" + username);
        return "/test";
    }

    //login
    @RequestMapping("/login")
    public String LoginUser(String username, String password) {
        System.out.println("username:" + username);
        System.out.println("password:" + password);
        return "/test";
    }

    //不同名id查询
    @RequestMapping("/ID")
    public String selectID(@RequestParam(value = "user_id") Integer id) {
        System.out.println("id:" + id);
        return "/test";
    }

    //登录
    @RequestMapping("/toLogin")
    public String toLogin() {
        return "/testLogin";
    }
    @RequestMapping("/Login")
    public String LoginCustomer(User user) {
        System.out.println("username=" + user.getUsername());
        System.out.println("password=" + user.getPassword());
        return "/testLogin";
    }

    //多选择
    @RequestMapping("/toFav")
    public String toFav() {
        return "/testFav";
    }
    @RequestMapping("/fav")
    public String fan(Integer[] fav) {
        if (fav != null) {
            String s = "";
            for (Integer i : fav) {
                if (i == 1) {
                    s += "听歌、";
                } else if (i == 2) {
                    s += "打球、";
                } else if (i == 3) {
                    s += "睡觉、";
                } else if (i == 4) {
                    s += "逛街";
                }
            }
            System.out.println("喜欢的是：" + s);
        } else {
            System.out.println("没有选择对象");
        }
        return "/testFav";
    }

    //批量修改
    @RequestMapping("/toUpdate")
    public String toUpdate() {
        return "/testUpdate";
    }
    @RequestMapping("/Update")
    public String Update(UserVO userVO) {
        List<User> list = userVO.getUsers();
        for (User u : list) {
            if (u.getId() != 0) {
                System.out.println("id:" + u.getId() + "修改usename为：" + u.getUsername());
            }
        }
        return "/testUpdate";
    }

    //id查询，如/post/1
    @RequestMapping("/post/{id}")
    public String post(@PathVariable Integer id) {
        System.out.println(id);
        return "/test";
    }

    //生成json格式的数据
    @RequestMapping("/json")
    @ResponseBody
    public String json() {
        User user=new User();
        user.setId(1);
        user.setUsername("Li");
        user.setPassword("123456");
        //生成HashMap对象
        Map map=new HashMap();
        map.put("success",true);
        map.put("data",user);
        //将集合数据转换成json字符串
        return JSON.toJSONString(map);
    }
}
```

在webapp->WEB-INF->views下新建test.jsp、testFav.jsp、testLogin.jsp、testUpdate.jsp，内容分别如下：

test.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>$Title$</title>
</head>
<body>
test
</body>
</html>
```

testFav.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
  <title>$Title$</title>
</head>
<body>
<form action="/success/fav" method="post">
  <input type="checkbox" name="fav" value="1">听歌
  <input type="checkbox" name="fav" value="2">打球
  <input type="checkbox" name="fav" value="3">睡觉
  <input type="checkbox" name="fav" value="4">逛街
  <br/><input type="submit" value="确认">
</form>
</body>
</html>
```

testLogin.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <form action="/success/Login" method="post">
    username:<input type="text" name="username"><br/>
    password:<input type="text" name="password"><br/>
    <input type="submit" value="LoginCustomer">
  </form>
  </body>
</html>
```

testUpdate.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <form action="/success/Update" method="post">
    <table border="1">
      <thead>
      <tr>
        <td>修改操作</td>
        <td>数据</td>
      </tr>
      </thead>
      <tbody>
      <tr>
        <td><input type="checkbox" value="1" name="users[0].id"></td>
        <td><input type="text" value="Li" name="users[0].username"></td>
      </tr>
      <tr>
        <td><input type="checkbox" value="2" name="users[1].id"></td>
        <td><input type="text" value="Ha" name="users[1].username"></td>
      </tr>
      </tbody>
    </table>
    <input type="submit" value="修改">
  </form>
  </body>
</html>
```

## 测试

1. 在浏览器中输入这个链接：http://localhost:8080/success/id?id=1，可以在Output中看到如下输出：

   ![mvcid](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvcid.PNG)

2. 在浏览器中输入这个链接：http://localhost:8080/success/username?username=2，可以在Output中看到如下输出：

   ![mvcusername](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvcusername.PNG)

3. 在浏览器中输入这个链接：http://localhost:8080/success/login?username=1&password=2，可以在Output中看到如下输出：

   ![mvcup](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvcup.PNG)

4. 在浏览器中输入这个链接：http://localhost:8080/success/ID?user_id=1，可以在Output中看到如下输出：

   ![mvcuserid](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/mvcuserid.PNG)

5. 在浏览器中输入这个链接：http://localhost:8080/success/toLogin，可以在Output中看到如下输出：

   ![tologin](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/tologin.PNG)

6. 在浏览器中输入这个链接：http://localhost:8080/success/toFav，可以在Output中看到如下输出：

![tofav](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/tofav.PNG)

7. 在浏览器中输入这个链接：http://localhost:8080/success/toUpdate，可以在Output中看到如下输出：

![toupdate](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/toupdate.PNG)

8. 在浏览器中输入这个链接：http://localhost:8080/success/post/11，可以在Output中看到如下输出：

![post](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/post.PNG)

9. 在浏览器中输入这个链接：http://localhost:8080/success/json，可以在Output中看到如下输出：

![json](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/json.PNG)

## 项目链接

- [https://github.com/lcfu1/springMVC](#https://github.com/lcfu1/springMVC)