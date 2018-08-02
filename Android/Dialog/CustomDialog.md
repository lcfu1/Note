# CustomDialog

自定义Dialog

目录：

- [效果](#效果)
- [实现](#实现)
- [使用](#使用)

## 效果

![CustomDialog](https://raw.githubusercontent.com/lcfu1/Note/master/Android/image/CustomDialog.gif)

## 实现

CustomDialog.java

```
package com.example.androidtest.Dialog;

import android.app.Dialog;
import android.content.Context;
import android.graphics.Point;
import android.os.Bundle;
import android.support.annotation.NonNull;
import android.text.TextUtils;
import android.view.Display;
import android.view.View;
import android.view.WindowManager;
import android.widget.TextView;

import com.example.androidtest.R;

public class CustomDialog extends Dialog implements View.OnClickListener {
    private String title, content, cancel, confirm;
    private IOnCancelListener mCancelListener;
    private IOnConfirmListener mConfirmListener;

    CustomDialog(@NonNull Context context) {
        super(context);
    }

    public CustomDialog setTitle(String title) {
        this.title = title;
        return this;
    }

    public CustomDialog setContent(String content) {
        this.content = content;
        return this;
    }

    public CustomDialog setCancel(String cancel, IOnCancelListener cancelListener) {
        this.cancel = cancel;
        mCancelListener = cancelListener;
        return this;
    }

    public CustomDialog setConfirm(String confirm, IOnConfirmListener confirmListener) {
        this.confirm = confirm;
        mConfirmListener = confirmListener;
        return this;
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.layout_custom_dialog);

        WindowManager windowManager=getWindow().getWindowManager();
        Display display=windowManager.getDefaultDisplay();
        WindowManager.LayoutParams params=getWindow().getAttributes();
        Point point=new Point();
        display.getSize(point);
        params.width=(int)(point.x * 0.8);
        getWindow().setAttributes(params);

        TextView title1 = (TextView) findViewById(R.id.title);
        TextView content1 = (TextView) findViewById(R.id.content);
        TextView cancel1 = (TextView) findViewById(R.id.cancel);
        TextView confirm1 = (TextView) findViewById(R.id.confirm);

        if (!TextUtils.isEmpty(title)) {
            title1.setText(title);
        }
        if (!TextUtils.isEmpty(content)) {
            content1.setText(content);
        }
        if (!TextUtils.isEmpty(cancel)) {
            cancel1.setText(cancel);
        }
        if (!TextUtils.isEmpty(confirm)) {
            confirm1.setText(confirm);
        }
        cancel1.setOnClickListener(this);
        confirm1.setOnClickListener(this);
    }

    @Override
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.cancel:
                if (mCancelListener != null) {
                    mCancelListener.onCancel(this);
                }
                break;
            case R.id.confirm:
                if (mConfirmListener != null) {
                    mConfirmListener.onConfirm(this);
                }
                break;
        }
    }

    public interface IOnCancelListener {
        void onCancel(CustomDialog dialog);
    }

    public interface IOnConfirmListener {
        void onConfirm(CustomDialog dialog);
    }
}
```

layout_custom_dialog.xml

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_horizontal"
    android:orientation="vertical"
    android:background="@drawable/dialog_bg">

    <TextView
        android:id="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="20dp"
        android:text="提示"
        android:textSize="20sp"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/content"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="20dp"
        android:text="内容" />

    <View
        android:layout_width="match_parent"
        android:layout_height="0.5dp"
        android:background="#000000" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:orientation="horizontal">

        <TextView
            android:id="@+id/cancel"
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:gravity="center"
            android:text="取消"
            android:textColor="#22d761" />
        <View
            android:layout_width="0.5dp"
            android:layout_height="match_parent"
            android:background="#000000"/>
        <TextView
            android:id="@+id/confirm"
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:gravity="center"
            android:text="确定"
            android:textColor="#22d761" />
    </LinearLayout>
</LinearLayout>
```

注：上面就是自定义dialog的主要实现代码和布局文件。

## 使用

```
final CustomDialog customDialog=new CustomDialog(DialogActivity.this);
	customDialog.setTitle("标题")
		.setContent("内容")
		.setCancel("取消", new CustomDialog.IOnCancelListener() {
			@Override
			public void onCancel(CustomDialog dialog) {
				Toast.makeText(DialogActivity.this,"取消",Toast.LENGTH_SHORT).show();
				customDialog.dismiss();
			}
		})
		.setConfirm("正确", new CustomDialog.IOnConfirmListener() {
			@Override
			public void onConfirm(CustomDialog dialog) {
				Toast.makeText(DialogActivity.this,"正确",Toast.LENGTH_SHORT).show();
				customDialog.dismiss();
			}
		})
		.show();
```

