# 输出

JavaScript 显示数据：

- 使用 **window.alert()** 弹出警告框。
- 使用 **document.write()** 方法将内容写到 HTML 文档中。
- 使用 **innerHTML** 写入到 HTML 元素。
- 使用 **console.log()** 写入到浏览器的控制台。

### window.alert()

弹出警告框。

使用如：

```
<script>
	window.alert("hello!");
</script>
```

### document.write()

输出内容到HTML中，如果HTML加载后执行 document.write，整个HTML页面将被覆盖掉。 

使用如：

```
<script>
	document.write("hello!");
</script>
```

### innerHTML

document.getElementById(*id*) ，使用 "id" 属性来标识 HTML元素，并innerHTML来获取或插入元素内容 。

使用如：

```
<p id="demo">段落</p>
<script>
	document.getElementById("demo").innerHTML="hello";
</script>
```

### console.log()

使用F12来启用调试模式，在调试窗口中点击 "Console" 菜单。

使用如：

```
<script>
	console.log("hello");
</script>
```

