## Android自定义View（四）

>要点：
>
>1、绘制图片
>
>2、参考

### 1、绘制图片

#### drawPicture(矢量图)

使用[Picture](https://developer.android.google.cn/reference/android/graphics/Picture.html)前请关闭硬件加速，以免引起不必要的问题！请参考[Android的硬件加速及可能导致的问题](https://github.com/GcsSloop/AndroidNote/issues/7)

关闭方法：
在AndroidManifest.xml的<application>加上android:hardwareAccelerated="false"。

>官方对Picture的描述：
A Picture records drawing calls (via the canvas returned by beginRecording) and can then play them back into Canvas (via draw(Canvas) or drawPicture(Picture)).For most content (e.g. text, lines, rectangles), drawing a sequence from a picture can be faster than the equivalent API calls, since the picture performs its playback without incurring any method-call overhead.
Note: Prior to API level 23 a picture cannot be replayed on a hardware accelerated canvas.
有道翻译：一个图片记录调用(通过画布返回的开始记录)，然后可以回放到画布(通过绘制(画布)或drawPicture(图片))。对于大多数内容(例如，文本、线条、矩形)，从图片中绘制序列比等效的API调用要快，因为图片执行它的播放而不产生任何方法调用开销。
注意:在API级别23之前，不能在硬件加速画布上重新播放图片。

Picture的公共方法和描述

公共方法|描述
-----------|-------
beginRecording(int width, int height)  |开始录制 (返回一个Canvas，在Canvas中所有的绘制都会存储在Picture中)
createFromStream(InputStream stream)|通过输入流创建一个Picture，该方法在API级别18中被弃用。推荐的替代方法是不使用writeToStream，而是把图片画成一个位图，你可以把它作为原始的或压缩的像素来保存。
draw(Canvas canvas)|将Picture中内容绘制到Canvas中
endRecording()|结束录制
getHeight()|获取高度
getWidth()|获取宽度
requiresHardwareAcceleration()|指示Picture是否包含只在绘制到硬件加速画布时才工作的记录命令。
writeToStream(OutputStream stream)|将Picture中内容写出到输出流中，该方法在API级别18中被弃用。推荐的方法是将图片绘制到位图中，您可以将其作为原始的或压缩的像素保存。

录制的内容是不会直接显示在屏幕上的，需要使用下面几种方法把它显示出来：

(1)[Picture](https://developer.android.google.cn/reference/android/graphics/Picture.html)提供的draw方法

- 对Canvas有影响，可操作性较弱。

(2)Canvas提供的drawPicture方法
- 对Canvas没有影响，可操作性较强。

(3)[PictureDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/PictureDrawable.html)的draw方法
- 对Canvas没有影响，可操作性较强。

*简单示例：*

```
package com.lcfu1.view;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.Picture;
import android.graphics.drawable.PictureDrawable;
import android.util.AttributeSet;
import android.view.View;

public class canvas extends View {
    // 1.创建一个画笔
    private Paint mPaint = new Paint();
    private Picture mPicture = new Picture();

    public canvas(Context context) {
        super(context);
    }

    public canvas(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
        //调用录制方法
        recording();
    }

    //初始化画笔
    private void init() {
        mPaint.setColor(Color.RED);
        mPaint.setStyle(Paint.Style.STROKE);
        mPaint.setStrokeWidth(2);
    }

    @Override
    public void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        //使用Picture提供的draw方法绘制
        //mPicture.draw(canvas);

        //使用Canvas提供的drawPicture方法绘制
        //canvas.drawPicture(mPicture);

        //将Picture包装成为PictureDrawable，使用PictureDrawable的draw方法绘制
        PictureDrawable drawable=new PictureDrawable(mPicture);
        //这里要设置绘制区域，不然显示不出来
        drawable.setBounds(0,0,400,400);
        drawable.draw(canvas);
    }

    //录制方法
    private void recording() {
        Canvas canvas = mPicture.beginRecording(400, 400);// 开始录制 (接收返回值Canvas)
        canvas.drawColor(Color.GRAY);
        canvas.translate(200,200);//位移
        canvas.drawCircle(0,0,100,mPaint);//绘制圆
        mPicture.endRecording();
    }
}
```

*layout中：*

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.lcfu1.view.canvas
        android:layout_width="200dp"
        android:layout_height="200dp" />
</LinearLayout>
```

*效果如下：*

![image.png](https://upload-images.jianshu.io/upload_images/6025530-db93df89e24b1b2f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### drawBitmap(位图)

drawBitmap的常用方法如下：

```
// 第一种，绘制图片内容，默认从坐标原点开始
public void drawBitmap(Bitmap bitmap, Matrix matrix, Paint paint)

// 第二种，指定与坐标原点的距离,这里要注意的是坐标原点，如果canvas有位移、旋转等操作，坐标原点会发生相应变化
public void drawBitmap(Bitmap bitmap, float left, float top, Paint paint)

// 第三种，Rect src指定绘制图片的区，Rect dst 或RectF dst指定图片在屏幕上显示(绘制)的区域
public void drawBitmap(Bitmap bitmap, Rect src, Rect dst, Paint paint)
public void drawBitmap(Bitmap bitmap, Rect src, RectF dst, Paint paint)
```

*简单示例如下：*

```
import android.content.Context;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.graphics.Canvas;
import android.graphics.Matrix;
import android.graphics.Paint;
import android.graphics.Rect;
import android.support.annotation.Nullable;
import android.util.AttributeSet;
import android.view.View;

/**
 * Created by lcf on 2018/4/21 0021.
 */

public class picture extends View {
    Bitmap mBitmap;

    public picture(Context context) {
        super(context);
    }

    public picture(final Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);

        //从mipmap获取Bitmap
        mBitmap= BitmapFactory.decodeResource(context.getResources(),R.mipmap.ic_launcher);

        //从assets获取Bitmap
//        try {
//            InputStream inputStream =context.getAssets().open("lcfu1.jpg");
//            mBitmap = BitmapFactory.decodeStream(inputStream);
//            inputStream.close();
//        } catch (IOException e) {
//            e.printStackTrace();
//        }

    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        //绘制图片内容，默认从坐标原点开始
        //canvas.drawBitmap(mBitmap, new Matrix(),new Paint());

        //指定与坐标原点的距离,这里要注意的是坐标原点，如果canvas有位移、旋转等操作，坐标原点会发生相应变化
        //canvas.drawBitmap(mBitmap,100,100,new Paint());

        Rect src = new Rect(0,0,mBitmap.getWidth(),mBitmap.getHeight()/2);// 指定图片绘制区域
        Rect dst = new Rect(0,0,mBitmap.getWidth(),mBitmap.getHeight()/2);//指定图片在屏幕上显示的区域,也就是给个固定区域来填满显示
        canvas.drawBitmap(mBitmap,src,dst,new Paint());// 绘制图片
    }
}
```

*效果如下：*

![image.png](https://upload-images.jianshu.io/upload_images/6025530-0615fd5fd988bd17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


上面例子是通过BitmapFactory从资源文件中获取Bitmap的，获取Bitmap的三种方式如下：

- 通过Bitmap创建：复制一个已有的Bitmap(新Bitmap状态和原有的一致)或者创建一个空白的Bitmap(内容可改变)。

- 通过BitmapDrawable获取：从资源文件、内存卡、网络等地方获取一张图片并转换为内容不可变的Bitmap。

- 通过BitmapFactory获取：从资源文件、内存卡、网络等地方获取一张图片并转换为内容不可变的Bitmap。

*BitmapFactory获取Bitmap的方法：*

- 从资源文件drawable、mipmap、raw、assets获取：

```
        //从mipmap获取Bitmap
        //mBitmap= BitmapFactory.decodeResource(context.getResources(),R.mipmap.ic_launcher);

        //从assets获取Bitmap
        try {
            InputStream is =context.getAssets().open("lcfu1.jpg");
            mBitmap = BitmapFactory.decodeStream(is);
            is.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
```

- 内存卡文件：

```
mBitmap= BitmapFactory.decodeFile("sdcard/lcfu1.jpg");
```

- 网络文件：

```
// 需要获取网络输入流的代码
Bitmap bitmap = BitmapFactory.decodeStream(inputStream);
inputStream.close();
```

注：从内存卡或网络上获取的方法我还没实现，有待解决！！

### 2、参考

- http://www.gcssloop.com/customview/CustomViewIndex/

