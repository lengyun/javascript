### 事件对象

在触发DOM上的某个事件时，会产生一个事件对象event，这个对象中包含着所有与事件有关的信息，包括导致事件的元素、事件的类型以及其他与特定事件相关的信息。

- DOM中的事件对象

  兼容DOM的浏览器会将一个event对象传入到事件处理程序中。无论指定事件处理程序时使用什么方法（DOM0级或DOM2级）都会传入event对象

  ------

  |         属性/方法          |     类型     | 读/写 |                             说明                             |
  | :------------------------: | :----------: | :---: | :----------------------------------------------------------: |
  |          bubbles           |   Boolean    |   r   |                         事件是否冒泡                         |
  |         cancelable         |   Boolean    |   r   |                  是否可以取消事件的默认行为                  |
  |       currentTarget        |   Element    |   r   |           其事件处理程序当前正在处理事件的那个元素           |
  |      defaultPrevented      |   Boolean    |   r   |        为 true 表 示 已 经 调 用 了 preventDefault()         |
  |           detail           |   Integer    |   r   |                     与事件相关的细节信息                     |
  |         eventPhase         |   Integer    |   r   | 调用事件处理程序的阶段： 1表示捕获阶段， 2表示处于目标，3表示冒泡阶段 |
  |      preventDefault()      |     fun      |   r   | 取消事件的默认行为。如 果 cancelable是true，则可以使用这个方法 |
  | stopImmediatePropagation() |     fun      |   r   | 取消事件的进一步捕获或冒泡，同时阻止任何事件处理程序被调用（DOM3） |
  |      stopPropagation       |     fun      |   r   | 取消事件的进一步捕获或冒泡。如果bubbles为true，则可以使用这个方法 |
  |           target           |      em      |   r   |                          事件的目标                          |
  |          trusten           |   boolean    |   r   | 为true表示事件是浏览器生成的。为false表示 事 件 是 由 开 发 人 员 通 过 JavaScript 创 建 的 |
  |            type            |    string    |   r   |                       被触发的事件类型                       |
  |            view            | AbstractView |   r   |            与事件关联的抽象视图。等同于发生事件的            |

  在事件处理程序内部，对象this始终等于 currentTarget的值，而target则只包含事件的实际目标。如果直接将事件处理程序给了目标元素，则this、currentTarget和target包含相同的值：

  ```js
  var btn = document.getElementById('myBtn');
  btn.onclick = function(event){
      alert(event.currentTarget === this); // true
      alert(event.target === this); // true
  }
  ```

  如果事件处理程序存在于按钮的父节点中（如document.body）,那么这些值是不同的：

  ```js
  document.body.onclick = function(event){
      alert(event.currentTarget === document.body); // true
      alert(this === document.body);  // true
      alert(event.target === document.getElementById('myBen')); //true
  }
  ```

  通过一个函数处理多个事件时，可用type属性：

  ```js
  var btn = document.getElementById('myBtn');
  var handler = function(event){
      switch(event.type){
          case 'click':
              alert('clicken');
              break;
          case 'mouseover':
              event.target.style.backgroundColor = "red";
              break;
          case 'click':
              event.target.style.backgroundColor = "";
              break;
      }
  }
  btn.onclick = handler;
  btn.onmouseover = handler;
  btn.onmouseout = handler;
  ```

  要阻止特定事件的默认行为，可以使用 **preventDefault()**方法。只有cancelable属性设置为true时事件，才可以使用preventDefault()来取消默认行为。

  **stopPropagation()**方法用于立即停止事件在DOM层次中的传播，即取消进一步的事件捕获或冒泡

  事件对象的**eventPhase** 属性，可以用来确定事件当前正位于事件流的哪个阶段。如果是在捕获阶段调用的事件处理程序，那么 eventPhase 等于 1；如果事件处理程序处于目标对象上，则 eventPhase 等于 2；如果是在冒泡阶段调用的事件处理程序， eventPhase 等于 3。这里要注意的是，尽管“处于目标”发生在冒泡阶段，但 eventPhase 仍然一直等于 2。

  ```js
  var btn = document.getElementById('myBtn');
  btn.onclick = function(event){
      alert(event.eventPhase); // 2
  }
  document.body.addEventListener('click',function(event){
      alert(event.eventPhase); // 1
  })
  document.body.onclick = function(event){
      alert(event.eventPhase); // 3
  }
  ```

- IE中的事件对象

  访问IE中的event对象有几种不同的方式，取决于指定事件处理程序的方法。

  - 使用DOM0级方法添加事件处理程序：

    event对象作为window对象的一个属性存在

    ```js
    var btn = document.getElementById("myBtn");
    btn.onclick = function(){
    	var event = window.event;
    	alert(event.type); //"click"
    };
    ```

  - 使用attachEvent()添加事件处理程序：

    event对象最为参数被传入事件处理程序函数中，也可以通过window对象来访问event对象

    ```js
    var btn = document.getElementById("myBtn");
    btn.attachEvent("onclick", function(event){
    	alert(event.type); //"click"
    });
    ```

  - 通过HTML特性指定的事件处理程序：

    可以通过一个名叫event的变量来访问event对象,就像使用DOM0级方法时一样

    ```html
    <input type="button" value="Click Me" onclick="alert(event.type)">
    ```

  | 属性/方法    | 类型    | 读/写 | 说明                                                         |
  | ------------ | ------- | ----- | ------------------------------------------------------------ |
  | cancelBubble | Boolean | 读/写 | 默认值为false,但将其设置为true就可以取消事件冒泡（与DOM中的stopPropagation()方法作用相同） |
  | returnValue  | Boolean | 读/写 | 默认为true，但将其设置为false就可以取消事件的默认行为（与DOM中的preventDefaule()方法作用相同） |
  | secElement   | El      | r     | 事件的目标（与DOM中的target属性相同）                        |
  | type         | String  | r     | 被触发的事件类型                                             |

  因为事件处理程序的作用域是根据指定它的方式来确定的，所以不能认为 this 会始终等于事件目标。故而，最好还是使用 **event.srcElement** 比较保险。

   **returnValue** 属性相当于 DOM 中的 preventDefault()方法，它们的作用都是取消
  给定事件的默认行为。只要将 returnValue 设置为 false，就可以阻止默认行为。

   **cancelBubble** 属性与 DOM 中的 stopPropagation()方法作用相同，都是用来停止事件冒泡的。

- 跨浏览器的事件对象

  ```js
  var EventUtil = {
      // 添加事件处理程序
  	addHandler: function(element, type, handler){
      	if (element.addEventListener){
          	element.addEventListener(type, handler, false);
          } else if (element.attachEvent){
          	element.attachEvent("on" + type, handler);
          } else {
          	element["on" + type] = handler;
          }
      },
      // 移除事件处理程序
      removeHandler: function(element, type, handler){
      	if (element.removeEventListener){
          	element.removeEventListener(type, handler, false);
          } else if (element.detachEvent){
          	element.detachEvent("on" + type, handler);
          } else {
          	element["on" + type] = null;
          }
      },
      // 获取事件对象
      getEvent: function(event){
      	return event ? event : window.event;
      },
      // 获取事件目标
      getTarget: function(event){
      	return event.target || event.srcElement;
      },
      // 取消事件默认行为
      preventDefault: function(event){
          if (event.preventDefault){
          	event.preventDefault();
          } else {
          	event.returnValue = false;
      	}
      },
     // 阻止事件冒泡
      stopPropagation: function(event){
          if (event.stopPropagation){
          	event.stopPropagation();
          } else {
          	event.cancelBubble = true;
          }
      }
  };
  ```