## jQuery 语法

通过选取 HTML 元素，并对选取的元素执行某些操作。

### 基础语法：$(selector).action()

- 用美元符号来定义jQuery。

- 选择符（selector）基于元素的 id、类、类型、属性、属性值等"查询"和"查找" HTML 元素。

- jQuery 的action()执行对元素的操作。

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