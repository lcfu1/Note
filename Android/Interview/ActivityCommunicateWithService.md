# Activity如何与Service通信？

主要有以下5种：

- binder+回调（listener）
  - Acitivity 将实例传入 Service，同时利用回调更新UI。
- binder+Handler
  - Service 持有 Activity的Handler 对象，Service 通过往该Handler send message 的方式，达到通信的目的。
- 广播
  - 利用系统的LocalBroadcastManager（推荐），Service send message，Activity receive message。
- 开源组件
  - 如EventBus、otto等，利用反射或者注释的方式实现对注册类的注册，然后遍历当前的注册表，通过key进行查询，然后dispatch，调用相应的事件处理方法。
- Android接口定义语言



参考：

- [https://www.jianshu.com/p/cd69f208f395](https://www.jianshu.com/p/cd69f208f395)


- [https://baike.so.com/doc/6948929-7171330.html](https://baike.so.com/doc/6948929-7171330.html)