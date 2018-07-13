# 动态修改主题

目录：

- [实现代码](#实现代码)
- [效果](#效果)
- [链接](#链接)

## 实现代码

在values中新建一个attrs.xml

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <attr name="button_text_color" format="reference|color" />
</resources>
```

在styles.xml中自定义theme

```
<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="button_text_color">#ffffff</item>
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

    <style name="RedTheme" parent="AppTheme">
        <item name="button_text_color">#ffffff</item>
        <item name="colorPrimary">#ed4747</item>
        <item name="colorPrimaryDark">#ed4747</item>
        <item name="colorAccent">#ed4747</item>
    </style>

    <style name="GreyTheme" parent="AppTheme">
        <item name="button_text_color">#000000</item>
        <item name="colorPrimary">#bcb6b6</item>
        <item name="colorPrimaryDark">#bcb6b6</item>
        <item name="colorAccent">#bcb6b6</item>
    </style>

</resources>
```

activity_set_theme.xml

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/red"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="红色"
        android:textColor="?button_text_color"/>
    <Button
        android:id="@+id/grey"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="灰色"
        android:textColor="?button_text_color"/>
</LinearLayout>
```

SetThemeActivity.java

```
package com.example.androidtest.Theme;

import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;
import com.example.androidtest.R;

public class SetThemeActivity extends AppCompatActivity {
    private Button mSetRedButton;
    private Button mSetGreyButton;
    private static int sTheme;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //必须在setContentView之前实现
        if(sTheme!=0){
            setTheme(sTheme);
        }
        setContentView(R.layout.activity_set_theme);

        mSetRedButton=(Button)findViewById(R.id.red);
        mSetGreyButton=(Button)findViewById(R.id.grey);

        mSetRedButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                sTheme=R.style.RedTheme;
                recreate();
            }
        });
        mSetGreyButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                sTheme=R.style.GreyTheme;
                recreate();
            }
        });
    }
}
```

这里我选择AppTheme做为SetThemeActivity默认主题，所以在AndroidManifest.xml中如下

```
<activity android:name=".Theme.SetThemeActivity"/>
```

不需要设置android:theme，但在styles.xml中必须加上attrs.xml中定义的属性

```
<item name="button_text_color">#ffffff</item>
```

如果选择RedTheme或GreyTheme作为SetThemeActivity的默认主题，则在AndroidManifest.xml中如下

```
<activity android:name=".Theme.SetThemeActivity" android:theme="@style/RedTheme"/>
```

## 效果

![setTheme](https://raw.githubusercontent.com/lcfu1/Image/master/Android/setTheme.gif)

## 链接

- 代码地址：[https://github.com/lcfu1/AndroidTest/tree/master/app/src/main/java/com/example/androidtest/Theme](https://github.com/lcfu1/AndroidTest/tree/master/app/src/main/java/com/example/androidtest/Theme)