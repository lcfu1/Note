# 开发环境搭建和新建项目

目录：

- [开发环境搭建](#开发环境搭建)
- [新建项目](#新建项目)

## 开发环境搭建

在电脑上安装：

- JDK1.8
- Gradle3.3
- Tomcat8
- Idea2017.2

### JDK1.8

根据软件提示，一路next安装。

在用户环境变量那配置JAVA_HOME：

![jdk1.8](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/jdk1.8.PNG)

在系统环境变量那配置path：

```
%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
```

按win+R组合键并输入cmd进入命令控制台，输入java测试有没有安装成功，如下：

![cmdjava](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/cmdjava.PNG)

### Gradle3.3

无需安装，将下载的安装包解压到指定的路径 C:\gradle-3.3，然后在系统环境变量那配置path：

```
C:\gradle-3.3;
```

把C:\GRADLE_REPO也下载下来放在C盘下，在系统环境变量那配置gradle_user_home，如下：

![GRADLE_REPO](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/GRADLE_REPO.PNG)

按win+R组合键并输入cmd进入命令控制台，输入`gradle -v`测试有没有安装成功，如下：

![gradle-v](https://github.com/lcfu1/Note/raw/master/JavaWeb/image/gradle-v.PNG)

### Tomcat8

无需安装，解压Tomcat8到指定目录 C:\apache-tomcat-8.0.44。

### Idea2017.2

在下载的文件中找到安装文件ideaIU-2017.3.1.exe，双击后，一路next即可。

选择license server，填写下面其中一个链接进行破解，要联网：

```
http://intellij.mandroid.cn/
http://idea.imsxm.com/
http://idea.iteblog.com/key.php
```

## 新建项目

步骤1：

![new1](https://github.com/lcfu1/Note/raw/master/JavaWeb/image/new1.PNG)

步骤2：

![new2](https://github.com/lcfu1/Note/raw/master/JavaWeb/image/new2.PNG)

步骤3：

![new3](https://github.com/lcfu1/Note/raw/master/JavaWeb/image/new3.PNG)

步骤4：

![new4](https://github.com/lcfu1/Note/raw/master/JavaWeb/image/new4.PNG)