# 滑动

目录：

- [slideDown()](#slidedown)
- [slideUp()](#slideup)
- [slideToggle()](#slidetoggle)
- [语法](#语法)

## slideDown()

- 向下滑动元素。

```
<script> 
$(document).ready(function(){
  $("#down").click(function(){
    $("#p").slideDown();
  });
});
</script>
<button id="down">点击</button>
<p id="p" style="display:none;">Hello!</p>
```

## slideUp()

- 向上滑动元素 。

```
<script> 
$(document).ready(function(){
  $("#up").click(function(){
    $("#p").slideUp();
  });
});
</script>
<button id="up">点击</button>
<p id="p">Hello!</p>
```

## slideToggle() 

- 可以在 slideDown() 与 slideUp() 方法之间进行切换。

```
<script> 
$(document).ready(function(){
  $("#toggle").click(function(){
    $("#p").slideToggle();
  });
});
</script>
<button id="toggle">点击</button>
<p id="p">Hello!</p>
```

## 语法

```
$(selector).slideDown(speed,callback);
$(selector).slideUp(speed,callback);
$(selector).slideToggle(speed,callback);
```

**注**：

- 可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
- 可选的 callback 参数是滑动完成后所执行的函数名称。