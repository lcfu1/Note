## 读取URL中的资源

URL类在java.net包中。

一个URL对象通常包含：

- 协议

  - 必须是URL对象所在的java虚拟机支持的协议，Http、Ftp、File协议等都是虚拟机支持的协议。

- 地址

  - 必须是能连接的有效域名或IP地址。

- 资源

  - 可以是主机上的任何一个文件。

URL的构造方法：

```
1、public URL (String spec)throws MalformedURLException//初始化一个URL对象
2、public URL (String protocol,String host,String file)throws MalformedURLException//protocol协议、host地址
```

例子：

```
import java.net.*;
import java.io.*;
class Look implements Runnable{
	URL url;
	public void setURL(URL url) {
		this.url=url;
	}
	public void run() {
		try {
			InputStream in=url.openStream();
			byte[] b=new byte[1024];
			int n=-1;
			while((n=in.read(b))!=-1) {
				String str=new String(b,0,n);
				System.out.println(str);
			}
		}catch(IOException exp) {
			
		}
	}
}
public class URL1 {
	public static void main(String args[]) {
		URL url;
		Thread readURL;
		Look look=new Look();
		try {
			url=new URL("https://lcfu1.github.io");
			look.setURL(url);
			readURL=new Thread(look);
			readURL.start();
		}catch(Exception exp) {
			System.out.println(exp);
		}
	}
}
```

