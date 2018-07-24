# Android Studio

目录：

- [优势](#优势)
- [下载安装](#下载安装)
- [离线配置Gradle](#离线配置gradle)
- [第一个Android程序](#第一个android程序)
- [实用技巧](#实用技巧)
  - [代码管理](#代码管理)
  - [代码编辑技巧](#代码编辑技巧)
  - [快捷键](#快捷键)
  - [调试技巧](#调试技巧)

## 优势

1. 由Google公司开发并大力完善和支持。
2. 稳定速度快：启动速度快，与其他IDE（集成开发环境Integrated Development Environment）相比，性能明显提升了。
3. 强大的UI编辑器：能实时展示界面布局效果。
4. 完善的插件管理：支持多种插件，可支持在插件管理中下载。
5. 支持多种代码管理工具：直接支持Git、SVN等主流的代码管理工具。
6. 整合了Gradle构建工具：Gradle继承了Ant的灵活性和Maven的生命周期管理，不使用XML作为配置文件格式，采用了DSL格式，使得脚本更加灵活简洁。
7. 内置终端：不需要自己打开一个终端来使用ADB等工具，当然也可以使用[Genymotion安卓模拟器](https://www.genymotion.com/)。
8. 智能化：智能保存、补齐等，提高开发效率。

## 下载安装

1. 可到[Android Studio中文社区](http://www.android-studio.org/)或[https://developer.android.google.cn/studio/](https://developer.android.google.cn/studio/)进行下载。
2. 最好下载的Android Studio包含构建 Android 应用所需的所有工具。
3. 如果电脑上已安装了JDK，则安装Android Studio时只需安装默认的选项。
4. 如果电脑上有SDK，可选择本地的SDK目录。

## 离线配置Gradle

1. Android Studio没有自带Gradle插件，但会自动下载Gradle。
2. [下载Gradle](http://gradle.android-studio.org/)。
3. 进入C:\Users\Administrator\.gradle\wrapper\dists\gradle-4.1-all\bzyivzo6n839fup2jbap0tjew，最后这个文件夹是随机生成的，直接进入，把下载好的gradle-3.0-all.zip放到这个文件夹内。
4. 重启Android Studio。

**注：Gradle是一种依赖管理工具，基于Groovy语言，抛弃了基于XML的各种繁琐配置，取而代之的是一种基于Groovy的内部领域特定语言（DSL），掌握Gradle脚本的编译和打包是应用开发非常必要的。**

## 第一个Android程序

**项目的结构模式：**

![项目结构模式](https://raw.githubusercontent.com/lcfu1/Image/master/Android/%E9%A1%B9%E7%9B%AE%E7%BB%93%E6%9E%84%E6%A8%A1%E5%BC%8F.PNG)

**Android模式的项目结构：**

![Android目录结构](https://raw.githubusercontent.com/lcfu1/Image/master/Android/Android%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.PNG)

**Project模式的项目结构：**

![Project目录结构](https://raw.githubusercontent.com/lcfu1/Image/master/Android/Project%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.PNG)

**app目录：**

![app目录](https://raw.githubusercontent.com/lcfu1/Image/master/Android/app%E7%9B%AE%E5%BD%95.PNG)

**Project模式目录讲解：**

1. .gradle和.idea：Android Studio自动生成的，不需要手动编辑。
2. app：放置项目中的代码、资源等内容。
3. build：编译时自动生成的文件。
4. gradle：包含了gradle wrapper的配置文件，使用gradle wrapper的方式不需要提前将gradle下载好，而是会自动根据本地缓存情况决定是否联网下载。Android Studio默认没有启动gradle wrapper的方式，可自己设置：File->Settings->Build，Execution，Deployment->Gradle。
5. .gitignore：过虑不需要上传的文件或目录。
6. build.gradle：项目全局的gradle构建脚本。
7. gradle.properties：全局的gradle配置文件。
8. gradlew和gradlew.bat：用于在命令行界面中执行gradle命令，其中gradlew是在Linux或Mac系统中使用的，gradlew.bat是在Windows系统中使用的。
9. Hello.iml：用于标识这是一个IntelliJ IDEA项目，所有的IntelliJ IDEA项目都会自动生成的一个文件（Android Studio是基于IntelliJ IDEA开发的）。
10. local.properties：指定本机Android SDK路径，如果Android SDK路径改变了，就修改此文件中的路径为新的本机Android SDK路径。
11. Settings.gradle：用于指定项目中所有引入的模块，Hello项目中只有一个app模块，所以只引入app模块。

**app目录讲解：**

1. build：编译时自动生成的文件。
2. libs：放置第三方jar包，会自动添加到构建路径中。
3. androidTest：用来编写Android Test测试用例的，可以对项目进行一些自动化测试。
4. java：放置所有Java代码。
5. res：放置项目所需资源。
6. AndroidManifest.xml：Android项目的配置文件。
7. test：用来编写Unit Test测试用例的，是对项目进行自动化测试的另一种方式。
8. .gitignore：过虑app模块中不需要上传的文件或目录。
9. app.iml：IntelliJ IDEA项目自动生成的文件。
10. build.gradle：app模块的gradle构建脚本。
11. proguard-rules.pro：指定项目的混淆规则，将代码进行混淆以防别人破解。

## 实用技巧

### 代码管理

1. 在电脑上安装Git，[windows下载地址](https://gitforwindows.org/)。

2. 配置File->Settings->Version Control->Git，配置Git目录。

   ![Git配置](https://github.com/lcfu1/Image/blob/master/Android/Git%E9%85%8D%E7%BD%AE.PNG?raw=true)

3. 配置File->Settings->Version Control->GitHub，点击Create API Token登录GitHub。

   ![Github配置](https://raw.githubusercontent.com/lcfu1/Image/master/Android/Github%E9%85%8D%E7%BD%AE.PNG)

4. 菜单栏->VCS->Enable Version Control Integration，选择Git，完成后工具栏会新增如下图所示的快捷工具：

   ![VCS](https://raw.githubusercontent.com/lcfu1/Image/master/Android/VCS.PNG)

5. 过虑不需要上传的文件或目录，File->Settings->Version Control->Ignored Files，选择不需要同步的文件或文件夹，也可以通过修改.gitignore文件来实现，建议使用第一种方式，如下图：

   ![Ignored Files](https://raw.githubusercontent.com/lcfu1/Image/master/Android/Ignored%20Files.PNG)

6. 同步代码到Github，菜单栏->VCS->Import into Version Control->Share Project On GitHub，如下图：

   ![Share Project](https://github.com/lcfu1/Image/blob/master/Android/Share%20Project.PNG?raw=true)

7. 单击Share按钮后，会弹出提交文件列表，不需要上传的文件或目录，可以不选择它，同步完成后就可以在Github上看到该目录了。

   ![Add Files](https://github.com/lcfu1/Image/blob/master/Android/Add%20Files.PNG?raw=true)

8. 后面修改的代码，就可以使用VCS快捷工具进行快速同步到Github上。

   ![VCS Commit](https://github.com/lcfu1/Image/blob/master/Android/VCS%20Commit.PNG?raw=true)

### 代码编辑技巧

1. 可通过File->Settings->Keymap来设置快捷键，如下图：

   ![Keymap](https://raw.githubusercontent.com/lcfu1/Image/master/Android/Keymap.PNG)

2. 内容补全：在写完方法名或需要用{}前，使用Ctrl+Shift+Enter组合键进行快速补全。

3. 列选择：按住Alt键选择代码块，可进行多行编辑。

4. 代码补全：使用Enter键可从光标处插入提示补全的代码，对前面的不做修改。使用Tab键可进行选择，但会删除后面的代码，直到遇到点号、圆括号、分号、空格等为止。

5. 查看方法调用路径：Ctrl+Alt+H组合键。

6. 预览某个方法或类的实现：Ctrl+Shift+I组合键。

7. 快速使用命令：Ctrl+Shift+A组合键。对于没有设置快捷键或忘记快捷键的菜单功能或命令时，可以使用该命令进行模糊匹配以快速调用。

8. 自动导入包设置：File->Settings->Editor->General->Auto Import，选中Show import popup、Show import pupup、Optimize imports on the fly 和 Add unambiguous imports on the fly，设置完成后就可以自动导入包和自动去掉无用包了。如下图：

   ![Auto Import](https://raw.githubusercontent.com/lcfu1/Image/master/Android/Auto%20Import.PNG)

9. Tip of the day：每次打开Android Studio都会出现一个Tip of the day，我们也可以手动打开：Help->Tip of the day。如下图：

   ![Tip of the day](https://github.com/lcfu1/Image/blob/master/Android/Tip%20of%20the%20day.PNG?raw=true)

### 快捷键

| 快捷键                | 作用                   |
| --------------------- | ---------------------- |
| Ctrl+鼠标左键点击     | 打开此关键字说明       |
| Alt+回车              | 导入包，自动修正       |
| Ctrl+N                | 查找类                 |
| Ctrl+Shift+N          | 查找文件               |
| Ctrl+Alt+L            | 格式化代码             |
| Ctrl+R                | 替换文本               |
| Ctrl+alt+空格         | 代码提示               |
| Ctrl+D                | 复制行                 |
| Ctrl+X                | 删除行                 |
| Ctrl+P                | 方法参数提示           |
| Ctrl+/                | 注释                   |
| Alt＋Up and Alt＋Down | 可在方法间快速移动     |
| Ctrl+Shift+Enter      | 内容补全               |
| 按住Alt键选择         | 列选择                 |
| Ctrl+Alt+H            | 查看方法调用路径       |
| Ctrl+Shift+I          | 预览某个方法或类的实现 |
| Ctrl+Shift+A          | 快速使用命令           |

### 调试技巧

1. 调试程序：通过菜单->Build->Attach to Android Process，也可以通过工具栏快捷键工具Attach to Android Process进入调试模式。
2. 条件断点：通过右键断点对一个断点加入条件，即填写Condition中的条件，只有满足条件时，才会进入断点。
3. 日志断点：当想打印一些日志信息，但又不想添加log代码后重新部署时，则可以在断点上单击鼠标右键，取消选中Suspend，然后勾选Log evaluated Expression，并在输入框中输入要打印的日志信息。
4. 分析传入/传出数据流：Menu->Analyze->Analyze Data Flow to Here这个操作将会根据当前选中的变量、参数或字段，分析出其传递到此处的路径。传出数据流（Analyze Data Flow from Here）则会分析当前选中的变量往下传递的路径，直到结束。
5. 修改变量值：修改变量值可以快速调试各个Case，提高异常处理的调试效率。