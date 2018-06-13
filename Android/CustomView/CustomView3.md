## Android自定义View（三）

目录：

- [自定义View的分类](#自定义view的分类)
- [自定义View需要注意的事项](#自定义view需要注意的事项)
- [继承View重写onDraw方法](#继承view重写ondraw方法)
- [实现一个简单的ImageView](#实现一个简单的imageview)
- [参考](#参考)

## 自定义View的分类

自定义View的分类标准不是唯一的，有些人分为两类：自定义View和自定义ViewGroup，而这里分为4类，如下：

（1）继承View重写onDraw方法：用于实现一些不规则的效果，需要自己支持wrap_content和处理padding。

（2）继承ViewGroup派生特殊的Layout：用于实现自定义布局，需要合适地处理ViewGroup的测量和布局这两个过程，并同时处理子元素的测量和布局过程。

（3）继承特定的View（如TextView）：扩展已有View的功能，不需要自己支持wrap_content和处理padding。

（4）继承特定的ViewGroup（如LinearLayout）：不需要自己处理ViewGroup的测量和布局这两个过程。

## 自定义View需要注意的事项

（1）让View支持wrap_content：直接继承View或ViewGroup的控件如果不在onMeasure中对wrap_content做特殊处理，那么在布局中设置wrap_content并不会达到预期的效果。

（2）让View支持padding：直接继承View的控件如果不在draw方法中处理padding，那么在布局中设置padding属性是达不到预期效果的（margin属性是由父容器控制的，所以不需要做特殊的处理）。继承ViewGroup的控件则需要在onMeasure和onLayout中考虑padding和子元素的margin对其造成的影响。

（3）不要在View中使用Handler：View内部提供了post系列的方法来替代Handler的作用。

（4）及时停止View中的线程或动画：如果View中有线程或动画，当包含此View的Activity退出、此View被remove或此View变得不可见时（预防内存泄漏），就调用onDetachedFromWindow方法。当包含此View的Activity启动时，就调用onAttachedToWindow方法。

（5）处理好View中的滑动冲突：当View带有滑动嵌套时，就要合适地处理滑动冲突，否则会影响View的效果。

## 继承View重写onDraw方法

举个简单的例子，如下：

CircleView.java代码如下：

```
package com.lcfu1.view;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.support.annotation.Nullable;
import android.util.AttributeSet;
import android.view.View;

public class CircleView extends View{
    private Paint mPaint = new Paint();
    private int mColor= Color.BLUE;

    public CircleView(Context context) {
        super(context);
        init();
    }
    public CircleView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        init();
    }

    private void init() {
        mPaint.setColor(mColor);
        mPaint.setStyle(Paint.Style.FILL);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        int paddingLeft=getPaddingLeft();
        int paddingRight=getPaddingRight();
        int paddingTop=getPaddingTop();
        int paddingBottom=getPaddingBottom();
        int width=getWidth()-paddingLeft-paddingRight;
        int height=getHeight()-paddingTop-paddingBottom;
        int radius=Math.min(width,height);
        canvas.drawCircle(width/2+paddingLeft,height/2+paddingTop,radius/2,mPaint);
    }
}
```

布局如下：

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.lcfu1.view.CircleView
        android:layout_width="match_parent"
        android:layout_height="200dp"
        android:background="#fc5355"
        android:layout_margin="30dp"
        android:padding="20dp"/>
</LinearLayout>
```

上面的代码是绘制一个中心点以宽高的最小值为直径的蓝色实心的圆形，因为margin属性是由父容器控制的，所以不需要在CircleView中做特殊的处理。但是直接继承View和ViewGroup的控件，padding属性是默认不生效的，需要在CircleView中自己处理。

没在代码中进行处理，设置padding属性是不生效的，如下：

![2.PNG](https://upload-images.jianshu.io/upload_images/6025530-7045606bf2074909.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在代码中处理后如下：

![image.png](https://upload-images.jianshu.io/upload_images/6025530-3138d32b3a3cc0c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为CircleView添加自定义属性，在values目录下新建一个attr.xml（文件名没有特殊限制，可以根据需要来命名），attr.xml如下：

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="CircleView">
        <attr name="circle_color" format="color"/>
    </declare-styleable>
</resources>
```

*CircleView.java代码修改如下：*

```
package com.lcfu1.view;

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.support.annotation.Nullable;
import android.util.AttributeSet;
import android.view.View;

public class CircleView extends View{
    private Paint mPaint = new Paint();
    private int mColor= Color.BLUE;

    public CircleView(Context context) {
        super(context);
        init();
    }
    public CircleView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        TypedArray typedArray=context.obtainStyledAttributes(attrs,R.styleable.CircleView);
        //如果没有指定circle_color属性就选择Color.BLUE做为默认颜色
        mColor=typedArray.getColor(R.styleable.CircleView_circle_color,Color.BLUE);
        //实现资源
        typedArray.recycle();
        init();
    }

    private void init() {
        mPaint.setColor(mColor);
        mPaint.setStyle(Paint.Style.FILL);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        int paddingLeft=getPaddingLeft();
        int paddingRight=getPaddingRight();
        int paddingTop=getPaddingTop();
        int paddingBottom=getPaddingBottom();
        int width=getWidth()-paddingLeft-paddingRight;
        int height=getHeight()-paddingTop-paddingBottom;
        int radius=Math.min(width,height);
        canvas.drawCircle(width/2+paddingLeft,height/2+paddingTop,radius/2,mPaint);
    }
}
```

*布局如下：*

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.lcfu1.view.CircleView
        android:layout_width="match_parent"
        android:layout_height="200dp"
        android:background="#fc5355"
        android:layout_margin="30dp"
        android:padding="20dp"
        app:circle_color="#ffea00"/>
</LinearLayout>
```

*效果如下：*

![image.png](https://upload-images.jianshu.io/upload_images/6025530-d250b10bd724288c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码首先加载自定义属性集合CircleView，然后解析circle_color属性，如果没有指定circle_color属性就选择Color.BLUE做为默认颜色，解析完成后就使用recycle()方法实现资源。在布局中使用自定义属性必须在布局文件中添加schemas声明，如：xmlns:app="http://schemas.android.com/apk/res-auto"，app是自定义属性的前缀，使用如：app:circle_color="#ffea00"。还有另外一个声明方法，就是使用该工程的应用包名，如xmlns:app="http://schemas.android.com/apk/res/com.lcfu1.view"，这种声明方式也是可以的，但是会有如下提示，所以建议使用第一种方式：

>In Gradle projects, always use http://schemas.android.com/apk/res-auto for custom attributes less... (Ctrl+F1) 
In Gradle projects, the actual package used in the final APK can vary; for example,you can add a .debug package suffix in one version and not the other. Therefore, you should not hardcode the application package in the resource; instead, use the special namespace http://schemas.android.com/apk/res-auto which will cause the tools to figure out the right namespace for the resource regardless of the actual package used during the build.
**翻译：在Gradle项目中，总是使用http://schemas.android.com/apk/res-auto来获取定制属性。(Ctrl + F1)
在Gradle项目中，最终APK中使用的实际包会有所不同;例如，您可以在一个版本中添加.debug包后缀，而不是另一个版本。因此，您不应该在资源中硬编码应用程序包;相反，使用特殊的命名空间http://schemas.android.com/apk/res-auto，它将会导致工具为资源找到正确的名称空间，而不考虑构建期间使用的实际包。**

在布局中，设置android:layout_width为match_parent或指定一个值（如200dp）都可以达到预期的效果，如指定为200dp，效果如下：

![image.png](https://upload-images.jianshu.io/upload_images/6025530-af7005f58c791bde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但如果设置为wrap_content并不会达到预期的效果，而是跟使用match_parent一样。可以通过在代码中指定一个wrap_content模式的默认宽或高来解决，如选择400px作为默认宽或高（注：这里是px而不是dp）。布局中layout_width设置为wrap_content（当然也可以把layout_height也设置为wrap_content，这里只是举个例子），CircleView.java代码修改如下：

```
package com.lcfu1.view;

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.support.annotation.Nullable;
import android.util.AttributeSet;
import android.view.View;

public class CircleView extends View{
    private Paint mPaint = new Paint();
    private int mColor= Color.BLUE;

    public CircleView(Context context) {
        super(context);
        init();
    }
    public CircleView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        TypedArray typedArray=context.obtainStyledAttributes(attrs,R.styleable.CircleView);
        //如果没有指定circle_color属性就选择Color.BLUE做为默认颜色
        mColor=typedArray.getColor(R.styleable.CircleView_circle_color,Color.BLUE);
        //实现资源
        typedArray.recycle();
        init();
    }

    private void init() {
        mPaint.setColor(mColor);
        mPaint.setStyle(Paint.Style.FILL);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);
        if(widthMode==MeasureSpec.AT_MOST && heightMode==MeasureSpec.AT_MOST){
            setMeasuredDimension(400,400);
        }else if(widthMode==MeasureSpec.AT_MOST){
            setMeasuredDimension(400,heightSize);
        }else if(heightMode==MeasureSpec.AT_MOST){
            setMeasuredDimension(widthSize,400);
        }
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        int paddingLeft=getPaddingLeft();
        int paddingRight=getPaddingRight();
        int paddingTop=getPaddingTop();
        int paddingBottom=getPaddingBottom();
        int width=getWidth()-paddingLeft-paddingRight;
        int height=getHeight()-paddingTop-paddingBottom;
        int radius=Math.min(width,height);
        canvas.drawCircle(width/2+paddingLeft,height/2+paddingTop,radius/2,mPaint);
    }
}
```

*效果如下：*

![image.png](https://upload-images.jianshu.io/upload_images/6025530-5d958bcb5e460771.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如上面的代码 ，AT_MOST对应LayoutParams中的wrap_content，如布局中layout_width使用了wrap_content，就指定一个wrap_content模式的默认宽给它，如400px。这里是px而不是dp。dp（英文density-independent-pixel的缩写，意为密度无关像素），在不同的像素密度的设备上会自动适配。上面的效果截图是正方形的，可能设备刚好是1dp = 2px的，在其它手机上测试就不一定是正方形了，这点是需要注意的。

## 实现一个简单的ImageView

*ImageView.java代码如下：*

```
package com.lcfu1.view;

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Bitmap;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.drawable.Drawable;
import android.support.annotation.Nullable;
import android.util.AttributeSet;
import android.view.View;

public class ImageView extends View{
    private Paint mBitmapPaint=new Paint();
    private Drawable mDrawable;
    Bitmap mBitmap;
    private int mWidth;
    private int mHeight;

    public ImageView(Context context) {
        super(context);
        init();
    }
    public ImageView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        if(attrs!=null){
            TypedArray typedArray=null;
            try{
                typedArray=context.obtainStyledAttributes(attrs,R.styleable.ImageView);
                mDrawable=typedArray.getDrawable(R.styleable.ImageView_src);
                measureDrawable();
            }finally {
                if(typedArray!=null){
                    typedArray.recycle();
                }
            }
        }
        init();
    }

    private void measureDrawable() {
        if(mDrawable==null){
            throw new RuntimeException("drawable不能为空");
        }
        mWidth=mDrawable.getIntrinsicWidth();
        mHeight=mDrawable.getIntrinsicHeight();
    }

    private void init() {
        //抗锯齿功能
        mBitmapPaint.setAntiAlias(true);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        setMeasuredDimension(resolveSize(mWidth, widthMeasureSpec),
                resolveSize(mHeight, heightMeasureSpec));
    }

    @Override
    protected void onDraw(Canvas canvas) {
        if(mBitmap==null){
            mBitmap = Bitmap.createScaledBitmap(ImageUtils.drawableToBitamp(mDrawable),
                    getMeasuredWidth(), getMeasuredHeight(), true);
        }
        canvas.drawBitmap(mBitmap,getLeft(),getTop(),mBitmapPaint);
        //保存画布状态
        canvas.save();
        //画布顺时针旋转90度
        canvas.rotate(90);
        //设置画笔颜色为黑色
        mBitmapPaint.setColor(Color.BLACK);
        //设置绘制的文本大小
        mBitmapPaint.setTextSize(30);
        //绘制文本
        canvas.drawText("LCFU1", getLeft() + 50, getTop() - 50, mBitmapPaint);
        //画布恢复原来的状态
        canvas.restore();
    }
}
```

上面代码使用了一个drawable转bitmap的工具类ImageUtils.java，代码如下：

```
package com.lcfu1.view;

import android.graphics.Bitmap;
import android.graphics.Canvas;
import android.graphics.drawable.BitmapDrawable;
import android.graphics.drawable.Drawable;

//drawable转bitmap
public final class ImageUtils {
    private ImageUtils() {
    }
    public static Bitmap drawableToBitamp(Drawable drawable) {
        if (drawable instanceof BitmapDrawable) {
            BitmapDrawable bd = (BitmapDrawable) drawable;
            return bd.getBitmap();
        }
        int w = drawable.getIntrinsicWidth();
        int h = drawable.getIntrinsicHeight();
        Bitmap bitmap = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_4444);
        Canvas canvas = new Canvas(bitmap);
        drawable.setBounds(0, 0, w, h);
        drawable.draw(canvas);
        return bitmap;
    }
}
```

*布局如下：*

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.lcfu1.view.ImageView
        android:layout_width="300dp"
        android:layout_height="300dp"
        app:src="@drawable/image" />
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="LCFU1"
        android:textSize="15sp"
        android:textColor="#000000"/>
</LinearLayout>
```

*attr.xml如下：*

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="ImageView">
        <attr name="src" format="integer"/>
    </declare-styleable>
</resources>
```

*效果如下：*

![image.png](https://upload-images.jianshu.io/upload_images/6025530-29d034b9b7aed7bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果布局文件中ImageView控件的layout_width和layout_height设置为wrap_content，则效果如下：

![image.png](https://upload-images.jianshu.io/upload_images/6025530-c63c217b7c9b4910.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


attr.xml中的ImageView属性集里面有一个src的整型属性，通过这个自定义属性，我们就可以在ImageView控件中使用该属性来设置图片的资源id。绘制文本，先保存画布状态，然后将画布顺时针旋转90度，再在画布上绘制文字，最后将画布恢复到原来的状态。对画布进行平移或旋转等其实都是对坐标系的平移或旋转，画布本身并没有变化。过程如下：

![无标题.png](https://upload-images.jianshu.io/upload_images/6025530-240815fd2d1e603b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 参考

- 《Android开发艺术探索》--任玉刚
- 《Android开发进阶从小工到专家》--何红辉

