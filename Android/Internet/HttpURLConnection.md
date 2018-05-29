# HttpURLConnection

目录：

- [简单用法](#简单用法)
- [例子](#例子)

Android上发送HTTP请求的两种方式：HttpURLConnection和HttpClient，但HttpClient在Android6.0中被移除了。

## 简单用法

1. 首先new出一个URL对象，并传入网址，然后获得HttpURLConnection实例，再调用openConnection()方法，如：

   ```
   URL url=new URL("http://www.baidu.com");
   HttpURLConnection connection=(HttpURLConnection)url.openConnection();
   ```

2. 使用setRequestMethod()方法来设置HTTP请求的方法：GET或POST，如：

   ```
   connection.setRequestMethod("GET");
   ```

3. 使用setConnectTimeout()方法来设置连接超时的毫秒数，如：

   ```
   connection.setConnectTimeout(10000);
   ```

4. 使用setReadTimeout()方法来设置读取超时的毫秒数，如：

   ```
   connection.setReadTimeout(10000);
   ```

5. 调用getInputStream()方法来获取返回的输入流，如：

   ```
   InputStream in=connection.getInputStream();
   ```

6. 获得输入流后要对其进行读取。

7. 最后使用disconnect()方法来关掉HTTP连接，如：

   ```
   connection.disconnect();
   ```

注：POST请求方法：在获得输入流之前写好要提交的数据，每条数据都要以键值对的形式存在，数据之间用&符号隔开，如：

```
connection.setRequestMethod("POST");
DataOutputStream out=new DataOutputStream(connection.getOutputStream());
out.writeBytes("username=lcfu1&password=111");
```

## 例子

**HttpURLConnectionActivity.java**

```
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class HttpURLConnectionActivity extends AppCompatActivity {
    TextView text;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_http_urlconnection);

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
                HttpURLConnection connection=null;
                BufferedReader reader=null;
                try{
                    URL url=new URL("http://www.baidu.com");
                    connection=(HttpURLConnection)url.openConnection();
                    connection.setRequestMethod("GET");
                    connection.setConnectTimeout(8000);
                    connection.setReadTimeout(8000);
                    InputStream inputStream=connection.getInputStream();
                    reader=new BufferedReader(new InputStreamReader(inputStream));
                    StringBuilder response=new StringBuilder();
                    while(reader.readLine()!=null){
                        response.append(reader.readLine());
                    }
                    showResponse(response.toString());
                }catch (Exception e){
                    e.printStackTrace();
                }finally {
                    if(reader!=null){
                        try{
                            reader.close();
                        }catch (IOException e){
                            e.printStackTrace();
                        }
                    }
                    if(connection!=null){
                        connection.disconnect();
                    }
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

