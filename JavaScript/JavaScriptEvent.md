# JavaScript事件

目录：

- [通过事件属性来调用](#通过事件属性来调用)
- [修改自身内容](#修改自身内容)
- [HTML事件列表](#html事件列表)

## 通过事件属性来调用

如：

```
<button id="test" onclick="date()">点击</button>
<script>
function date(){
	document.getElementById("test").innerHTML=Date();
}
</script>
```

效果：

<button id="test" onclick="date()">点击</button>
<script>
function date(){
	document.getElementById("test").innerHTML=Date();
}
</script>

## 修改自身内容

```
<button onclick="this.innerHTML=Date()">点击</button>
```

效果：

<button onclick="this.innerHTML=Date()">点击</button>

## HTML事件列表

| 事件        | 描述                         |
| ----------- | ---------------------------- |
| onchange    | HTML元素改变                 |
| onclick     | 用户点击 HTML元素            |
| onmouseover | 用户在一个HTML元素上移动鼠标 |
| onmouseout  | 用户从一个HTML元素上移开鼠标 |
| onkeydown   | 用户按下键盘按键             |
| onload      | 浏览器已完成页面的加载       |