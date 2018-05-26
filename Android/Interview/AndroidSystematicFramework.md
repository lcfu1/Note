# Android系统架构

Android系统架构图：

![Android系统架构图](https://raw.githubusercontent.com/lcfu1/Image/master/Android/Android%E7%B3%BB%E7%BB%9F%E6%9E%B6%E6%9E%84%E5%9B%BE.png)

- 可以分为应用层、应用框架层、系统运行库层、硬件抽象层和Linux内核层。

  - 应用层：系统内置的应用程序和非系统级的应用程序，主要负责与人交互。

  - 应用框架层：为开发者提供开发所需的API等。

  - 系统运行库层：包含C/C++程序库和Android运行时库。

  - 硬件抽象层：分为核心库和ART(5.0系统之后，Dalvik虚拟机被ART取代)。

  - Linux内核层：android的核心系统服务基于Linux内核，系统的安全性、内存管理、进程管理、网络协议栈和驱动模型等都依赖于该内核。

**参考：**

- [https://blog.csdn.net/itachi85/article/details/54695046](https://blog.csdn.net/itachi85/article/details/54695046)