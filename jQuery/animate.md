# 动画

目录：

- [animate()](#animate)
- [stop()](#stop)
- [语法](#语法)

## animate()

- 创建自定义动画。 

```
<script> 
$(document).ready(function(){
  $("button").click(function(){
	$("#p").animate({opacity:'0.2',fontSize:'2em'});
  	$("#p").animate({left:'100px',opacity:'0.5',height:'toggle',width:'+=150px'});
  });
});
</script>
<button>点击</button>
<div id="p" style="background:#888888;height:100px;width:100px;position:absolute;">
Hello
</div>
```

**注**：

- 默认情况下，所有 HTML 元素都有一个静态位置，且无法移动。 如需对位置进行操作，要记得首先把元素的 CSS position 属性设置为 relative、fixed 或 absolute。
- 生成动画的过程中可同时使用多个属性，用逗号隔开。
- 可以定义相对值（该值相对于元素的当前值），需要在值的前面加上 += 或 -= 。
- 可以把属性的动画值设置为 "show"、"hide" 或 "toggle" 。
- 编写多个 animate() 调用，jQuery 会创建包含这些方法调用的"内部"队列。然后逐一运行这些 animate 调用。 

## stop()

- 用于停止动画或效果，在它们完成之前。
- 适用于所有 jQuery 效果函数，包括滑动、淡入淡出和自定义动画。 

```
<script> 
$(document).ready(function(){
  $("#start").click(function(){
    $("#p").slideDown(5000);
  });
  $("#stop").click(function(){
    $("#p").stop();
  });
});
</script>
<button id="start">开始</button>
<button id="stop">停止</button>
<div id="p" style="background:#888888;height:100px;width:100px;position:absolute;display:none">
Hello
</div>
```

## 语法

```
$(selector).animate({params},speed,callback);
$(selector).stop(stopAll,goToEnd);
```

**注**：

- 必需的 params 参数定义形成动画的 CSS 属性。
- 可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
- 可选的 callback 参数是动画完成后所执行的函数名称。
- 可选的 stopAll 参数规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行。
- 可选的 goToEnd 参数规定是否立即完成当前动画。默认是 false。