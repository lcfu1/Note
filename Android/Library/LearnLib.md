# 学习封装库

目录：

- [前言](#前言)
- [实现](#实现)
- [JitPack](#jitpack)
- [链接](#链接)

## 前言

学习Android好久了，也使用过很多第三方开源库，这些开源库使用我们的开发变得更加的简单，那么有没有想过自己也写一个开源库并提供引用让别人使用？

## 实现

1. 在Android Studio中新建一个项目，这里举例命名为LearnLib，然后File->new->new module，如下：

   ![new module](https://raw.githubusercontent.com/lcfu1/Image/master/Android/new%20module.PNG)

2. 选择Android Library，如下：

   ![Android Library](https://raw.githubusercontent.com/lcfu1/Image/master/Android/Android%20Library.PNG)

3. 进行新建，如下：

   ![Create Android Library](https://raw.githubusercontent.com/lcfu1/Image/master/Android/Create%20Android%20Library.PNG)

4. 完成后，我们封装一个简单的示例，如下：

   ![testLib](https://raw.githubusercontent.com/lcfu1/Image/master/Android/testLib.PNG)

5. 然后上传整个项目到github上面，这一步不会的请参考[这里](https://lcfu1.github.io/Note/Android/AndroidStudio.html#代码管理https://lcfu1.github.io/Note/Android/AndroidStudio.html#代码管理)。

6. 进入[github官网](https://github.com/)， 点击进入LearnLib项目，然后再点击releases，创建一个releases，这里我填1.0.0，如下：

   ![releases](https://raw.githubusercontent.com/lcfu1/Image/master/Android/releases.PNG)

7. 例子也有了，也上传到github上面了，那怎么提供引用让别人使用呢？这里就要靠[JitPack](https://jitpack.io/)了。

## JitPack

1. 进行[JitPack的官网](https://jitpack.io)。

2. 然后Sign In，这里使用你的github授权登录就好。

3. 登录进入就会看到你上传到github上的项目，点击我们刚刚上传到github上的LearnLib项目，或在搜索栏输入`github用户名/项目名`如下：

   ![Lib Git it](https://raw.githubusercontent.com/lcfu1/Image/master/Android/Lib%20Git%20it.PNG)

4. 选择你要引用的版本，点击Get it，现在就可以在我们的项目中引用了，使用如下：

   ![Lib使用](https://raw.githubusercontent.com/lcfu1/Image/master/Android/Lib%E4%BD%BF%E7%94%A8.PNG)

## 链接

- [项目地址](https://github.com/lcfu1/LearnLib)
- [sample](https://github.com/lcfu1/LearnLib/tree/master/sample)