# 速查列表

目录

- [标题](#标题)
- [段落](#段落)
- [链接](#链接)
- [图片](#图片)
- [水平线](#水平线)
- [注释](#注释)
- [折行](#折行)
- [文本格式化](#文本格式化)
- [表格](#表格)
- [有序列表](#有序列表)
- [无序列表](#无序列表)
- [自定义列表](#自定义列表)
- [区块](#区块)
- [样式](#样式)
- [使用`<table>`元素的网页布局](#使用table元素的网页布局)
- [使用`<div>`元素的网页布局](#使用div元素的网页布局)
- [表单](#表单)
- [框架](#框架)
- [字符实体](#字符实体)
- [URL](#url)
- [ASCII字符集](#ascii字符集)
- [URL编码](url编码)

## 标题

```
<h1>标题1</h1>
<h2>标题2</h2>
<h3>标题3</h3>
<h4>标题4</h4>
<h5>标题5</h5>
<h6>标题6</h6>
```

## 段落

```
<p>段落</p>
```

## 链接

```
<a href="https://lcfu1.github.io/Note/" target="_blank">链接</a>

<a id="tips">提示部分</a>
<a href="#tips">跳到提示部分</a>
```

注：

- target属性：
  - _blank：在新窗口打开。
  - _self：在同一个窗口打开。
  - _parent：在父窗口打开。

## 图片

```
<img border="1" src="https://raw.githubusercontent.com/lcfu1/Image/master/favicon.ico" width="32" height="32" alt="favicon.ico" usemap="#faviconmap">
<map name="faviconmap">
	<area shape="circle" coords="16,16,16" alt="favicon" href="https://lcfu1.github.io/Note/">
</map>
```

注：

- border属性：边框。
- src属性：图像的地址。
- alt属性：在浏览器无法载入图像时，为图像定义一串预备的可替换的文本。 
- align属性：默认对齐的图像，可设置为top、middle、bottom。align属性在HTML4中已废弃，HTML5 已不支持该属性。
- usemap属性：图像映射。
- width和height属性：设置图片的宽高。
- `<map>`标签：定义图像地图。
- `<area>`标签：定义图像地图中的可点击区域。
  - shape属性：定义形状，可设置为rect、circle、poly等。
  - coords属性：链接区域在图片中的坐标，如上面coords="16,16,16"是指圆心坐标为(16,16)，半径为16。

## 水平线

```
<hr/>
```

## 注释

```
<!--注释-->
```

## 折行

```
<p>段落<br>段落</p>
```

注：

- `<br/>`是一个空的 HTML 元素，关闭标签没有任何意义，所以它没有结束标签。 

## 文本格式化

```
<b>粗体文本</b>
<code>计算机代码</code>
<em>强调文本</em>
<i>斜体文本</i>
<kbd>键盘输入</kbd> 
<pre>预格式化文本</pre>
<small>更小的文本</small>
<strong>重要的文本</strong>
<q>块引用</q>
<abbr title="我是缩写的">缩写</abbr>
<acronym title="首字母缩写">缩写</acronym>
<address><a href="mailto:lcfu1@qq.com">lcfu1</a>.<br> </address>
<bdo dir="rtl">从右到左显示</bdo>
<blockquote>从另一个源引用的部分</blockquote>
<cite>工作的名称</cite>
<del>删除的文本</del>
<ins>插入的文本</ins>
<sub>下标文本</sub>
<sup>上标文本</sup>
<pre>
    预格式
文本
</pre>
```

## 表格

```
<table border="1" cellpadding="10" cellspacing="0">
<colgroup>
	<col style="background-color:blue">
    <col span="3" style="background-color:red">
</colgroup>
<caption>标题</caption>
<thead>
	<tr>
    	<th>Head</th>
    	<th>Header1</th>
		<th>Header2</th>
		<th>Header3</th>
	</tr>
</thead>
<tbody>
	<tr>
		<th>body</th>
  		<td rowspan="2">2</td>
  		<td colspan="2">2</td>
	</tr>
	<tr>
		<th>body</th>
  		<td>2</td>
  		<td>2</td>
	</tr>
</tbody>
<tfoot>
	<tr>
      	<th>foot</th>
      	<td>3</td>
		<td>3</td>
		<td>3</td>
    </tr>
</tfoot>
</table>
```

注：

- 用`<table>`标签来定义。
- border属性用于设置边框。
- cellpadding属性用于设置单元格边距。
- cellspacing属性用于设置单元格间距。
- `<colgroup>`和`<col>`标签为表格中的列设置背景色。
- `<caption>`标签定义表格的标题。
- `<thead>`标签用于组合表头内容。
- `<tbody>`标签用于组合主体内容。
- `<tfoot>`标签用于组合页脚内容。
- `<tr>`标签定义行。
- `<th>`标签定义表头。
- `<td>`标签定义单元格 。
- rowspan属性用于跨列。
- colspan属性用于跨单元格。

## 有序列表

```
<ol start="2">
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ol>
<ol type="A">
    <li>A</li>
    <li>B</li>
</ol> 
```

注：

- start属性可用于设置列表项目从哪个数字开始。
- type属性可用于设置不同类型的有序列表：
  - A：大写字母列表。
  - a：小写字母列表。
  - I：罗马数字列表。
  - i：小写罗马数字列表。

## 无序列表

```
<ul style="list-style-type:circle">
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
```

注：

- style属性：使用CSS定义不同类型的无序列表：
  - list-style-type:circle：圆点列表。
  - list-style-type:circle：圆圈列表。
  - list-style-type:square：正方形列表。

## 自定义列表

```
<dl>
	<dt>AA</dt>
    <dd>1</dd>
    <dt>BB</dt>
    <dd>2</dd>
</dl>
```

注：

- `<dl>`：定义列表。
- `<dt>`：自定义列表项目。
- `<dd>`：自定义列表项的描述。

## 区块

```
<p>段落</p>
<div style="color:blue">
	<p>段落</p>
	<p>段落</p>
</div>
<p>A <span style="color:red;font-weight:bold">B</span> C</p>
```

注：

- `<div>`块级元素，浏览器会在其前后显示折行。

- `<span>`内联元素，用来组合文档中的行内元素。

## 样式

```
<style type="text/css">
h1 {color:red;}
</style>
<h1>A</h1>
```

## 使用`<table>`元素的网页布局

```
<table width="600" border="0">
<tr>
	<td colspan="2" style="background-color:red;text-align:center;">header</td>
</tr>

<tr>
	<td style="background-color:blue;vertical-align:top;">menu</td>
	<td style="background-color:grey;height:300px;width:500px;vertical-align:top;">body</td>
</tr>

<tr>
	<td colspan="2" style="background-color:red;text-align:center;">footer</td>
</tr>
</table>
```

## 使用`<div>`元素的网页布局

```
<div id="container" style="width:600px">

<div id="header" style="background-color:red;text-align:center;">header</div>

<div id="menu" style="background-color:blue;height:300px;width:100px;float:left;">menu</div>

<div id="content" style="background-color:grey;height:300px;width:500px;float:left;">body</div>

<div id="footer" style="background-color:red;clear:both;text-align:center;">footer</div>

</div>
```

## 表单

用于收集不同类型的用户输入，包含表单元素。

表单元素是允许用户在表单中输入内容，如：文本域(textarea)、下拉列表、单选按钮(radio-buttons)、复选框(checkboxes)等等。 

```
<form name="input" action="" method="get">
username: <input type="text" name="user"><br>
password: <input type="password" name="password"><br>
<input type="submit" value="submit">
</form>
```

## 框架

使用框架可在同一个浏览器窗口中显示多个页面。 

```
<iframe style="background-color:grey;" name="iframe" width="400" height="200" frameborder="0"></iframe></br>
<a href="https://lcfu1.github.io/Note/" target="iframe">点击</a>
```

注：

- frameborder属性表示是否显示边框。

## 字符实体

是html中的预留字符，对大小写敏感 。

```
&nbsp;	指空格
&quot;	引号
...
```

[更多](https://www.runoob.com/html/html-entities.html)

## URL

统一资源定位器(Uniform Resource Locators)

```
scheme://host.domain:port/path/filename
```

注：

- scheme：定义因特网服务的类型。最常见的类型是http(超文本传输协议)、https(安全超文本传输协议)、ftp(文件传输协议)。
- host：定义域主机（http的默认主机是www）。
- domain：定义因特网域名，比如 runoob.com。
- :port：定义主机上的端口号（http 的默认端口号是 80）。
- path：定义服务器上的路径（如果省略，则文档必须位于网站的根目录中）。
- filename：定义文档/资源的名称。

## ASCII字符集

[参考](https://www.runoob.com/tags/html-ascii.html)

## URL编码

[参考](https://www.runoob.com/tags/html-urlencode.html)