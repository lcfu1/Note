# Glide v4用法

目录：

- 

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
            Glide.with(this).load(url).into(mImageView);
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
Glide.with(this).load(url).apply(options).into(mImageView);
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
Glide.with(this).load(url).apply(options).into(mImageView);
```

注：

- 这里只是多串接了一个error()方法指定异常占位图。

把网络关掉，测试效果如下：

![glide_3](https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_3.gif)

## 指定图片大小

主要代码如下：

```
String url="https://raw.githubusercontent.com/lcfu1/Image/master/Android/glide_logo.png";
RequestOptions options=new RequestOptions()
	.override(100,100)
	.error(R.drawable.error)
	.placeholder(R.drawable.loading)
	.diskCacheStrategy(DiskCacheStrategy.NONE);
Glide.with(this).load(url).apply(options).into(mImageView);
```

注：

- 串接了一个override()方法来指定加载图片的尺寸，这里是将图片加载成100*100像素的尺寸。
- 如果想加载原图的话，在override()方法中使用Target.SIZE_ORIGINAL，如`.override(Target.SIZE_ORIGINAL)`。