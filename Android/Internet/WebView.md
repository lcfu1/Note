# WebView

目录：

- [简单的使用例子](#简单的使用例子)

使用WebView控件，我们可以在自己的应用里嵌入一个浏览器，用来浏览网页。

### 简单的使用例子

*activity_main.xml*

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".WebViewActivity">

    <WebView
        android:id="@+id/web_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</android.support.constraint.ConstraintLayout>
```

在这里使用了WebView控件，并设置它的id为web_view。

*MainActivity.java*

```
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.webkit.WebView;
import android.webkit.WebViewClient;

public class WebViewActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_web_view);

        WebView webView=(WebView)findViewById(R.id.web_view);
        webView.getSettings().setJavaScriptEnabled(true);
        webView.setWebViewClient(new WebViewClient());
        webView.loadUrl("http://www.baidu.com");
    }
}
```

先获取WebView实例，使用getSettings()方法来设置浏览器的属性，setJavaScriptEnabled(true)让WebView支持JavaScript脚本，setWebViewClient(new WebViewClient())使用setWebViewClient()方法并传入一个WebViewClient实例。

*AndroidManifest.xml*

```
<uses-permission android:name="android.permission.INTERNET"/>
```

**注**：要打开网页肯定需要网络，所以还要在AndroidManifest.xml文件中加入以上代码来**声明权限**。