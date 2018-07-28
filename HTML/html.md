# HTML

目录：

- [例子](#例子)
- [style](#style)
- [meta](#meta)
- [link](#link)
- [script](#script)
- [noscript](#noscript)
- [base](#base)

## 例子

下面是一个简单的html文件：

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>文档标题</title>
</head>
<body>
	<p>文档内容</p>
</body>
</html>
```

注：

- `<head>`：头部标签，可以添加在头部区域的标签有：`<title>`、`<style>` `<meta>`，`<link>`，`<script>`，`<noscript>`，`<base>`。

- `<meta>`：描述基本的元数据，用于指定网页的描述、关键词、文件的最后修改时间、作者和其他元数据。

- `<title> `：文档标题。

- `<body> `：文档内容。


## style

定义HTML文档的样式文件引用地址，也可以直接添加样式。

```
<style type="text/css">
	body{
		background-color:red
	}
</style>
```

## meta

描述基本的元数据，用于指定网页的描述、关键词、文件的最后修改时间、作者和其他元数据。

```
<!--每5秒钟刷新当前页面-->
<meta http-equiv="refresh" content="5">
<!--定义作者-->
<meta name="author" content="Note">
<!--定义描述内容-->
<meta name="description" content="html">
<!--为搜索引擎定义关键词-->
<meta name="keywords" content="HTML">
```

## link

定义文档与外部资源之间的关系，通常用于链接到样式表。

```
<link rel="stylesheet" type="text/css" href="mystyle.css">
```

## script

用于加载脚本文件，如：JavaScript。 

```
<script src="../libs/jquery/jquery-3.3.1.min.js"></script>
```

## noscript

在不支持JavaScript 的浏览器中显示替代的内容。 

```
<noscript>
	<p>本页面需要浏览器支持JavaScript</p>
</noscript>
```

## base

基本的链接地址或链接目标，作为HTML文档中所有的链接标签的默认链接。

```
<base href="https://lcfu1.github.io/Note/" target="_blank">
```

