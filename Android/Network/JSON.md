# 解析JSON格式的数据

目录：

- [测试数据](#测试数据)

- [使用JSONObject](#使用jsonobject)
- [使用GSON](#使用gson)

- [例子](#例子)



## 测试数据

测试数据我放在github上的。

地址：[https://raw.githubusercontent.com/lcfu1/test/master/json_data.json](https://raw.githubusercontent.com/lcfu1/test/master/json_data.json)

内容：

```
[
  {
    "id":"1",
    "name":"AA"
  },
  {
    "id":"2",
    "name":"BB"
  }
]
```



## 使用JSONObject

简单用法：

1. 返回的数据是一个JSON数组，把返回的数据传入一个JSONArray对象中。
2. 循环遍历JSONArray，并得到JSONObject对象。
3. 调用JSONObject的getString()方法得到解析出来的数据，并打印出来。

## 使用GSON

简单用法：

1. 添加以下依赖：

   ```
   implementation 'com.google.code.gson:gson:2.8.5'
   ```

2. 定义一个App类，GSON库把JSON数据映射成一个对象。

3. 解析：

   ```
   //解析一个对象
   App app=gson.fromJson(responseData,App.class);
   
   //解析一段JSON数组
   List<App> appList=gson.fromJson(responseData,new TypeToken<List<App>>(){}.getType());
   ```

4. 循环遍历appList，并打印。

## 例子

JSONActivity.java

```
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;

import org.json.JSONArray;
import org.json.JSONObject;

import java.net.URL;
import java.util.List;

import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class JSONActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_json);
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
                    URL url=new URL("https://raw.githubusercontent.com/lcfu1/test/master/json_data.json");
                    OkHttpClient client=new OkHttpClient();
                    Request request=new Request.Builder().url(url).build();
                    Response response=client.newCall(request).execute();
                    String responseData=response.body().string();

                    //parseJSONWithJSONObject(responseData);
                    parseJSONWithGSON(responseData);
                }catch (Exception e){
                    e.printStackTrace();
                }
            }
        }).start();
    }

    private void parseJSONWithGSON(String responseData) {
        Gson gson=new Gson();
        //App app=gson.fromJson(responseData,App.class);
        List<App> appList=gson.fromJson(responseData,new TypeToken<List<App>>(){}.getType());
        for(App app:appList){
            LogUtil.d("JSONActivity----------id=", app.getId());
            LogUtil.d("JSONActivity----------name=", app.getName());
        }
    }

    private void parseJSONWithJSONObject(String responseData) {
        try{
            JSONArray jsonArray=new JSONArray(responseData);
            for(int i=0;i<jsonArray.length();i++){
                JSONObject jsonObject=jsonArray.getJSONObject(i);
                String id=jsonObject.getString("id");
                String name=jsonObject.getString("name");
                LogUtil.d("JSONActivity----------id=", id);
                LogUtil.d("JSONActivity----------name=", name);
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```



App.java

```
public class App {
    private String id;
    private String name;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```



activity_json.xml

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
</LinearLayout>
```



两种方式打印的结果一样，如效果截图：

![json](https://raw.githubusercontent.com/lcfu1/Image/master/Android/json.PNG)

