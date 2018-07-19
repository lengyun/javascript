## 内存和性能

事件处理程序可以为现代Web应用程序提供交互能力。

在JavaScript中添加到页面上的事件处理程序数量将直接关系到页面的整体运行性能。导致这一问题的原因是多方面的：

首先，每个函数都是对象，都占用内存；内存中的对象越多，性能就越差。

其次，必须事先指定所有事件处理程序而导致的DOM访问次数，会延迟整个页面的交互就绪时间。

* 事件委托

  对“事件处理程序过多”问题的解决方案就是*事件委托*。事件委托利用了事件冒泡，只指定一个事件处理程序，就可以管理某一来型的所有事件。

  ```js
  var list = document.getElementById('myLinks')
  EventUtil.addHandler(list,'click',function(event){
      event = EventUtil.getEvent(event);
      switch(target.id){
          case 'doSomethins':
              //do somethins
              break;
          case "goSomeWhere":
              location.href = 'http://www.baidu.com'
              break;
          case 'sayHi':
              alert('hi')
              break;
      }
  })
  ```

* 移除事件处理程序

  每当将事件处理程序指定给元素时，运行中的浏览器代码与支持的页面交互的JavaScript代码之间就会建立一个链接。这种链接越多，页面执行起来就越慢。

  在不需要的时候移除事件处理程序，也是解决这个问题的一种方案。内存中留有哪些过时不用的“空事件处理程序”也是造成Web应用程序内存与性能问题的主要原因。

  * 从文档中移除带有事件处理程序的元素时。

    如通过纯粹的DOM操作 removeChild()和replaceChild()方法，更多的是发生在使用innerHTML替换页面某一部分的时候。

  > 如果你知道某个元素即将被移除，那么最好手工移除事件处理程序。

  ```html
  <div id="myDiv">
  <input type="button" value="Click Me" id="myBtn">
  </div>
  <script type="text/javascript">
  var btn = document.getElementById("myBtn");
      btn.onclick = function(){
  	//先执行某些操作
  	btn.onclick = null; //移除事件处理程序
  	document.getElementById("myDiv").innerHTML = "Processing...";
  };
  </script>
  // 防止按钮重复点击
  ```

  * 卸载页面的时候

    （IE8）一般来说，最好的做法是在页面卸载之前，先通过 onunload 事件处理程序移除所有事件处理程序。