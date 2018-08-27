# Bootstrap4网格系统

目录：

- [分类](#分类)
- [相等宽度的列](#相等宽度的列)
- [等宽响应式列](#等宽响应式列)
- [不等宽响应式列](#不等宽响应式列)
- [偏移列](#偏移列)

## 分类

| 设备           | 类前缀   | 屏幕宽度 |
| -------------- | -------- | -------- |
| 针对所有设备   | .col-    | auto     |
| 平板           | .col-sm- | >=576px  |
| 桌面显示器     | .col-md- | >=768px  |
| 大桌面显示器   | .col-lg- | >=992px  |
| 超大桌面显示器 | .col-xl- | >=1200px |

### 相等宽度的列

```
<div class="row">
	<div class="col" style="background-color:lavender;">.col</div>
    <div class="col" style="background-color:orange;">.col</div>
    <div class="col" style="background-color:grey;">.col</div>
</div>
```

### 等宽响应式列

```
<div class="row">
    <div class="col-sm-3" style="background-color:lavender;">.col-sm-3</div>
    <div class="col-sm-3" style="background-color:lavenderblush;">.col-sm-3</div>
    <div class="col-sm-3" style="background-color:lavender;">.col-sm-3</div>
    <div class="col-sm-3" style="background-color:lavenderblush;">.col-sm-3</div>
</div>
```

### 不等宽响应式列

```
<div class="row">
    <div class="col-sm-4" style="background-color:lavender;">.col-sm-4</div>
    <div class="col-sm-8" style="background-color:lavenderblush;">.col-sm-8</div>
</div>
```

## 平板和桌面端

```
<div class="col-sm-3 col-md-6 bg-success">
	col-sm-3 col-md-6 bg-success
</div>
<div class="col-sm-9 col-md-6 bg-warning">
	col-sm-9 col-md-6 bg-warning
</div>
```

在桌面设置上显示两列各占50%，在平板上则左边的宽度为25%，右边的宽度为75%。

## 偏移列

```
<div class="row">
	<div class="col-md-4 bg-success">.col-md-4</div>
	<div class="col-md-4 offset-md-4 bg-warning">.col-md-4 .offset-md-4</div>
</div>
```

