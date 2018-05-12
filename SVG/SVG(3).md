## SVG(3)

### stroke属性

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

- stroke-dasharray：创建虚线。