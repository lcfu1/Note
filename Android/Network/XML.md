# 解析XML格式的数据

目录：

- [Pull解析方式](#Pull解析方式)
- [SAX解析方式](#SAX解析方式)

## Pull解析方式

简单用法：

1. 先通过OkHttp获取服务器返回的数据。

2. 通过parseXMLWithPull()方法解析返回的数据。

3. 获取一个XmlPullParserFactory实例，然后再得到XmlPullParser对象，并设置返回的数据给XmlPullParser的setInput()方法。

   ```
   XmlPullParserFactory factory = XmlPullParserFactory.newInstance();
   XmlPullParser xmlPullParser = factory.newPullParser();
   xmlPullParser.setInput(new StringReader(responseData));
   ```

4. 通过XmlPullParser的getEventType()方法得到当前的解析事件，然后进行while循环判断。

5. 开始解析某个节点，如果节点名符合条件的，就调用XmlPullParser的nextText()方法来获取节点内的内容。

6. 解析完后显示出来和通过LogUtil工具类打印出来，调用next()方法获取下一个解析事件，如果解析事件不等于XmlPullParser.END_DOCUMENT就继续重复以上解析工作。

XMLActivity.java

```
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

import org.xmlpull.v1.XmlPullParser;
import org.xmlpull.v1.XmlPullParserFactory;

import java.io.StringReader;
import java.net.URL;

import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class XMLActivity extends AppCompatActivity {
    TextView text;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_xml);

        text = (TextView) findViewById(R.id.text);

        Button get = (Button) findViewById(R.id.get_btn);
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
                try {
                    URL url = new URL("https://raw.githubusercontent.com/lcfu1/test/master/xml_data.xml");
                    OkHttpClient client = new OkHttpClient();
                    Request request = new Request.Builder().url(url).build();
                    Response response = client.newCall(request).execute();
                    String responseData = response.body().string();
                    parseXMLWithPull(responseData);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }

    private void parseXMLWithPull(String responseData) {
        try {
            XmlPullParserFactory factory = XmlPullParserFactory.newInstance();
            XmlPullParser xmlPullParser = factory.newPullParser();
            xmlPullParser.setInput(new StringReader(responseData));
            String id = "";
            String name = "";
            StringBuilder s = new StringBuilder();
            int eventType = xmlPullParser.getEventType();
            while ((eventType != XmlPullParser.END_DOCUMENT)) {
                String nodeName = xmlPullParser.getName();
                switch (eventType) {
                    case XmlPullParser.START_TAG: {
                        if ("id".equals(nodeName)) {
                            id = xmlPullParser.nextText();
                        } else if ("name".equals(nodeName)) {
                            name = xmlPullParser.nextText();
                        }
                        break;
                    }
                    case XmlPullParser.END_TAG: {
                        if ("app".equals(nodeName)) {
                            s.append("id=" + id + "  name=" + name + '\n');
                            showResponse(s.toString());
                            LogUtil.d("XMLActivity----------id=", id);
                            LogUtil.d("XMLActivity----------name=", name);
                        }
                        break;
                    }
                    default:
                        break;
                }
                eventType = xmlPullParser.next();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
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

activity_xml.xml

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
        android:text="parseXMLWithPull" />
    <TextView
        android:id="@+id/text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
</LinearLayout>
```

效果截图：

![parseXMLWithPull](https://raw.githubusercontent.com/lcfu1/Image/master/Android/parseXMLWithPull.PNG)

## SAX解析方式

