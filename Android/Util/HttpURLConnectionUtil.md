# HttpURLConnectionUtil

目录：

- [HttpCallbackListener接口](#httpcallbacklistener接口)

- [HttpURLConnectionUtil](#httpurlconnectionutil)
- [使用](#使用)

## HttpCallbackListener接口

这是一个基于HttpURLConnection网络请求的公共类，可以重复使用，提高效率。

首先定义一个接口，如下：

```
public interface HttpCallbackListener {
    void onFinish(String response);
    void onError(Exception e);
}
```

解析：

- 这里定义了两个方法，onFinish()表示当成功响应时调用，onError()表示操作出现错误时调用。

## HttpURLConnectionUtil

HttpURLConnectionUtil.java

```
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class HttpURLConnectionUtil {
    public static void sendHttpRequest(final String address,final HttpCallbackListener listener){
        new Thread(new Runnable() {
            @Override
            public void run() {
                HttpURLConnection connection=null;
                try{
                    URL url=new URL(address);
                    connection=(HttpURLConnection)url.openConnection();
                    connection.setRequestMethod("GET");
                    connection.setConnectTimeout(8000);
                    connection.setReadTimeout(8000);
                    connection.setDoInput(true);
                    connection.setDoOutput(true);
                    InputStream inputStream=connection.getInputStream();
                    BufferedReader reader=new BufferedReader(new InputStreamReader(inputStream));
                    StringBuilder response=new StringBuilder();
                    while(reader.readLine()!=null){
                        response.append(reader.readLine());
                    }
                    if(listener!=null){
                        listener.onFinish(response.toString());
                    }
                }catch (Exception e){
                    if(listener!=null){
                        listener.onError(e);
                    }
                }finally {
                    if(connection!=null){
                        connection.disconnect();
                    }
                }
            }
        }).start();
    }
}
```

解析：

- 关于HttpURLConnection的知识请看[HttpURLConnection](Network/HttpURLConnection.md)。

## 使用

使用方法如下：

```
HttpURLConnectionUtil.sendHttpRequest("https://www.baidu.com", new HttpCallbackListener() {
		@Override
         public void onFinish(String response) {
				showResponse(response);
         }

         @Override
         public void onError(Exception e) {

         }
});
```

解析：

- 这里给sendHttpRequest()方法传入了一个url，和HttpCallbackListener的实例。
- 在sendHttpRequest()方法内部开启一个子线程，并在子线程中执行网络操作。
- onFinish()方法得到响应的数据，onError()方法得到异常信息。