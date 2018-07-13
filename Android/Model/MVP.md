# MVP

目录：

- [简介](#简介)
- [代码](#代码)
- [链接](#链接)

## 简介

MVP架构中包括：Model（数据模型）、View（视图）和Presenter（主持者）。

- Model：对应一些DataSource、DataBean的相关对象。
- View：对应layout文件夹中的xml文件和Activity/Fragment作为视图的显示。
- Presenter：负责Model和View层之间的通信，响应View层的请求并调用Model层的数据返回给View层。

## 代码

IContract是View和Presenter之间的协议，放在contract文件夹中。

IMVPContract.java

```
package com.example.androidtest.MVP.contract;

public interface IMVPContract {
    interface View{
        void showToast(String s);
    }

    interface Presenter{
        //表示presenter被激活
        void start();
        //Button被点击调用的方法
        void BtnClick();
        //表示presenter结束
        void destroy();
    }
}
```

Model层的MVPModel.java文件放在model文件夹中。

MVPModel.java

```
package com.example.androidtest.MVP.model;

public class MVPModel {
    private static MVPModel mInstance=null;
    public static MVPModel getmInstance(){
        if(mInstance==null){
            synchronized (MVPModel.class){
                if(mInstance==null){
                    mInstance=new MVPModel();
                }
            }
        }
        return mInstance;
    }
    public String showToast(){
        return "Toast";
    }
}
```

View层的MVPActivity.java文件放在view文件夹中。

MVPActivity.java

```
package com.example.androidtest.MVP.view;

import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.example.androidtest.MVP.contract.IMVPContract;
import com.example.androidtest.MVP.presenter.MVPPresenter;
import com.example.androidtest.R;

public class MVPActivity extends AppCompatActivity implements IMVPContract.View {
    private Button mBtnMVP;
    private IMVPContract.Presenter mPresenter;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_mvp);
        //构建presenter
        mPresenter=new MVPPresenter(this);
        //初始化View
        mBtnMVP=(Button)findViewById(R.id.mvp);
        mBtnMVP.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                mPresenter.BtnClick();
            }
        });
    }

    @Override
    protected void onStart() {
        super.onStart();
        mPresenter.start();
    }

    @Override
    protected void onDestroy() {
        if(mPresenter!=null){
            mPresenter.destroy();
            mPresenter=null;
        }
        super.onDestroy();
    }
    @Override
    public void showToast(String s) {
        Toast.makeText(this,s,Toast.LENGTH_SHORT).show();
    }
}
```

Presenter层的MVPPresenter.java文件放在presenter文件夹中。

MVPPresenter.java

```
package com.example.androidtest.MVP.presenter;

import com.example.androidtest.MVP.contract.IMVPContract;
import com.example.androidtest.MVP.model.MVPModel;

public class MVPPresenter implements IMVPContract.Presenter {
    private IMVPContract.View mView;
    public MVPPresenter(IMVPContract.View view){
        mView=view;
    }
    @Override
    public void start() {

    }

    @Override
    public void BtnClick() {
        String t= MVPModel.getmInstance().showToast();
        mView.showToast(t);
    }

    @Override
    public void destroy() {
        mView=null;
    }
}
```

activity_mvp.xml

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/mvp"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="HelloMVP" />
</LinearLayout>
```

## 链接

- 代码地址：[https://github.com/lcfu1/AndroidTest/tree/master/app/src/main/java/com/example/androidtest/MVP](https://github.com/lcfu1/AndroidTest/tree/master/app/src/main/java/com/example/androidtest/MVP)

- 参考文章：[http://blog.csdn.net/yang542397/article/details/78074629](http://blog.csdn.net/yang542397/article/details/78074629)