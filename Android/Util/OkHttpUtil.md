# OkHttpUtil

目录：

- [OkHttpUtil](#okhttputil)
- [使用](#使用)

## OkHttpUtil

这是一个基于OkHttp网络请求的公共类，可以重复使用，提高效率。

OkHttpUtil.java

```
import okhttp3.Callback;
import okhttp3.OkHttpClient;
import okhttp3.Request;

public class OkHttpUtil {
    public static void sendHttpRequest(String address, Callback callback) {
        OkHttpClient client=new OkHttpClient();
        Request request=new Request.Builder().url(address).build();
        client.newCall(request).enqueue(callback);
    }
}
```

解析：

- Callback是OkHttp自带的一个回调接口。
- 创建一个Request对象，并设置。
- 把Callback的参数传入enqueue()方法中，会在enqueue()方法的子 线程中去执行HTTP请求，并把请求的结果回调给Callback。

## 使用

使用方法如下：

```
OkHttpUtil.sendHttpRequest("https://www.baidu.com", new Callback() {
		@Override
         public void onResponse(Call call, Response response) throws IOException {
         		String data = response.body().string();
         }

         @Override
         public void onFailure(Call call, IOException e) {

         }
});
```

解析：

- 这里给sendHttpRequest()方法传入了一个url，和一个Callback的实例。
- onResponse()方法返回的就是请求的结果，onFailure()方法返回异常信息。