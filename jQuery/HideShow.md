## 隐藏和显示

### hide()

- 使用 hide() 方法来隐藏HTML 元素。

### show()

- 使用 show() 方法来显示HTML 元素。

### toggle()

使用 toggle() 方法来切换 hide() 和 show() 方法。

### 语法

```
$(selector).hide(speed,callback);
$(selector).show(speed,callback);
$(selector).toggle(speed,callback);
```

### 例子

```
<script>
$(document).ready(function(){
	$("#hide").click(function(){
    	$("p").hide();
  	});
  	$("#show").click(function(){
    	$("p").show();
  	});
 	$("#hs").click(function(){
    	$("p").toggle();
 	});
});
</script>
<button id="hide">隐藏</button>
<button id="show">显示</button>
<button id="hs">隐藏/显示</button>
<p>段落</p>
```

