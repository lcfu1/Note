# 淡入淡出

目录：

- [fadeIn()](#fadeIn())
- [fadeOut()](#fadeOut())
- [fadeToggle()](#fadeToggle())
- [fadeTo()](fadeTo())
- [语法](#语法)

## fadeIn()

- 淡入已隐藏的元素。

```
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("#div1").fadeIn();
    $("#div2").fadeIn("slow");
    $("#div3").fadeIn(3000);
  });
});
</script>
<button>点击</button>
<br><br>
<div id="div1" style="width:80px;height:80px;display:none;background-color:red;"></div><br>
<div id="div2" style="width:80px;height:80px;display:none;background-color:green;"></div><br>
<div id="div3" style="width:80px;height:80px;display:none;background-color:blue;"></div>
```

## fadeOut()

- 淡出可见元素。 

```
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("#div1").fadeOut();
    $("#div2").fadeOut("slow");
    $("#div3").fadeOut(3000);
  });
});
</script>
<button>点击</button>
<br><br>
<div id="div1" style="width:80px;height:80px;background-color:red;"></div><br>
<div id="div2" style="width:80px;height:80px;background-color:green;"></div><br>
<div id="div3" style="width:80px;height:80px;background-color:blue;"></div>
```

## fadeToggle()

- 可以在 fadeIn() 与 fadeOut() 方法之间进行切换。

```
<script>
$(document).ready(function(){
  $("button").click(function(){
    $("#div1").fadeToggle();
    $("#div2").fadeToggle("slow");
    $("#div3").fadeToggle(3000);
  });
});
</script>
<button>点击</button>
<br><br>
<div id="div1" style="width:80px;height:80px;background-color:red;"></div><br>
<div id="div2" style="width:80px;height:80px;background-color:green;"></div><br>
<div id="div3" style="width:80px;height:80px;background-color:blue;"></div>
```

##  fadeTo()

- 允许渐变为给定的不透明度（值介于 0 与 1 之间）。

```
<script>
$(document).ready(function(){
  $("button").click(function(){
	$("#div1").fadeTo("slow",0.15);
    $("#div2").fadeTo("slow",0.4);
    $("#div3").fadeTo("slow",0.7);
  });
});
</script>
<button>点击</button>
<br><br>
<div id="div1" style="width:80px;height:80px;background-color:red;"></div><br>
<div id="div2" style="width:80px;height:80px;background-color:green;"></div><br>
<div id="div3" style="width:80px;height:80px;background-color:blue;"></div>
```

## 语法

```
$(selector).fadeIn(speed,callback);
$(selector).fadeOut(speed,callback);
$(selector).fadeToggle(speed,callback);
$(selector).fadeTo(speed,opacity,callback);
```

**注**：

- 可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
- 可选的 callback 参数是 fading 完成后所执行的函数名称。 
- opacity 参数将淡入淡出效果设置为给定的不透明度（值介于 0 与 1 之间）。 