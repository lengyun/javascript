### 事件处理程序

事件就是用户或浏览器自身执行的某种动作。而响应某个事件的函数就叫做**事件处理程序/事件侦听器**

- HTML事件处理程序

  某个元素支持的每种事件，都可以使用一个与响应事件处理程序同名的HTML特性来指定。这个特性的值应该是能够执行的Javascript代码。

  ```html
  <input type="button" value="Click Me" onclick="alert('Clicked')" />
  ```

  > 在HTML中定义的事件处理程序可以包含要执行的具体动作，也可以调用在页面其他地方定义的脚本

  ```html
  <script type="text/javascript">
  function showMessage(){
  	alert("Hello world!");
  }
  </script>
  <input type="button" value="Click Me" onclick="showMessage()" />
  ```

  事件处理程序中的代码在执行时，有权访问全局作用域中的任何代码。

  这样指定事件处理程序具有一些独到之处。**首先**，这样会创建一个封装着元素属性值的函数。这个函数中有一个局部变量event，也就是事件对象。

  通过event变量，可以直接访问事件对象，你不用自己定义它，也不用从函数的参数列表中读取。在这个函数内部，this值等于事件的目标元素。

  ```html
  <!-- 输出 "Click Me" -->
  <input type="button" value="Click Me" onclick="alert(this.value)">
  ```

  **其次**，这个动态创建的函数内部，可以像访问全局变量一样访问document及该元素本身的成员。

  ```javascript
  //这个函数使用 with 像下面这样扩展作用域：
  function(){
      with(document){
          with(this){
          	//元素属性值
          }
      }
  }
  ```

  ```html
  <!-- 输出 "Click Me" -->
  <input type="button" value="Click Me" onclick="alert(value)">
  ```

  > 如果当前元素是一个表单输入元素，则作用域中还会包含访问表单元素（父元素）的入口，这个函数就变成了如下所示

  ```js
  function(){
      with(document){
          with(this.form){
              with(this){
              	//元素属性值
              }
          }
      }
  }
  ```

  > 实际上，这样扩展作用域的方式，无非就是想让事件处理程序无需引用表单元素就能访问其他表单字段。例如:

  ```html
  <form method="post">
  	<input type="text" name="username" value="">
  	<input type="button" value="Echo Username" onclick="alert(username.value)">
  </form>
  <!--这里直接引用了 username 元素-->
  ```

  HTML中指定事件处理程序两个**缺点**

  - 存在一个时差问题。用户可能会在HTML元素已出现在页面上就触发相应的事件，单当时的事件处理程序有可能尚不具备执行条件。为此很多HTML事件处理程序都会被封装在一个`try-catch`块中，以便错误不会浮出水面。
  - 扩展事件处理程序的作用域链在不同浏览器中会导致不同结果。
  - HTML与JavaScript代码紧密耦合。

- DOM0级事件处理程序

  通过javaScript指定事件处理程序的传统方式，就是将一个函数赋值给一个事件处理程序属性。

  每个元素都有自己的事件处理程序属性，这些属性通常全部小写，如：onclick。将这种属性的值设置为一个函数，就可以指定事件处理程序

  ```js
  var btn = document.getElementById('myBtn');
  btn.onclick = function(){
      alert(this.id); // 结果弹出myBtn
  }
  ```

  > 一是简单，二是具有跨浏览器的优势

  使用DOM0级方法指定的事件处理程序被认为是元素的方法，因此，这时候的事件处理程序时在元素的作用域中运行，换句话说，程序中的this引用当前元素。

  这种方式添加的事件处理程序会在事件流的冒泡阶段被处理。

  删除通过DOM0级方法指定的事件处理程序，只要将事件处理程序属性的值设置为null即可: `btn.onclick=null`


- DOM2级事件处理程序

  DOM2级事件定义了两个方法，用于处理和删除指定事件处理程序的操作：**addEventListener()** 和**removeEventListener()**

  3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。布尔值为true表示在捕获节点调用事件处理程序，为false，表示在冒泡阶段调用事件处理程序

  **好处**可以添加多个事件处理程序，事件处理程序会按照添加他们的顺序触发

  通过**addEventListener()** 添加的事件处理程序只能使用**removeEventListener()** 来移除；移除时传入的参数与添加处理程序时使用的参数相同。这也意味着通过addEventListener()添加的匿名函数将无法移除

  ```js
  var btn = document.getElementById("myBtn");
  var handler = function(){
  	alert(this.id);
  };
  btn.addEventListener("click", handler, false);
  //这里省略了其他代码
  btn.removeEventListener("click", handler, false); //有效！
  ```

- IE事件处理程序

  IE 实现了与 DOM 中类似的两个方法： **attachEvent()**和 **detachEvent() **。这两个方法接受相同的两个参数：事件处理程序名称与事件处理程序函数。由于 IE8 及更早版本只支持事件冒泡，所以通过attachEvent()添加的事件处理程序都会被添加到**冒泡**阶段。

  > 在 IE 中使用 attachEvent()与使用 DOM0 级方法的主要区别在于事件处理程序的作用域。在使用 DOM0 级方法的情况下，事件处理程序会在其所属元素的作用域内运行；在使用 attachEvent()方法的情况下，事件处理程序会在全局作用域中运行，因此 this 等于 window。

   attachEvent()方法也可以用来为一个元素添加多个事件处理程序,这些事件处理程序不是以添加它们的顺序执行，而是以**相反**的顺序被触发。

  使用 attachEvent()添加的事件可以通过 detachEvent()来移除，条件是必须提供相同的参数。**匿名函数将不能被移除**

- 跨浏览器的事件处理程序

  ```js
  var EventUtil = {
      addHandler: function(element, type, handler){
          if (element.addEventListener){
          	element.addEventListener(type, handler, false);
          } else if (element.attachEvent){
          	element.attachEvent("on" + type, handler);
          } else {
          	element["on" + type] = handler;
          }
      },
      removeHandler: function(element, type, handler){
          if (element.removeEventListener){
          	element.removeEventListener(type, handler, false);
          } else if (element.detachEvent){
          	element.detachEvent("on" + type, handler);
          } else {
          	element["on" + type] = null;
          }
      }
  };
  ```

### 