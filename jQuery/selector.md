##  jQuery选择器

> 要点：
>
> 1、$(this).hide()
>
> 2、$("p").hide()
>
> 3、$("#test").hide()
>
> 4、$("p.test").hide()

### $(this).hide()

- 隐藏当前元素。

- 代码：

```
<script>
  	$(function() {
  		$("p").click(function() {
  			$(this).hide();
  		});
  	});
</script>
<p>点击这个段落进行测试。</p>
```

### $("p").hide()

- 隐藏所有 `<p>` 元素，通过元素名选取元素。

- 代码：

```
<script>
  	$(function() {
  		$("p").click(function() {
  			$("p").hide();
  		});
  	});
</script>
<p>点击这个段落进行测试。</p>
```

### $("#test").hide()

- 隐藏所有 `id="test"`  的元素，通过 HTML 元素的 id 属性选取指定的元素。

- 代码：

```
<script>
	$(function() {
		$("#test3").click(function() {
			$("#test3").hide();
		});
	});
</script>
<p id="test3">点击这个段落进行测试。</p>
```

### $("p.test").hide()

- 隐藏所有 `class="test"` 的 `<p>` 元素，通过指定的 class 查找元素。

- 代码：

```
<script>
	$(function() {
		$(".test4").click(function() {
			$(".test4").hide();
		});
	});
</script>
<p class="test4">点击这个段落进行测试。</p>
```