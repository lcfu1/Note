# OkHttp

目录：

- [使用](#使用)
- [简单用法](#简单用法)
- [例子](#例子)

## 使用

OkHttp的github主页地址：[https://github.com/square/okhttp](https://github.com/square/okhttp)

添加OkHttp库的依赖：

```
implementation 'com.squareup.okhttp3:okhttp:3.10.0'
```

然后Sync Now。

## 简单用法

1. 添加OkHttp库的依赖后会自动下载两个库：OkHttp库和Okio库，Okio库是OkHttp库的通信基础。

2. 创建一个OkHttpClient实例，如：

   ```
   OkHttpClient client=new OkHttpClient();
   ```

3. 创建一个Request对象，这里还通过连缀的方式来丰富Request对象，如：

   ```
   URL url=new URL("http://www.baidu.com");
   Request request=new Request.Builder().url(url).build();
   ```

4. 调用OkHttpClient的newCall()方法来创建一个call对象，并调用它的execute()方法来发送请求并获取服务器返回的数据，如：

   ```
   Response response=client.newCall(request).execute();
   String responseData=response.body().string();  //responseData得到的就是返回的具体内容。
   ```

5. 以上就是GET请求，POST请求：先构建一个RequestBody，然后在Request.Builder()中调用post()方法，并传入RequestBody对象，最后调用execute()方法来发送请求并获取服务器返回的数据，如：

   ```
   RequestBody  requestBody=new FormBody.Builder()
   			.add("username","lcfu1")
   			.add("password","111")
   			.build();
   
   URL url=new URL("http://www.baidu.com");
   Request request=new Request.Builder().url(url).post(requestBody).build();
   ```


## 例子

**OkHttpActivity.java**

```
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import java.net.URL;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class OkHttpActivity extends AppCompatActivity {
    TextView text;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_ok_http);

        text=(TextView)findViewById(R.id.text);

        Button get=(Button)findViewById(R.id.get_btn);
        get.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                sendRequest();
            }
        });
    }

    private void sendRequest() {
        new Thread(new Runnable() {
            @Override
            public void run() {
                try{
                    URL url=new URL("https://raw.githubusercontent.com/lcfu1/test/master/get_html.html");
                    OkHttpClient client=new OkHttpClient();
                    Request request=new Request.Builder().url(url).build();
                    Response response=client.newCall(request).execute();
                    String responseData=response.body().string();
                    LogUtil.d("OkHttpActivity----------",responseData);
                    showResponse(responseData);
                }catch (Exception e){
                    e.printStackTrace();
                }
            }
        }).start();
    }
    private void showResponse(final String response) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                text.setText(response);
            }
        });
    }
}
```

**activity_main.xml**

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/get_btn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="get" />
    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <TextView
            android:id="@+id/text"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />
    </ScrollView>
</LinearLayout>
```

