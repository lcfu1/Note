# Android自定义View（二）

>要点：
>
>1、自定义View绘制流程
>
>2、绘制随手移动的小球
>
>3、Canvas
>
>4、Paint
>
>5、参考

#### 1、自定义View绘制流程

![image](http://upload-images.jianshu.io/upload_images/6025530-da79ee12c89dbad3..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 构造函数

四种重载：

```
//一般在直接New一个View的时候调用。
public void SimpleView(Context context) {}
//一般在layout文件中使用的时候会调用，关于它的所有属性(包括自定义属性)都会包含在attrs中传递进来。
public void SimpleView(Context context, AttributeSet attrs) {}
//调用了三个参数的构造函数，必须明确指定第三个参数，第三个参数是默认的Style（当前Application或Activity所用的Theme中的默认Style）。
public void SimpleView(Context context, AttributeSet attrs, int defStyleAttr) {}
public void SimpleView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {}
```

使用：

在activity中：

```
SimpleView view=new SimpleView(this);
```

在layout中：

```
<com.lcfu1.view.SimpleView
  android:layout_width"wrap_content"
  android:layout_height"wrap_content"/>
```

##### onMeasure()

测量View大小，View的大小不仅由自身所决定，同时也会受到父控件的影响。

```
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);

        int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);
}
```

widthMeasureSpec 和 heightMeasureSpec两个 int 类型的参数，是由宽、高和各自方向上对应的测量模式来合成的一个值。getMode()：测量模式，getSize()：确切数值。

SpecMode类型：
- UNSPECIFIED：父视图没有给子视图任何限制，子视图可以设置为任意大小。
- EXACTLY：表示父视图已经确切的指定了子视图的大小，match_parent、具体的数值（如100dp）对应的都是这个模式。
- AT_MOST：表示子视图具体大小没有尺寸限制，但是存在上限，上限一般为父视图大小，一般来说wrap_content对应这种模式。

##### onSizeChanged()

```
@Override
protected void onSizeChanged(int w, int h, int oldw, int oldh) {
	super.onSizeChanged(w, h, oldw, oldh);
}
```

在视图大小发生改变时调用。w和h就是View最终的大小。

###### onLayout()

```
@Override
protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
	super.onLayout(changed, left, top, right, bottom);
}
```

确定布局的函数，用于确定子View的位置，在自定义ViewGroup中会用到，他调用的是子View的layout函数。

在自定义ViewGroup中，onLayout一般是循环取出子View，然后经过计算得出各个子View位置的坐标值。

对于非ViewGroup类型来说，它需要做的工作只是测量尺寸与绘制自身内容。

##### onDraw()

```
@Override
public void onDraw(Canvas canvas)
{
	super.onDraw(canvas);
}
```

就是实际绘制内容。

#### 2、绘制随手移动的小球

```
package com.lcfu1.view;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;

public class DrawView extends View
{
	//小球的初始位置
	public float currentX = 50;
	public float currentY = 50;

	// 定义、并创建画笔
	Paint p = new Paint();

	public DrawView(Context context)
	{
		super(context);
	}
	public DrawView(Context context , AttributeSet set)
	{
		super(context,set);
	}
	
	//绘制内容
	@Override
	public void onDraw(Canvas canvas)
	{
		super.onDraw(canvas);
                // 设置画布的颜色
		canvas.drawColor(Color.GRAY);
		// 设置画笔的颜色
		p.setColor(Color.RED);
		// 绘制一个小圆（作为小球）
		canvas.drawCircle(currentX, currentY, 15, p);
	}
	// 为该组件的触碰事件重写事件处理方法
	@Override
	public boolean onTouchEvent(MotionEvent event)
	{
		// 修改currentX、currentY两个属性
		currentX = event.getX();
		currentY = event.getY();
		// 通知当前组件重绘自己
		invalidate();
		// 返回true表明该处理方法已经处理该事件
		return true;
	}
}
```

在现实中要画东西就需要画纸，而要在屏幕上面画东西，则需要[Canvas](https://developer.android.google.cn/reference/android/graphics/Canvas.html)，也就是画布。

#### 3、Canvas

常用方法如下：

![image.png](https://upload-images.jianshu.io/upload_images/6025530-72208d40347687a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

例：

- drawColor(int color)：设置画布的颜色，如：

```
canvas.drawColor(Color.GRAY);
```

- drawPaint([Paint paint)：画点，如:

```
canvas.drawPoint(10,10,paint); 
```

- drawPoints(float[] pts,Paint paint)：画一组点，如：

```
canvas.drawPoints(new float[]{100,100,130,100},paint);
```

- drawLine(float startX, float startY, float stopX, float stopY, Paint paint)：画线，如画一条横线：

```
canvas.drawLine(100,100,200,100,paint);
```

- drawLines(float[] pts,Paint paint)：画一组线，如画两条横线：

```
canvas.drawLines(new float[]{100,100,200,100,100,200,200,200},paint);
```

- drawCircle(float cx, float cy, float radius, Paint paint)：画圆，如：

```
canvas.drawCircle(100, 100, 15, paint);
```

- drawRect(float left, float top, float right, float bottom, Paint paint)：画距形，如：

```
canvas.drawRect(100,100,200,200,paint);
```

- drawRoundRect(float left, float top, float right, float bottom, float rx, float ry, Paint paint)：画圆角距形，如：

```
canvas.drawRoundRect(100,100,200,200,20,20,paint);
```

- 不过上面这种方法在API21及以上才能使用。如果是API21以下，我们可以使用如下方法:

```
RectF rectF=new RectF(100,100,200,200);
canvas.drawRoundRect(rectF,20,20,paint);
```

- drawOval(float left, float top, float right, float bottom,Paint paint)：画椭圆，如：

```
canvas.drawOval(100,100,200,200,paint);
```

- 不过上面这种方法同画圆角距形一样，在API21及以上才能使用。如果是API21以下，我们可以使用如下方法:

```
RectF rectF = new RectF(100,100,200,200);
canvas.drawOval(rectF,paint);
```

- drawArc(float left, float top, float right, float bottom, float startAngle, float sweepAngle, boolean useCenter, Paint paint)：画圆弧，在API21及以上才能使用，其中startAngle是开始角度、sweepAngle是扫过角度、useCenter是是否使用中心，使用中心绘制出来的图形类似于一个扇形，而设置为false则是圆弧起始点和结束点之间的连线加上圆弧围成的图形。如：

```
canvas.drawArc(100,100,200,200,0,90,false,paint);
```

- drawArc(RectF oval, float startAngle, float sweepAngle, boolean useCenter, Paint paint)：也是画圆弧，如果是API21以下，我们可以使用以上这种方法，如：

```
RectF rectF = new RectF(100,100,200,200);
canvas.drawArc(rectF,0,90,true,paint);
```

- translate(float dx, float dy)：位移，translate是坐标系的移动，如果有多个translate()，则是基于当前位置移动，而不是每次都基于屏幕左上角的(0,0)点移动。如下：

```
//设置画笔颜色
mPaint.setColor(Color.BLACK);
canvas.translate(100,100);
canvas.drawCircle(0,0,10,mPaint);
canvas.translate(100,100);
canvas.drawCircle(0,0,10,mPaint);
canvas.translate(100,-100);
canvas.drawCircle(0,0,10,mPaint);
```

![image.png](https://upload-images.jianshu.io/upload_images/6025530-de9d2109037733a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- scale(float sx, float sy)：缩放，缩放的中心默认为坐标原点，而缩放中心轴就是坐标轴，当缩放比例为负数的时候会根据缩放中心轴进行翻转。

```
mPaint.setColor(Color.BLACK);//设置画笔颜色
canvas.translate(100,100);//移动坐标系原点到（100，100）
canvas.drawCircle(0,0,100,mPaint);//画圆
canvas.scale(0.5f,0.5f);// 画布缩放
mPaint.setColor(Color.YELLOW);// 改变画笔颜色为黄色
canvas.drawCircle(0,0,100,mPaint);//画圆
```

![image.png](https://upload-images.jianshu.io/upload_images/6025530-2caafdbbed720020.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- scale(float sx, float sy, float px, float py)：这个方法可以控制缩放中心的位置，缩放中心就是对应坐标系的原点。如果使用了translate()，坐标系位置发生了改变，则缩放中心的位置是改变后对应的坐标原点，而不是屏幕左上角的(0,0)点。如：

```
mPaint.setColor(Color.BLACK);//设置画笔颜色
canvas.translate(100,100);//移动坐标系原点到（100，100）
canvas.drawCircle(0,0,100,mPaint);//画圆
canvas.scale(0.5f,0.5f,0,0);// 画布缩放
mPaint.setColor(Color.YELLOW);// 改变画笔颜色为黄色
canvas.drawCircle(0,0,100,mPaint);//画圆
```

![image.png](https://upload-images.jianshu.io/upload_images/6025530-8f9cd965a198f2e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- scale是可以叠加的，如下：

```
mPaint.setColor(Color.BLACK);//设置画笔颜色
mPaint.setStyle(Paint.Style.STROKE);//描边
mPaint.setStrokeWidth(20f);//设置画笔宽度为10px
canvas.translate(100,100);//移动坐标系原点到（100，100）
canvas.drawCircle(0,0,100,mPaint);//画圆
canvas.scale(0.5f,0.5f);// 画布缩放
canvas.drawCircle(0,0,100,mPaint);//画圆
canvas.scale(0.5f,0.5f);// 画布缩放
canvas.drawCircle(0,0,100,mPaint);//画圆
```

![image.png](https://upload-images.jianshu.io/upload_images/6025530-4e0d2ad72aa6d681.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- rotate(float degrees)：以当前坐标原点旋转，跟scale一样有控制中心和可叠加。如：

```
mPaint.setColor(Color.BLACK);//设置画笔颜色
canvas.translate(100,100);//移动坐标系原点到（100，100）
canvas.drawRect(0,0,100,100,mPaint);//画距形
canvas.rotate(90);//旋转90度
mPaint.setColor(Color.YELLOW);//改变画笔颜色
canvas.drawRect(0,0,100,100,mPaint);//画距形
```

![image.png](https://upload-images.jianshu.io/upload_images/6025530-978e6560c94ba814.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- skew(float sx, float sy)：sx是将画布在x方向上倾斜相应的角度，sx倾斜角度的tan值，
sy是将画布在y轴方向上倾斜相应的角度，sy为倾斜角度的tan值。也可以叠加。X = x + sx * y、Y = sy * x + y。如下：

```
mPaint.setColor(Color.BLACK);//设置画笔颜色
canvas.translate(100,100);//移动坐标系原点到（100，100）
canvas.drawRect(0,0,100,100,mPaint);//画距形
canvas.skew(1,0);//1=tan45度，在x轴方向上倾斜45度
mPaint.setColor(Color.YELLOW);//改变画笔颜色
canvas.drawRect(0,0,100,100,mPaint);//画距形
```

![image.png](https://upload-images.jianshu.io/upload_images/6025530-d342d6f661880810.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 4、Paint

上面简述了Canvas的使用，要想在Canvas上画东西，肯定不能少了Paint，也就是画笔。

Paint部分函数：

![image.png](https://upload-images.jianshu.io/upload_images/6025530-3f500a61d322efa6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

例：

```
// 定义、并创建画笔
Paint paint = new Paint();
//设置画笔颜色
paint.setColor(Color.RED);
//设置画笔模式为填充，STROKE：描边、FILL：填充、FILL_AND_STROKE：描边加填充
paint.setStyle(Paint.Style.FILL); 
//设置画笔宽度为10px
paint.setStrokeWidth(20f);
//抗锯齿功能
paint.setAntiAlias(true);
//设置画笔的样式 Paint.Cap.Round：圆形、Paint.Cap.SQUARE：方形
paint.setStrokeCap(Paint.Cap.ROUND);
//结合处为圆弧
paint.setStrokeJoin(Paint.Join.ROUND);
```

#### 5、参考

- [http://www.gcssloop.com/customview/Canvas_BasicGraphics](http://www.gcssloop.com/customview/Canvas_BasicGraphics)
- [https://developer.android.google.cn/reference/android/graphics/Canvas.html](https://developer.android.google.cn/reference/android/graphics/Canvas.html)
- [https://developer.android.google.cn/reference/android/graphics/Paint.html](https://developer.android.google.cn/reference/android/graphics/Paint.html)
