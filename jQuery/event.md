## jQuery 事件

### jQuery中的事件有：

- 点击元素。

- 在元素上移动鼠标。

- 选取单选按钮。

### jQuery 事件方法

- **click()**

  - 当按钮或元素点击事件被触发时会调用一个函数 。
  - 如：

  ```
  <script>
  $(document).ready(function(){
    $("#p").click(function(){
      $(this).hide();
    });
  });
  </script>
  <p id="p">单击</p>
  ```

- **dblclick()**

  - 当按钮或元素被双击时会调用一个函数 。
  - 如：

  ```
  <script>
  $(document).ready(function(){
    $("#p").dblclick(function(){
      $(this).hide();
    });
  });
  </script>
  <p id="p">双击</p>
  ```

- **mouseenter()**

  - 当鼠标移到元素上，会发生 mouseenter 事件。 
  - 如：

  ```
  <script>
  $(document).ready(function(){
    $("#p").mouseenter(function(){
      alert("鼠标在此段落上");
    });
  });
  </script>
  <p id="p">鼠标移到此段落</p>
  ```

- **mouseleave()**

  - 当鼠标离开此元素时，会发生 mouseleave 事件。 
  - 如：

  ```
  <script>
  $(document).ready(function(){
    $("#p").mouseleave(function(){
      alert("鼠标离开此段落");
    });
  });
  </script>
  <p id="p">鼠标移到此段落</p>
  ```

- **mousedown()**

  - 鼠标移到元素上方，并点击时，会发生 mousedown 事件。 
  - 如：

  ```
  <script>
  $(document).ready(function(){
    $("#p").mousedown(function(){
      alert("鼠标点击了此段落");
    });
  });
  </script>
  <p id="p">鼠标移到此段落</p>
  ```

- **mouseup()**

  - 鼠标在元素上点击并松开时，会发生 mouseup 事件。 
  - 如：

  ```
  <script>
  $(document).ready(function(){
    $("#p").mouseup(function(){
      alert("鼠标点击了此段落并放开了");
    });
  });
  </script>
  <p id="p">鼠标移到此段落</p>
  ```

- **hover()**

  - 鼠标移动到元素上时，会触发指定的第一个函数，当鼠标移开时，会触发指定的第二个函数。
  - 如： 

  ```
  <script>
  $(document).ready(function(){
      $("#p").hover(
  		function(){
  			alert("鼠标移到此段落上");
  		},
  		function(){
  			alert("鼠标离开了此段落");
  		}
      )
  });
  </script>
  <p id="p">鼠标移到此段落</p>
  ```

- **focus()**

  - 当通过鼠标点击选中元素或通过 tab 键定位到元素时，该元素就会获得焦点。 
  - 如：

  ```
  <script>
  $(document).ready(function(){
    $("#i").focus(function(){
      $(this).css("background-color","#f2f2f2");
    });
  });
  </script>
  <input id="i" type="text">
  ```

- **blur()**

  - 当元素失去焦点时，发生 blur 事件。 
  - 如：

  ```
  <script>
  $(document).ready(function(){
    $("#i").blur(function(){
      $(this).css("background-color","#f2f2f2");
    });
  });
  </script>
  <input id="i" type="text">
  ```

- **keydown()**

  - 当键盘键被按下时触发 keydown 事件，可通过event.which属性返回指定事件上哪个键盘键或鼠标按钮被按下。 
  - 如：

  ```
  <script>
  $(document).ready(function(){
    $(window).keydown(function(){
  	$("#p").html("Key: " + event.which);
    });
  });
  </script>
  <p id="p">0</p>
  ```

  