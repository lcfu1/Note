# SVG(1)

目录：

- [SVG定义](#SVG定义)
- [什么是SVG？](#什么是SVG？)
- [优势](#优势)
- [SVG编辑器](#SVG编辑器)
- [简单的画圆例子代码解析](#简单的画圆例子代码解析)
- [SVG的使用](#SVG的使用)

## SVG定义

可缩放矢量图形是基于可扩展标记语言(标准通用标记语言的子集)，用于描述二维矢量图形的一种图形格式。它由万维网联盟制定，是一个开放标准。

## 什么是SVG？

- SVG 指可伸缩矢量图形 (Scalable Vector Graphics)

- SVG 用来定义用于网络的基于矢量的图形

- SVG 使用 XML 格式定义图形

- SVG 图像在放大或改变尺寸的情况下其图形质量不会有所损失

- SVG 是万维网联盟的标准

- SVG 与诸如 DOM 和 XSL 之类的 W3C 标准是一个整体

## 优势

- SVG 可被非常多的工具读取和修改(比如记事本)

- SVG 与 JPEG 和 GIF 图像比起来，尺寸更小，且可压缩性更强。

- SVG 是可伸缩的

- SVG 图像可在任何的分辨率下被高质量地打印

- SVG 可在图像质量不下降的情况下被放大

- SVG 图像中的文本是可选的，同时也是可搜索的(很适合制作地图)

- SVG 可以与 Java 技术一起运行

- SVG 是开放的标准

- SVG 文件是纯粹的 XML

#### 矢量图像格式和位图图像格式的区别：

矢量图像用点和线来描述物体，所以文件会比较小，同时也能提供高清晰的画面，适合于直接打印或输出。而位图图像的存储单位是图像上每一点的像素值，因此一般的图像文件都很大，会占用大量的网络带宽。SVG是一种矢量图形格式，GIF、JPEG是光栅文件格式。

**1.任意放缩：**

用户可以任意缩放图像显示，而不会破坏图像的清晰度、细节等。

**2.文本独立：**

SVG图像中的文字独立于图像，文字保留可编辑和可搜寻的状态。也不会再有字体的限制，用户系统即使没有安装某一字体，也会看到和他们制作时完全相同的画面。

**3.较小文件：**

总体来讲，SVG文件比那些GIF和JPEG格式的文件要小很多，因而下载也很快。

**4.超强显示效果：**

SVG图像在屏幕上总是边缘清晰，它的清晰度适合任何屏幕分辨率和打印分辨率。

**5.超级颜色控制：**

SVG图像提供一个1 600万种颜色的调色板，支持ICC颜色描述文件标准、RGB、线X填充、渐变和蒙版。

**6.交互X和智能化**

SVG面临的主要问题一个是如何和已经占有重要市场份额的矢量图形格式Flash竞争的问题，另一个问题就是SVG的本地运行环境下的厂家支持程度。

## SVG编辑器

- 线上编辑器（下载地址：https://c.runoob.com/more/svgeditor）

- 本地编辑器inkscape（下载地址：http://baoku.360.cn/soft/show/appid/102866）

## 简单的画圆例子代码解析

```
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" 
"http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="black"
  stroke-width="2" fill="red" />
</svg>
```

**SVG 代码解析：**

- standalone属性：该属性规定此 SVG 文件是否是"独立的"，或含有对外部文件的引用。standalone="no"意味着 SVG 文档会引用一个外部文件 - 在这里，是 DTD 文件。

- http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd：外部的 SVG DTD，该 DTD 位于 W3C，含有所有允许的 SVG 元素。

- `<svg>`元素：包括开启标签`<svg>`和关闭标签`</svg>`，这是根元素。

- width和height属性：设置此SVG文档的宽度和高度，这里设置为100px。

- xmlns 属性：定义 SVG 命名空间。

- version 属性：定义所使用的 SVG 版本。

- `<circle> `：创建一个圆。

- cx和cy属性：定义圆中心的 x 和 y 坐标，如果忽略，那么圆点会被设置为 (0, 0)。

- r 属性：定义圆的半径。

- stroke和 stroke-width属性：控制如何显示形状的轮廓，这里把圆的轮廓设置为 2px 宽，黑边框。

- fill属性：设置形状内的颜色，这里设置为红色。

## SVG的使用

#### SVG在HTML页面的使用：

- SVG 文件可通过以下标签嵌入 HTML 文档：`<embed>`、`<object>`或者`<iframe>`。

- SVG的代码可以直接嵌入到HTML页面中，或链接到SVG文件。

- 谷歌Chrome，火狐，Internet Explorer9，和Safari都支持。

**`<embed>`标签：**

- 优势：所有主要浏览器都支持，并允许使用脚本

- 缺点：不推荐在HTML4和XHTML中使用（但在HTML5允许）

- 使用：`<embed src="circle.svg" type="image/svg+xml" />`

**`<object>`标签：**

- 优势：所有主要浏览器都支持，并支持HTML4，XHTML和HTML5标准

- 缺点：不允许使用脚本。

- 使用：`<object data="circle.svg" type="image/svg+xml"></object>`

**`<iframe>`标签：**

- 优势：所有主要浏览器都支持，并允许使用脚本

- 缺点：不推荐在HTML4和XHTML中使用（但在HTML5允许）

- 使用：`<iframe src="circle.svg"></iframe>`

**直接在HTML嵌入SVG代码：**

- *代码如下：*

```
<!DOCTYPE html>
<html>
<body>
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="black"  stroke-width="2" fill="red" />
</svg>
</body>
</html>
```

**链接到SVG文件：**

- 通过`<a>`标签可以链接到一个SVG文件。

- 使用：`<a href="circle.svg">SVG</a>`
