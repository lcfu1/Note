##  jQuery选择器

**`$(this).hide()`：隐藏当前元素。**

- 代码：

```
<!--test1-->
  <div class="test">
  	<script>
  		$(function() {
  			$("h4").click(function() {
  				$(this).hide();
  			});
  		});
  	</script>
  	<h4>点击这个段落进行测试。</h4>
  </div>
```

- 测试：

 <!--test1-->
 <div class="test">
  	<script>
  		$(function() {
  			$("h4").click(function() {
  				$(this).hide();
  			});
  		});
  	</script>
  	<h4>点击这个段落进行测试。</h4>
 </div>

**`$("p").hide()`：隐藏所有 `<p>` 元素，通过元素名选取元素。**

- 代码：

```
<!--test2-->
  <div class="test">
  	<script>
  		$(function() {
  			$("h5").click(function() {
  				$("h5").hide();
  			});
  		});
  	</script>
  	<h5>点击这个段落进行测试。</h5>
  </div>
```

- 测试：

 <!--test2-->
 <div class="test">
  	<script>
  		$(function() {
  			$("h5").click(function() {
  				$("h5").hide();
  			});
  		});
  	</script>
  	<h5>点击这个段落进行测试。</h5>
 </div>

**`$("#test").hide()`：隐藏所有 `id="test"`  的元素，通过 HTML 元素的 id 属性选取指定的元素。**

- 代码：

```
<!--test3-->
  <div class="test">
	<script>
		$(function() {
			$("#test3").click(function() {
				$("#test3").hide();
			});
		});
		</script>
	<h6 id="test3">点击这个段落进行测试。</h6>
  </div>
```

- 测试：

 <!--test3-->
 <div class="test">
	<script>
		$(function() {
			$("#test3").click(function() {
				$("#test3").hide();
			});
		});
	</script>
	<h6 id="test3">点击这个段落进行测试。</h6>
 </div>

**`$("p.test").hide()`：隐藏所有 `class="test"` 的 `<p>` 元素，通过指定的 class 查找元素。**

- 代码：

```
  <!--test4-->
<div class="test">
	<script>
		$(function() {
			$(".test4").click(function() {
				$(".test4").hide();
			});
		});
	</script>
	<h7 class="test4">点击这个段落进行测试。</h7>
  </div>
```

- 测试：

 <!--test4-->
 <div class="test">
	<script>
		$(function() {
			$(".test4").click(function() {
				$(".test4").hide();
			});
		});
	</script>
	<h7 class="test4">点击这个段落进行测试。</h7>
 </div>