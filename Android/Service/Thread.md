# 线程

目录：

- [创建和启动线程的方式](#创建和启动线程的方式)
- [异步消息处理机制](#异步消息处理机制)
- [AsyncTask](#asynctask)

## 创建和启动线程的方式

### 继承Thread

用继承的方式定义一个线程只需要新建一个类继承自Thread，重写run()方法，并在run()方法中编写耗时逻辑，如：

```
class MyThread extends Thread(){
    @Override
    public void run(){
        //耗时逻辑
    }
}
```

启动上面方法的方法，如：

```
new MyThread().start();
```

也就是new出MyThread的实例，并调用start()方法，不过使用继承的方式耦合性高。

### Runnable接口

用Runnable接口的方式来定义一个线程，如：

```
class MyThread implements Runnable{
    @Override
    public void run(){
        //耗时逻辑
    }
}
```

启动方法如：

```
MyThread myThread=new MyThread();
new Thread(myThread).start();
```

还可以使用匿名类的方式来写，如：

```
new Thread(new Runnable(){
    @Override
    public void run(){
        //耗时逻辑
    }
}).start();
```

## 异步消息处理机制

### 解析

主要由4个部分组成：

- Message：

  在线程之间传递的消息，有what字段，obj字段(携带一个对象)，arg1、arg2字段(携带一些整型数据)。

- Handler：

  用于发送消息(一般用sendMessage()方法)和处理消息(handleMessage()方法)。

- MessageQueue：

  消息队列，用来存放所有通过Handler发送的消息，每个线程中只有一个MessageQueue。

- Looper：

  MessageQueue的管家，调用Looper的loop()方法并进入到一个无限循环当中，每当MessageQueue中存在一条消息，就会将它取出并传递到Handler的handleMessage()方法中，每个线程中也只有一个Looper对象。

### 例子1

MainActivity.java

```
package com.java.androidservice;

import android.os.Handler;
import android.os.Message;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    public static final int UPDATE_TEXT=1;
    private TextView mTextView;
    private Handler mHandler=new Handler(){
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what){
                case UPDATE_TEXT:
                    mTextView.setText("text");
                    break;
                default:
                    break;
            }
        }
    };
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mTextView=(TextView)findViewById(R.id.text);
        mTextView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        Message message=new Message();
                        message.what=UPDATE_TEXT;
                        mHandler.sendMessage(message);
                    }
                }).start();
            }
        });
    }
}
```

activity_main.xml

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</android.support.constraint.ConstraintLayout>
```

创建一个Message对象，并将what字段的值指定为UPDATE_TEXT，调用Handler的sendMessage()方法来发送，Handler收到这条Message后在handleMessage()中处理。

**注**：handleMessage()方法中的代码是在主线程中运行的。

**效果**：点击“Hello World!”就会更新为text。

### 例子2

```
package com.java.androidservice;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    private TextView mTextView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mTextView=(TextView)findViewById(R.id.text);
        mTextView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        mTextView.setText("text");
                    }
                });
            }
        });
    }
}
```

使用runOnUiThread()方法来更新UI操作，它是异步消息处理机制的接口封装。

## AsyncTask

AsyncTask是一个抽象类，通过创建一个子类继承它来使用，有3个泛型参数：

1. Params：

   在执行AsyncTask时需要传入的参数，可用在后台任务中。

2. Progress：

   后台任务执行时，如需要在界面上显示当前的进度，则可以使用这里指定的泛型作为进度单位。

3. Result：

   当任务执行完毕后，使用这里指定的泛型可以对结果进行返回。

另外需要重写的方法如下：

1. onPreExecute：在后台任务开始执行前调用，可用于进行一些界面上的初始化操作。

2. doInBackground(Params...)：里面的代码会在子线程中运行，可在里面进行耗时任务，任务完成后可通过return返回执行结果，还可调用publishProgress(Progress...)方法来更新执行进度。

3. onProgressUpdate(Progress...)：当后台任务中调用publishProgress(Progress...)方法后，onProgressUpdate(Progress...)方法就会很快被调用，它携带的参数就是后台任务中传递过来的，可在这里对UI进行操作。

4. onPostExecute(Result)：当后台任务执行完毕并通过return返回时，就会调用这个方法，返回的数据Result会作为参数传递给它，还可利用Result进行UI操作。

   