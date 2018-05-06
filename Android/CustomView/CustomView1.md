## Android自定义View（一）

>要点：
>
>1、屏幕坐标系
>
>2、view的坐标
>
>3、MotionEvent和TouchSlop
>
>4、角度与弧度
>
>5、简介颜色
>
>6、颜色混合模式
>
>7、定义颜色
>
>8、参考

### 1、屏幕坐标系

手机屏幕左上角为坐标原点，向右为x轴增大方向，向下为y轴增大方向。

如下图：

![1.png](https://upload-images.jianshu.io/upload_images/6025530-0ad7a028d14ead9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2、view的坐标

view的坐标是相对父容器而言的，包括：getTop()、getBottom()、getLeft()、getRight()。

如下图：

![2.png](https://upload-images.jianshu.io/upload_images/6025530-3915e3c650d35ffe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从android3.0开始，View增加了几个参数：x、y、translationX和traslationY，x和y是View左上角的坐标，而translationX和traslationY是View左上角相对于父容器的偏移量。这几个参数是相对于父容器的坐标，并且translationX和traslationY的默认值是0，和View的四个基本的位置参数一样，View也提供了get和set方法。这几个参数的换算关系如下：
x=left+translationX、y=top+translationY，View在平移的过程中， top和left表示原始左上角的位置信息，它们的值并不会发生改变，此时发生改变的是x、y、translationX和traslationY这四个参数。

### 3、MotionEvent和TouchSlop

#### MotionEvent中的getRaw和get

event.getRowX()、event.getRowY()：触摸点相对于屏幕原点的x坐标

event.getX()、event.getY()： 触摸点相对于其所在组件原点的x坐标

![3.png](https://upload-images.jianshu.io/upload_images/6025530-56c44a9c570205a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### TouchSlop

TouchSlop是系统所能识别出的被认为是滑动的最小距离，就是两次滑动之间的距离小于这个常量，系统就不认为你在进行滑动操作，这是一个常量，和设备有关。获取方法：ViewConfiguration.get(getContext()).getScaledTouchSlop()。

### 4、角度与弧度

注：默认的屏幕坐标系中角度增大方向为顺时针

角度：角度是60进制，两条射线从圆心向圆周射出，形成一个夹角和夹角正对的一段弧。当这段弧长正好等于圆周长的360分之一时，两条射线的夹角的大小为1度。

如下图：

![4.png](https://upload-images.jianshu.io/upload_images/6025530-1a2d61afbea4bf4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

弧度：弧度是10进制，两条射线从圆心向圆周射出，形成一个夹角和夹角正对的一段弧。当这段弧长正好等于圆的半径时，两条射线的夹角大小为1弧度。

如下图：

![4.png](https://upload-images.jianshu.io/upload_images/6025530-ae2d945fcaaa0280.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

角度和弧度的换算：rad = deg x π / 180，deg = rad x 180 / π，其中rad是弧度，deg是角度。圆的周长为2πr，由上面弧度概念可知圆360度对应2π弧度。

### 5、简介颜色

屏幕上默认的模式是RGB565，而我们常用的都是ARGB8888（四通道高精度(32位)）和ARGB4444（四通道低精度(16位)），还有一种不常用的Alpha8（仅有透明通道(8位)）。

ARGB通道，其中RGB是红绿蓝，A是Alpha（通常用来作为此颜色的透明度），取值范围均为0~255，RGB 从0x00到0xff表示颜色从深到浅，A 从ox00到oxff表示从透明到不透明。

在Android Studio中打开一张图片，在左上角可以看到一个绿色吸管（颜色选择器）。 

如下图：

![image.png](https://upload-images.jianshu.io/upload_images/6025530-7d0c06a9eb3ca7b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击打开，在中间的圆圈色板中选择任意颜色可得到它的RGB值，不过它还有个功能，点击下图红色圈出的绿色吸管，你就可以查看电脑当前屏幕中任意颜色的RGB值。

![image.png](https://upload-images.jianshu.io/upload_images/6025530-85456ea32c4f6e66.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注：Android Studio界面不要最大化，然后把你想要查的图片或其它画面和它放在电脑的同一屏幕下。别小看这个小功能，很实用的。

如下gif图（有点模糊，将就看）：

![5.gif](https://upload-images.jianshu.io/upload_images/6025530-249f6f4041e77b78.gif?imageMogr2/auto-orient/strip)

### 6、颜色混合模式

安卓着色器(tint)可以为图标着色，既可以在xml中，也可以在代码中设置，一共有16种颜色混合模式。

如下图：

![image.png](https://upload-images.jianshu.io/upload_images/6025530-54ed2b581e9cdb32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- Clear：在底部中清除内容所在的区域
- Dst：可以看做底部的背景
- Src：可以看做上面的内容
- Srcover：内容覆盖在背景之上
- DstOver：背景覆盖在内容之上
- SrcIn:显示Src和Dst相交的Src区域
- Dstin：显示相交的Dst区域
- SrcOut：显示不相交的Src区域
- DstOut：显示不相交的Dst区域
- SrcAtTop:显示Dst所在的区域，相交区域显示Src
- DstAtTop：显示Src所在区域，相交区域显示Dst
- Xor：异或，相交区域不显示，其他区域保留
- Sarken：Src覆盖在Dst之上，相交区域Src以最暗显示
- Lighten：Src覆盖在Dst之上，相交区域Src以最亮显示
- Multiply：显示相交区域的混合
- Screen：显示全部区域，相交区域以全色显示，即白色

Alpha通道主要在两个图像混合的时候生效。默认情况下，当一个颜色绘制到Canvas上时的混合模式是这样计算的：(RGB通道) 最终颜色 = 绘制的颜色 + (1 - 绘制颜色的透明度) × Canvas上的原有颜色。绘制颜色和原有颜色都是经过预乘了自己的Alpha通道的值的。

PorterDuff模式的混合计算公式：（D指原本在Canvas上的内容dst，S指绘制输入的内容src，a指alpha通道，c指RGB各个通道）。

- ADD：Saturate(S + D)
- CLEAR：[0, 0]
- DARKEN：[Sa + Da - SaDa, Sc(1 - Da) + Dc*(1 - Sa) + min(Sc, Dc)]
- DST：[Da, Dc]
- DST_ATOP：[Sa, Sa * Dc + Sc * (1 - Da)]
- DST_IN：[Sa * Da, Sa * Dc]
- DST_OUT：[Da * (1 - Sa), Dc * (1 - Sa)]
- DST_OVER：[Sa + (1 - Sa)Da, Rc = Dc + (1 - Da)Sc]
- LIGHTEN：[Sa + Da - SaDa, Sc(1 - Da) + Dc*(1 - Sa) + max(Sc, Dc)]
- MULTIPLY：[Sa * Da, Sc * Dc]
- SCREEN：[Sa + Da - Sa * Da, Sc + Dc - Sc * Dc]
- SRC：[Sa, Sc]
- SRC_ATOP：[Da, Sc * Da + (1 - Sa) * Dc]
- SRC_IN：[Sa * Da, Sc * Da]
- SRC_OUT：[Sa * (1 - Da), Sc * (1 - Da)]
- SRC_OVER：[Sa + (1 - Sa)Da, Rc = Sc + (1 - Sa)Dc]
- XOR：[Sa + Da - 2 * Sa * Da, Sc * (1 - Da) + (1 - Sa) * Dc]

### 7、定义颜色

上面简单了解了一下颜色的相关内容，现在来介绍一下使用。

在java中定义使用：

```
int color= Color.BLUE;//蓝色
int color1= Color.rgb(0,0,255);//蓝色
int color2= Color.argb(127,0,0,255);//半透明度蓝色
int color3=0xff0000ff;//蓝色带透明度
int color4=getResources().getColor(R.color.blue);//引用xml中定义的颜色
```

在xml中定义：

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!--蓝色-->
    <color name="blue">#0000ff</color>
    <!--半透明度蓝色-->
    <color name="blue1">#7f0000ff</color>
</resources>
```

### 8、参考

- [http://www.gcssloop.com/customview/CustomViewIndex/](http://www.gcssloop.com/customview/CustomViewIndex/)

- [http://www.cnblogs.com/zhucai/p/android-graphics-animation.html](http://www.cnblogs.com/zhucai/p/android-graphics-animation.html)

- [http://blog.csdn.net/why200981317/article/details/51582128](http://blog.csdn.net/why200981317/article/details/51582128)