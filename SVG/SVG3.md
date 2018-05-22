# SVG(3)

目录：

- [stroke属性](#stroke属性)
- [过虑器](#过虑器)
- [渐变](#渐变)

## stroke属性

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <g fill="none">
    <path stroke="red" stroke-linecap="butt" stroke-width="4" d="M10 10 L100 10" />
    <path stroke="black" stroke-linecap="round" stroke-width="5" d="M10 20 L100 20" />
    <path stroke="blue" stroke-linecap="square" stroke-width="6" stroke-dasharray="20,10,5,5,5,10" d="M10 30 L200 30" />
  </g>
</svg>
```

**解析：**

- stroke：定义一条线，文本或元素轮廓颜色。
- stroke-width：定义厚度。
- stroke-linecap：定义不同类型的开放路径的终结。
  - butt：默认。向线条的每个末端添加平直的边缘。 
  - round：向线条的每个末端添加圆形线帽。 
  - square： 向线条的每个末端添加正方形线帽。 
- stroke-dasharray：创建虚线。

## 过虑器

**SVG支持的过虑器表：**

| 过虑器              | 作用                                   |
| ------------------- | -------------------------------------- |
| feBlend             | 使用不同的混合模式把两个对象合成在一起 |
| feColorMatrix       | 用于彩色滤光片转换                     |
| feComponentTransfer | 执行数据的 component-wise 重映射       |
| feComposite         |                                        |
| feConvolveMatrix    |                                        |
| feDiffuseLighting   |                                        |
| feDisplacementMap   |                                        |
| feFlood             |                                        |
| feGaussianBlur      | 执行高斯模糊图像                       |
| feImage             |                                        |
| feMerge             | 建立在彼此顶部图像层                   |
| feMorphology        |                                        |
| feOffset            | 过滤阴影，  创建阴影效果               |
| feSpecularLighting  |                                        |
| feTile              |                                        |
| feTurbulence        |                                        |
| feDistantLight      | 定义一个光源，用于照明过滤             |
| fePointLight        | 用于照明过滤                           |
| feSpotLight         | 用于照明过滤                           |

过滤器在`<defs>`元素中定义，使用`<filter>`标记来定义SVG滤镜，`<filter>`标记使用id属性来定义向图形应用哪个滤镜，在图形中使用`<filter>`属性，通过将 url 值设置为用户分配给过滤器的 id 属性的值。

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <defs>
    <filter id="f1" x="0" y="0">
      <feGaussianBlur in="SourceGraphic" stdDeviation="10" />
    </filter>
  </defs>
  <rect width="100" height="100" stroke="black" stroke-width="2" fill="red" filter="url(#f1)" />
</svg>
```

**解析：**

- `<feGaussianBlur>`元素：定义模糊效果。
- `in="SourceGraphic"`：定义了由整个图像创建效果。
- `stdDeviation`属性：定义模糊量。
- `<rect>`元素的滤镜属性用来把元素链接到"f1"滤镜。

## 渐变

是一种从一种颜色到另一种颜色的平滑过渡，有两种基本形式：线性（Linear）和径向渐变（Radial）。

#### （1）线性渐变`<linearGradient>`

线性渐变可以定义为水平，垂直或角渐变：

- 当y1和y2相等，而x1和x2不同时，可创建水平渐变
- 当x1和x2相等，而y1和y2不同时，可创建垂直渐变
- 当x1和x2不同，且y1和y2不同时，可创建角形渐变

**水平线性渐变例子：**

 ```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <defs>
    <linearGradient id="grad1" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" style="stop-color:rgb(255,255,0);stop-opacity:1" />
      <stop offset="100%" style="stop-color:rgb(255,0,0);stop-opacity:1" />
    </linearGradient>
  </defs>
  <circle cx="100" cy="100" r="50" fill="url(#grad1)" />
</svg>
 ```

**解析：**

- `<linearGradient>`标签的id属性可为渐变定义一个唯一的名称。
- `<linearGradient>`标签的x1，x2，y1，y2属性定义渐变开始和结束位置。
- 渐变的颜色范围可由两种或多种颜色组成。每种颜色通过一个`<stop>`标签来规定。offset属性用来定义渐变的开始和结束位置。
- 填充属性把 circle 元素链接到此渐变。

**垂直线性渐变例子：**

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <defs>
    <linearGradient id="grad1" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%" style="stop-color:rgb(255,255,0);stop-opacity:1" />
      <stop offset="100%" style="stop-color:rgb(255,0,0);stop-opacity:1" />
    </linearGradient>
  </defs>
  <circle cx="100" cy="100" r="50" fill="url(#grad1)" />
</svg>
```

#### （2）放射性渐变`<radialGradient>`

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <defs>
    <radialGradient id="grad1" cx="50%" cy="50%" r="50%" fx="50%" fy="50%">
      <stop offset="0%" style="stop-color:rgb(255,255,0);stop-opacity:0" />
      <stop offset="100%" style="stop-color:rgb(255,0,0);stop-opacity:1" />
    </radialGradient>
  </defs>
  <circle cx="100" cy="100" r="50" fill="url(#grad1)" />
</svg>
```

**解析：**

- `<radialGradient>`标签的 id 属性可为渐变定义一个唯一的名称。
- cx，cy和r属性定义的最外层圆和fx和fy定义的最内层圆。
- `circle`标签中的cx，cy和r属性定义圆的起始坐标和半径。
- 渐变颜色范围可以由两个或两个以上的颜色组成。每种颜色用一个`<stop>`标签指定。offset属性用来定义渐变色开始和结束。
- 填充属性把circle元素链接到此渐变。