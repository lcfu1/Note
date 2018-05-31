# 解析XML格式的数据

目录：

- [测试数据](#测试数据)

- [Pull解析方式](#pull解析方式)
- [SAX解析方式](#sax解析方式)
- [例子](#例子)



## 测试数据

测试数据我放在github上的。

地址：[https://raw.githubusercontent.com/lcfu1/test/master/xml_data.xml](https://raw.githubusercontent.com/lcfu1/test/master/xml_data.xml)

内容：

```
<apps>
  <app>
    <id>1</id>
    <name>AA</name>
  </app>
  <app>
    <id>2</id>
    <name>BB</name>
  </app>
</apps>
```



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

6. 解析完后通过LogUtil工具类打印出来，调用next()方法获取下一个解析事件，如果解析事件不等于XmlPullParser.END_DOCUMENT就继续重复以上解析工作。



## SAX解析方式

简单用法：

1. 新建一个SAXContentHandler继承DefaultHandler，并重写父类的5 个方法。
2. startDocument()初始化，startElement()开始解析某个节点，characters()解析节点的具体内容，在endElement()中进行判断，如果节点解析完成，就打印出来，并把StringBuilder的内容清空。
3. 获取一个SAXParserFactory实例，然后再得到xmlReader对象，使用setContentHandler()方法把handler设置到XMLReader中，最后使用XMLReader的parse()方法进行解析。



## 例子

XMLActivity.java

```
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import org.xml.sax.InputSource;
import org.xml.sax.XMLReader;
import org.xmlpull.v1.XmlPullParser;
import org.xmlpull.v1.XmlPullParserFactory;

import java.io.StringReader;
import java.net.URL;

import javax.xml.parsers.SAXParserFactory;

import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class XMLActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_xml);

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

                    //parseXMLWithPull(responseData);
                    parseXMLWithSAX(responseData);

                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }

    //SAX解析
    private void parseXMLWithSAX(String responseData) {
        try {
            SAXParserFactory factory = SAXParserFactory.newInstance();
            XMLReader xmlReader = factory.newSAXParser().getXMLReader();
            SAXContentHandler handler = new SAXContentHandler();
            xmlReader.setContentHandler(handler);
            xmlReader.parse(new InputSource(new StringReader(responseData)));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //Pull解析
    private void parseXMLWithPull(String responseData) {
        try {
            XmlPullParserFactory factory = XmlPullParserFactory.newInstance();
            XmlPullParser xmlPullParser = factory.newPullParser();
            xmlPullParser.setInput(new StringReader(responseData));
            String id = "";
            String name = "";
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
}
```

SAXContentHandler.java

```
import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

public class SAXContentHandler extends DefaultHandler{
    private String nodeName;
    private StringBuilder id;
    private StringBuilder name;
    @Override
    public void startDocument() throws SAXException {
        super.startDocument();
        id=new StringBuilder();
        name=new StringBuilder();
    }

    @Override
    public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
        super.startElement(uri, localName, qName, attributes);
        nodeName=localName;
    }

    @Override
    public void characters(char[] ch, int start, int length) throws SAXException {
        super.characters(ch, start, length);
        if("id".equals(nodeName)){
            id.append(ch, start, length);
        }else if("name".equals(nodeName)){
            name.append(ch, start, length);
        }
    }

    @Override
    public void endElement(String uri, String localName, String qName) throws SAXException {
        super.endElement(uri, localName, qName);
        if("app".equals(localName)){
            LogUtil.d("XMLActivity----------id=", id.toString().trim());
            LogUtil.d("XMLActivity----------name=", name.toString().trim());
            id.setLength(0);
            name.setLength(0);
        }
    }

    @Override
    public void endDocument() throws SAXException {
        super.endDocument();
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
        android:text="get" />
</LinearLayout>
```

两种方式打印的结果一样，如效果截图：

![parseXMLWithPull](https://raw.githubusercontent.com/lcfu1/Image/master/Android/parseXMLWithPull.PNG)

