## 模拟事件

事件，是网页中某个特别值得关注的瞬间，事件经常由用户操作或通过其他浏览器功能来触发。也可以使用JavaScript在任意时刻来触发特定的事件，而此时的事件就如同浏览器创建的事件一样。在测试web应用程序，模拟触发事件是一种极其有用的技术。

### DOM中的事件模拟

可以在document对象上使用createEvent()方法创建event对象。这个方法接收一个参数，即表示要创建的事件类型的字符串。

* UIEvents：一般化的 UI 事件。 鼠标事件和键盘事件都继承自 UI 事件。 DOM3 级中是 UIEvent。
* MouseEvents：一般化的鼠标事件。 DOM3 级中是 MouseEvent。
* MutationEvents：一般化的 DOM 变动事件。 DOM3 级中是 MutationEvent。
* HTMLEvents：一般化的 HTML 事件。没有对应的 DOM3 级事件（HTML 事件被分散到其他类别中）。

在创建了 event 对象之后，还需要使用与事件有关的信息对其进行初始化。每种类型的 event 对象都有一个特殊的方法，为它传入适当的数据就可以初始化该 event 对象。不同类型的这个方法的名字也不相同，具体要取决于 createEvent()中使用的参数。

模拟事件的最后一步就是触发事件。这一步需要使用 dispatchEvent()方法，所有支持事件的DOM 节点都支持这个方法。调用 dispatchEvent()方法时，需要传入一个参数，即表示要触发事件的 event 对象。触发事件之后，该事件就跻身“官方事件”之列了，因而能够照样冒泡并引发相应事件处理程序的执行。

```js
var btn = document.getElementById("myBtn");
//创建事件对象
var event = document.createEvent("MouseEvents");
//初始化事件对象
event.initMouseEvent("click", true, true, document.defaultView, 0, 0, 0, 0, 0,false, false, false, false, 0, null);
//触发事件
btn.dispatchEvent(event);
```



### IE中的事件模拟

