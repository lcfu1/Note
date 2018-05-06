# 语法

### jQuery 语法

- 通过选取 HTML 元素，并对选取的元素执行某些操作。

### 基础语法：$(selector).action()

- 美元符号定义 jQuery。

- 选择符（selector）基于元素的 id、类、类型、属性、属性值等"查询"和"查找" HTML 元素。

- jQuery 的 action() 执行对元素的操作。

### 例子

- `$(this).hide()`：隐藏当前元素。

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

- `$("p").hide()`：隐藏所有 `<p>` 元素，通过元素名选取元素。

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


- `$("#test").hide()`：隐藏所有 `id="test"`  的元素，通过 HTML 元素的 id 属性选取指定的元素。

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

- `$("p.test").hide()`：隐藏所有 `class="test"` 的 `<p>` 元素，通过指定的 class 查找元素。

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

### jQuery 函数

- 位于document ready 函数中。

- 为了防止文档在完全加载（就绪）之前运行 jQuery 代码，即在 DOM 加载完成后才可以对 DOM 进行操作。

- 没有完全加载之前就运行函数，操作可能失败，例子：

  - 试图隐藏一个不存在的元素。
  
  - 获得未完全加载的图像的大小。

- document ready 函数如下：

  ```
  $(document).ready(function(){
     //jQuery 代码
  });
  ```

- 简洁写法，但效果一样，如下：

  ```
  $(function(){
     //jQuery 代码
  });
  ```