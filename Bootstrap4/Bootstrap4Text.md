##  Bootstrap4文字排版

目录：

- [display标题类](#display标题类)
- [small标签](#small标签)
- [mark标签](#mark标签)
- [abbr标签](#abbr标签)
- [blockquote标签](#blockquote标签)
- [dl标签](#dl标签)
- [code标签](#code标签)
- [kbd标签](#kbd标签)
- [pre标签](#pre标签)
- [更多排版类](#更多排版类)

## display标题类

```
<h1 class="display-1">Display 1</h1>
<h1 class="display-2">Display 2</h1>
<h1 class="display-3">Display 3</h1>
<h1 class="display-4">Display 4</h1>
```

## small标签

```
<h1>h1 标题 <small>副标题</small></h1>
<h2>h2 标题 <small>副标题</small></h2>
<h3>h3 标题 <small>副标题</small></h3>
<h4>h4 标题 <small>副标题</small></h4>
<h5>h5 标题 <small>副标题</small></h5>
<h6>h6 标题 <small>副标题</small></h6>
```

## mark标签

```
<p>mark<mark>高亮</mark></p>
```

## abbr标签

```
<p>The <abbr title="World Health Organization">WHO</abbr></p>
```

## blockquote标签

```
<blockquote class="blockquote">
    <p>For 50 years, WWF has been protecting the future of nature. The world's leading conservation organization, WWF works in 100 countries and is supported by 1.2 million members in the United States and close to 5 million globally.</p>
    <footer class="blockquote-footer">From WWF's website</footer>
</blockquote>
```

## dl标签

```
<dl>
	<dt>AA</dt>
	<dd>- aa</dd>
	<dt>BB</dt>
	<dd>- aa</dd>
</dl>
```

## code标签

```
<p>段落<code>span</code></p>
```

## kbd标签

```
<p>段落<kbd>ctrl + p</kbd></p>
```

## pre标签

```
<pre>
A   B
CD   E
</pre>
```

## 更多排版类

| 类名                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| .font-weight-bold   | 加粗文本                                                     |
| .font-weight-normal | 普通文本                                                     |
| .font-weight-light  | 更细的文本                                                   |
| .font-italic        | 斜体文本                                                     |
| .lead               | 让段落更突出                                                 |
| .small              | 指定更小文本 (为父元素的 85% )                               |
| .text-left          | 左对齐                                                       |
| .text-center        | 居中                                                         |
| .text-right         | 右对齐                                                       |
| .text-justify       | 设定文本对齐，段落中超出屏幕部分文字自动换行                 |
| .text-nowrap        | 段落中超出屏幕部分不换行                                     |
| .text-lowercase     | 设定文本小写                                                 |
| .text-uppercase     | 设定文本大写                                                 |
| .text-capitalize    | 设定单词首字母大写                                           |
| .initialism         | 显示在`<abbr>`元素中的文本以小号字体展示，且可以将小写字母转换为大写字母 |
| .list-unstyled      | 移除默认的列表样式，列表项中左对齐 ( `<ul>` 和 `<ol>`中)，这个类仅适用于直接子列表项 (如果需要移除嵌套的列表项，你需要在嵌套的列表中使用该样式) |
| .list-inline        | 将所有列表项放置同一行                                       |
| .pre-scrollable     | 使`<pre>`元素可滚动，代码块区域最大高度为340px，一旦超出这个高度，就会在Y轴出现滚动条 |

例：

```
<p class="font-weight-bold">Bold text.</p>
<p class="font-weight-normal">Normal weight text</p>
<p class="font-weight-light">Light weight text</p>
<p class="font-italic">Italic text</p>
<p class="lead">lead</p>
<p class="small">small</p>
<p class="text-left">text-left</p>
<p class="text-center">text-center</p>
<p class="text-right">text-right</p>
<p class="text-justify">text-justify</p>
<p class="text-nowrap">text-nowrap</p>
<p class="text-lowercase">text-lowercase</p>
<p class="text-uppercase">text-uppercase</p>
<p class="text-capitalize">text-capitalize</p>
<p class="initialism">initialism</p>
<p class="list-unstyled">list-unstyled</p>
<p class="list-inline">list-inline</p>
<p class="pre-scrollable">pre-scrollable</p>
```

