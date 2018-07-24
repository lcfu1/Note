# Glide v4用法

目录：

- [简介](#简介)
- [开始](#开始)
- [权限](#权限)
  - [从应用中加载图片](从应用中加载图片)
  - [从Internet上加载图片](#从internet上加载图片)
  - [从本地存储中加载图片](#从本地存储中加载图片)
- [简单的例子](#简单的例子)
- [占位图](#占位图)
  - [加载占位图](#加载占位图)
  - [异常占位图](#异常占位图)
- [加载格式](#加载格式)
  - [指定加载静态图片](#指定加载静态图片)
  - [指定加载动态图片](#指定加载动态图片)
- [指定图片大小](#指定图片大小)
- [缓存机制](#缓存机制)
  - [内存缓存](#内存缓存)
  - [硬盘缓存](#硬盘缓存)
- [回调与监听](#回调与监听)
  - [into()](#into)
  - [submit()](#submit)
  - [listener()](#listener)
  - [preload()](#preload)
- [参考](#参考)

## 简介

- 一个快速高效的Android图片加载库。
- 提供了易用的API，高性能、可扩展的图片解码管道（decode pipeline），以及自动的资源池技术。 
- 支持拉取，解码和展示视频快照，图片，和GIF动画。
- github地址：[https://github.com/bumptech/glide](https://github.com/bumptech/glide)。
- 简体中文文档地址：[https://muyangmin.github.io/glide-docs-cn/](https://muyangmin.github.io/glide-docs-cn/)。

## 开始

在app/build.gradle文件中添加以下依赖：

```
implementation 'com.github.bumptech.glide:glide:4.7.1'
annotationProcessor 'com.github.bumptech.glide:compiler:4.7.1'
```

## 权限

### 从应用中加载图片

如果你要访问的数据都存储在你的应用中，则不要求任何权限。 

### 从Internet上加载图片

如果从Internet 上加载图片，则必须添加INTERNET权限到AndroidManifest.xml文件中，如下：

```
<uses-permission android:name="android.permission.INTERNET"/>
```

此外，还可以添加ACCESS_NETWORK_STATE权限，不是必需的，但是它将帮助 Glide 处理片状网络(flaky network)和飞行模式。

如果你正在从URL加载图片，Glide 可以自动帮助你处理片状网络连接：它可以监听用户的连接状态并在用户重新连接到网络时重启之前失败的请求。如果 Glide 检测到你的应用拥有ACCESS_NETWORK_STATE权限，Glide 将自动监听连接状态而不需要额外的改动。

```
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

### 从本地存储中加载图片

从本地文件夹或DCIM或图库中加载图片，则需要添加READ_EXTERNAL_STORAGE权限，如下：

```
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

如果要使用ExternalPreferredCacheDiskCacheFactory来将 Glide 的缓存存储到公有 SD 卡上，则还需要添加WRITE_EXTERNAL_STORAGE权限。

```
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```

## 简单的例子

布局文件activity_glide.xml，目前只有一个Button和ImageView。

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/load_btn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="下载图片" />
    <ImageView
        android:id="@+id/img"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

GlideActivity.java

```
package com.example.androidtest.Glide;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;

import com.bumptech.glide.Glide;
import com.example.androidtest.R;

public class GlideActivity extends AppCompatActivity implements View.OnClickListener{
    private Button mLoadBtn;
    private ImageView mImageView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_glide);

        mLoadBtn=(Button)findViewById(R.id.load_btn);
        mImageView=(ImageView)findViewById(R.id.img);

        mLoadBtn.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        if(v.getId()==R.id.load_btn){
            String url="https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
            Glide.with(this)
            	 .load(url)
            	 .into(mImageView);
        }
    }
}
```

注：这里我准备了一张图片并放在了github上，要下载下来需要添加INTERNET权限。

运行就可以看到以下效果：

![glide_1](https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_1.gif)

## 占位图

### 加载占位图

主要代码如下：

```
String url="https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
RequestOptions options=new RequestOptions()
	.placeholder(R.drawable.loading)
	.diskCacheStrategy(DiskCacheStrategy.NONE);
Glide.with(this)
	 .load(url)
	 .apply(options)
	 .into(mImageView);
```

注：

- 这里创建一个RequestOptions对象，使用placeholder()方法指定加载占位图。
- 这里使用diskCacheStrategy()方法禁掉Glide的缓存功能，以便测试。

效果如下：

![glide_2](https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_2.gif)

### 异常占位图

主要代码如下：

```
String url="https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
RequestOptions options=new RequestOptions()
	.error(R.drawable.error)
	.placeholder(R.drawable.loading)
	.diskCacheStrategy(DiskCacheStrategy.NONE);
Glide.with(this)
	 .load(url)
	 .apply(options)
	 .into(mImageView);
```

注：

- 这里只是多串接了一个error()方法指定异常占位图。

把网络关掉，测试效果如下：

![glide_3](https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_3.gif)

## 加载格式

### 指定加载静态图片

```
String url = "https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_test.gif";
Glide.with(this)
	 .asBitmap()
	 .load(url)
	 .into(mImageView);
```

注：

- 串接asBitmap()方法来指定加载静态图片。
- 如上面url指向的是一张gif图片，Glide加载出来的只是它的第一帧。

### 指定加载动态图片

```
String url = "https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
RequestOptions options=new RequestOptions()
	.error(R.drawable.error)
	.placeholder(R.drawable.loading);
Glide.with(this)
	 .asGif()
	 .load(url)
	 .apply(options)
	 .into(mImageView);
```

注：

- 串接asGif()方法来指定加载动态图片。
- 但这里url指向的是一张静态图片，所以Glide加载会出错，加载不出来。
- 把上面代码中的url指定为`https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_test.gif`，就能成功加载出来了。

## 指定图片大小

主要代码如下：

```
String url="https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
RequestOptions options=new RequestOptions()
	.override(100,100)
	.error(R.drawable.error)
	.placeholder(R.drawable.loading)
	.diskCacheStrategy(DiskCacheStrategy.NONE);
Glide.with(this)
	 .load(url)
	 .apply(options)
	 .into(mImageView);
```

注：

- 串接了一个override()方法来指定加载图片的尺寸，这里是将图片加载成100*100像素的尺寸。
- 如果想加载原图的话，在override()方法中使用Target.SIZE_ORIGINAL，如`.override(Target.SIZE_ORIGINAL)`。

## 缓存机制

### 内存缓存

Glide默认开启了。

禁用它的方法如下：

```
String url="https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
RequestOptions options=new RequestOptions()
	.skipMemoryCache(true);
Glide.with(this)
	 .load(url)
	 .apply(options)
	 .into(mImageView);
```

注：

- 使用skipMemoryCache()方法，传入true来禁用Glide的内存缓存功能。

### 硬盘缓存

```
String url="https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
RequestOptions options=new RequestOptions()
	.diskCacheStrategy(DiskCacheStrategy.NONE);
Glide.with(this)
	 .load(url)
	 .apply(options)
	 .into(mImageView);
```

注：

- 使用diskCacheStrategy()方法，传入DiskCacheStrategy.NONE来禁用Glide的硬盘缓存功能。
- 可填参数：
  - DiskCacheStrategy.NONE：不缓存任何内容。
  - DiskCacheStrategy.DATA：只缓存原始图片。
  - DiskCacheStrategy.RESOURCE：只缓存转换过后的图片。
  - DiskCacheStrategy.ALL：缓存所有图片。
  - DiskCacheStrategy.AUTOMATIC：Glide智能选择缓存策略。

## 回调与监听

### into()

into()方法可以让加载的图片显示到ImageView上。

自定义Target：

```
String url = "https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
SimpleTarget<Drawable> simpleTarget=new SimpleTarget<Drawable>() {
	@Override
	public void onResourceReady(@NonNull Drawable resource, @Nullable Transition<? super Drawable> transition) {
		mImageView.setImageDrawable(resource);
	}
};
Glide.with(this)
	 .load(url)
	 .into(simpleTarget);
```

注：

- 创建一个SimpleTarget的实例，并指定为Drawable泛型。
- 重写onResourceReady()方法来获取加载出来的图片对象。
- 把它传入into()方法中。

### submit()

使用submit()方法来下载图片。

```
new Thread(new Runnable() {
	@Override
	public void run() {
		try {
			String url = "https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
			final Context context=getApplicationContext();
			FutureTarget<File> target=Glide.with(context).asFile().load(url).submit();
			final File file=target.get();
			runOnUiThread(new Runnable() {
				@Override
				public void run() {
					Bitmap bitmap= BitmapFactory.decodeFile(file.getPath());
                      mImageView.setImageBitmap(bitmap);
                  }
              });
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}).start();
```

注：

- 调用submit()方法后会返回一个FutureTarget对象，开始下载图片。
- 在子线程中下载图片，不然图片还没下载完成就使用get()方法会阻塞线程。
- 下载完成后，使用FutureTarget的get()方法来获取下载下来的图片文件。
- 这里使用runOnUiThread()回到主线程并显示图片到ImageView上。

### listener()

listener()方法可以结合into()和preload()方法使用。

```
String url = "https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
Glide.with(this)
	 .load(url)
	 .listener(new RequestListener<Drawable>() {
	 	@Override
	 	public boolean onLoadFailed(@Nullable GlideException e, Object model, Target<Drawable> target, boolean isFirstResource) {
	 		Toast.makeText(GlideActivity.this,e+"---onLoadFailed",Toast.LENGTH_SHORT).show();
	 			return false;
	 	}
	 	@Override
	 	public boolean onResourceReady(Drawable resource, Object model, Target<Drawable> target, DataSource dataSource, boolean isFirstResource) {
	 		Toast.makeText(GlideActivity.this,resource+"---onResourceReady",Toast.LENGTH_SHORT).show();
		 	return false;
	  	}
      })
	 .into(mImageView);
```

注：

- 串接listener()方法并实现一个RequestListener的实例。
- RequestListener需要实现两个方法：
  - onLoadFailed()：图片加载失败的时候回调。
  - onResourceReady()：图片加载完成的时候回调。

### preload()

用preload()方法来预加载图片，等需要用的时候Glide就会直接从缓存中获取图片。

预加载图片：

```
String url = "https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
Glide.with(this)
	 .load(url)
	 .listener(new RequestListener<Drawable>() {
	 	@Override
		 public boolean onLoadFailed(@Nullable GlideException e, Object model, Target<Drawable> target, boolean isFirstResource) {
		 	Toast.makeText(GlideActivity.this, "onLoadFailed", Toast.LENGTH_SHORT).show();
		 		return false;
		 	}
		 @Override
		 public boolean onResourceReady(Drawable resource, Object model, Target<Drawable> target, DataSource dataSource, boolean isFirstResource) {
		 	Toast.makeText(GlideActivity.this, "onResourceReady", Toast.LENGTH_SHORT).show();
		 		return false;
		 	}
		})
	 .preload();
```

使用图片：

```
String url = "https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
Glide.with(this)
	 .load(url)
	 .into(mImageView);
```

注：

- 上面使用preload()方法来预加载图片。
- preload()方法有两个方法重载：
  - preload()：不带参，加载图片的原始尺寸。
  - preload(int width,int height)：带参，指定加载图片的宽高。
- 这里串接listener()方法来测试是否下载成功。

## 参考

- [https://blog.csdn.net/guolin_blog/article/details/78582548](https://blog.csdn.net/guolin_blog/article/details/78582548)