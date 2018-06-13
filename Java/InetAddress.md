# InetAddress类

在Internet上的主机有两种方式表示地址，如：

- 域名

  - 如：www.baidu.com

- IP地址

  - 如：185.199.111.153

获得一个InetAddress对象，使用InetAddress类的静态方法getByName(String s)将一个域名或IP地址传给s，该对象含有主机地址的域名和IP地址。

例子：

```
import java.net.*;
import java.util.Scanner;
public class InetAddress1 {
	public static void main(String args[]) {
		try {
			InetAddress i=InetAddress.getByName("lcfu1.github.io");
			System.out.println("域名和IP地址："+i.toString());
			System.out.println("域名："+i.getHostName());
			System.out.println("IP地址："+i.getHostAddress());
			i=InetAddress.getLocalHost();
			System.out.println("本地机器的域名和IP地址："+i.toString());
		}catch(UnknownHostException e) {
			System.out.println("查询失败");
		}
	}
}
```

注：

- 使用InetAddress类的静态方法getLocalHost()来获得一个InetAddress对象，该对象包含本地机器的域名和IP地址。

- InetAddress类的实例方法：

  - getHostName()：获得InetAddress对象所含的域名。

  - getHostAddress()：获得InetAddress对象所含的IP地址。