## SVG（2）

SVG的一些预定义形状元素 ，如：

- 矩形 <rect>

- 圆形 <circle>

- 椭圆 <ellipse>

- 线 <line>

- 折线 <polyline>

- 多边形 <polygon>

- 路径 <path>

### 矩形 <rect>

用 <rect> 标签来创建矩形 。

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <rect x="50" y="50" rx="20" ry="20" width="100" height="100" style="fill:blue;stroke:pink;stroke-width:2;fill-opacity:0.1;stroke-opacity:0.9" />
</svg>
```

**解析：** 

- x、y属性分别定义矩形的左侧和顶端位置。
- rx、ry属性可使矩形产生圆角。 
- width、height 属性分别定义矩形的高度和宽度。
- style 属性用来定义 CSS 属性。
- CSS 的 fill 属性定义矩形的填充颜色（rgb 值、颜色名或者十六进制值）。
- CSS 的 stroke-width 属性定义矩形边框的宽度。
- CSS 的 stroke 属性定义矩形边框的颜色。
- CSS 的 fill-opacity 属性定义填充颜色透明度（合法的范围是：0 - 1）。
- CSS 的 stroke-opacity 属性定义笔触颜色的透明度（合法的范围是：0 - 1）。

### 圆形 <circle>

用<circle> 标签来创建一个圆。

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
   <circle cx="100" cy="100" r="40" stroke="black" stroke-width="2" fill="red" />
</svg>
```

**解析：**

- cx和cy属性分别定义圆点的x和y坐标。 
- r属性定义圆的半径。
- 其它属性跟上面矩形style属性中的CSS属性一样。

### 椭圆 <ellipse>

用<ellipse> 标签来创建一个椭圆 。

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <ellipse cx="100" cy="100" rx="50" ry="20" style="fill:yellow;stroke:purple;stroke-width:2" />
</svg>
```

**解析：**

- cx、cy属性分别定义椭圆中心的x坐标、y坐标。
- rx、ry属性分别定义水平半径、垂直半径。

### 线 <line>

用<line> 标签来创建一条直线。

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <line x1="0" y1="0" x2="200" y2="0" style="stroke:rgb(255,0,0);stroke-width:2" />
</svg>
```

**解析：**

- x1、y1属性分别在 x、y轴定义线条的开始。
- x2、y2属性分别在 x、y轴定义线条的结束。

### 折线 <polyline>

用<polyline> 标签来创建任何只有直线的形状。

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <polyline points="0,0 100,0 100,100" style="fill:none;stroke:black;stroke-width:2" />
</svg>
```

**解析：**

- points属性定义多边形每个角的 x 和 y 坐标。

### 多边形 <polygon>

用<polygon> 标签来创建含有不少于三个边的图形。 

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <polygon points="100,10 40,180 190,60 10,60 160,180" style="fill:red;stroke:black;stroke-width:2;fill-rule:evenodd;" />
</svg>
```

**解析：**

- points属性定义多边形每个角的 x 和 y 坐标。
- fill-rule属性：
  - nonzero：按该规则，要判断一个点是否在图形内，从该点作任意方向的一条射线，然后检测射线与图形路径的交点情况。从0开始计数，路径从左向右穿过射线则计数加1，从右向左穿过射线则计数减1。得出计数结果后，如果结果是0，则认为点在图形外部，否则认为在内部。 
  - evenodd：按该规则，要判断一个点是否在图形内，从该点作任意方向的一条射线，然后检测射线与图形路径的交点的数量。如果结果是奇数则认为点在内部，是偶数则认为点在外部。 

### 路径 <path>

用<path> 标签来定义一个路径。 

可用以下命令控制路径数据，允许大小写字母，大写表示绝对定位，小写表示相对定位。 

- M = moveto
- L = lineto
- H = horizontal lineto
- V = vertical lineto
- C = curveto
- S = smooth curveto
- Q = quadratic Bézier curve
- T = smooth quadratic Bézier curveto
- A = elliptical Arc
- Z = closepath

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <path d="M0 0 L100 0 L100 100 Z" />
</svg>
```

**解析：**

- d属性用来定义路径。
- M0 0：从位置(0,0)开始。
- L100 0：直线到位置(100,0)。
- L100 100：直线到位置(100,100)。
- Z：关闭路径。