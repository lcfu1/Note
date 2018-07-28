# 背景

目录：

- [background-color](#background-color)
- [background-image](#background-image)
- [background-repeat](#background-repeat)
- [background-attachment](#background-attachment)
- [background-position](#background-position)
- [background](#background)

## background-color

用background-color属性定义背景颜色，如：

```
body
{
	background-color:red;
}
```

CSS中，颜色值用下面方式来定义：

- 十六进制：#ff0000。
- RGB：rgb(255,0,0)。
- 颜色名称：red。

## background-image

用background-image属性来定义背景图像，如：

```
body 
{
	background-image:url('image.png');
}
```

## background-repeat

用background-repeat属性来定义背景图像平铺，如：

```
body 
{
	background-repeat:no-repeat;
}
```

background-repeat的属性值有：

| 属性值    | 说明                                         |
| --------- | -------------------------------------------- |
| repeat    | 背景图像将向垂直和水平方向重复，默认         |
| repeat-x  | 水平重复平铺背景图像                         |
| repeat-y  | 垂直重复平铺背景图像                         |
| no-repeat | background-image不会重复                     |
| inherit   | 指定background-repea属性设置应该从父元素继承 |

## background-attachment

background-attachment属性用来设置背景图像是否固定或者随着页面的其余部分滚动，如：

```
body 
{
	background-attachment:fixed;
}
```

background-attachment的属性值有：

| 属性值  | 说明                                              |
| ------- | ------------------------------------------------- |
| scroll  | 背景图片随页面的其余部分滚动,默认                 |
| fixed   | 背景图像是固定的                                  |
| inherit | 指定background-attachment属性设置应该从父元素继承 |

## background-position

background-position属性用来设置背景图像的起始位置，如：

```
body 
{
	background-position:left top;
}
```

background-position的属性值有：

| 值                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| left top、left center、left bottom、right top、right center、right bottom、center top、center center、center bottom | 如果仅指定一个关键字，其他值将会是"center"                   |
| *x% y%*                                                      | 第一个值是水平位置，第二个值是垂直。左上角是0％0％。右下角是100％100％。如果仅指定了一个值，其他值将是50％。 。默认值为：0％0％ |
| *xpos ypos*                                                  | 第一个值是水平位置，第二个值是垂直。左上角是0。单位可以是像素（0px0px）或任何其他[CSS单位](http://www.runoob.com/cssref/css-units.html)。如果仅指定了一个值，其他值将是50％。你可以混合使用％和positions |
| inherit                                                      | 指定background-position属性设置应该从父元素继承              |

## background

可以将多个属性合并在同一个属性中，如：

```
body
{
	background:#ffffff url('image.png') repeat-x;
}
```

使用简写属性时，属性值的顺序为：

- background-color
- background-image
- background-repeat
- background-attachment
- background-position

注：不按这个顺序也是可以显示的。