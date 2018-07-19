## 事件类型

DOM3级事件分类：

* UI（user interface 用户界面）事件，当用户与页面上的元素交互时触发
* 焦点事件，当元素获得或失去焦点时触发
* 鼠标事件，当用户通过鼠标在页面上执行操作时触发
* 滚轮事件，当使用鼠标滚轮（或类似设备）时触发
* 文本事件，当在文档中输入文本时触发
* 键盘事件，当用户通过键盘在页面上执行操作时触发
* 合成事件，当为IME（Input Method Editor，输入法编辑器）输入字符时触发
* 变动（mutation）事件，当底层DOM结构发生变化时触发
* ~~变动名称事件 （废弃）~~

### UI事件

UI事件指的是哪些不一定与用户操作有关的事件。

* ~~DOMActivate~~:表示元素已经被用户操作。（DOM3废弃）
* load：当叶脉你完全加载后在window上面触发，当所有框架都加载完时在框架集上面触发，当图像加载完毕时在<img>元素上面触发，或者当嵌入的内容加载完毕时在<object>元素上面触发
* unload：当页面完全卸载后在 window 上面触发，当所有框架都卸载后在框架集上面触发，或者当嵌入的内容卸载完毕后在<object>元素上面触发。
* abort：在用户停止下载过程时，如果嵌入的内容没有加载完，则在<object>元素上面触发。
* error：当发生 JavaScript 错误时在 window 上面触发，当无法加载图像时在<img>元素上面触发，当无法加载嵌入内容时在<object>元素上面触发，或者当有一或多个框架无法加载时在框架集上面触发。
* select：当用户选择文本框（<input>或<texterea>）中的一或多个字符时触发。
* resize：当窗口或框架的大小变化时在 window 或框架上面触发。
* scroll：当用户滚动带滚动条的元素中的内容时，在该元素上面触发。 <body>元素中包含所加载页面的滚动条

1. load事件

   当页面完全加载后（包括所有图像，js文件，css文件等外部资源），就触发window 上面的load事件。

   可以在 HTML 中为任何图像指定 onload 事件处理程序

   ```js
   //方式1
   <img src="smile.gif" onload="alert('Image loaded.')">
   // 方式2
   var image = document.getElementById("myImage");
   	EventUtil.addHandler(image, "load", function(event){
   	event = EventUtil.getEvent(event);
   	alert(EventUtil.getTarget(event).src);
   });
   ```

   > 在创建新的<img>元素时，可以为其指定一个事件处理程序，以便图像加载完毕后给出提示。

   ```js
   EventUtil.addHandler(window,'load',function(){
       var image = document.createElement('img');
       // 在指定src属性之前先指定事件
       EventUtil.addHandler(image,"load",function(event){
           event = EventUtil.getEvent(event)
           alert(EventUtil.getTarget(event).src)
       })
       document.body.appendChild(image);
       image.src = 'img.png'
   })
   ```

   **新图像元素不一定要从添加到文档后才开始下载，只要设置了 src 属性就会开始下载。**

   在 IE9+、 Firefox、 Opera、 Chrome 和 Safari 3+及更高版本中， <script>元素也会触发 load 事件，以便开发人员确定动态加载的 JavaScript 文件是否加载完毕。与图像不同，只有在设置了<script>元素的 src 属性并将该元素添加到文档后，才会开始下载 JavaScript 文件。换句话说，对于<script>元素而言，指定 src 属性和指定事件处理程序的先后顺序就不重要了。

   ```js
   EventUtil.addHandler(window,'load',function(){
       var script = document.createElement('script')
       EventUtil.addHandler(script,'load',function(event){
           alert('script loaded')
       })
       script.src = 'example.js'
       document.body.appendChild(script)
   })
   ```

   > IE 和 Opera 还支持<link>元素上的 load 事件，以便开发人员确定样式表是否加载完毕。

2. unload事件

   与load事件对应的是unload事件，这个事件在文档被完全卸载后触发。只要用户从一个页面切换到另一个页面，就会发生unload事件。而利用这个事件最多的情况是清除引用，以避免内存泄露。

   ```js
   EventUtil.addHandler(window, "unload", function(event){
   	alert("Unloaded");
   });
   ```

   ```html
   <body onunload="alert('Unloaded!')"></body>
   ```

   >无论使用哪种方式，都要小心编写 onunload 事件处理程序中的代码。既然 unload 事件是在一切都被卸载之后才触发，那么在页面加载后存在的那些对象，此时就不一定存在了。此时，操作 DOM 节点或者元素的样式就会导致错误。

3. resize事件

   当浏览器窗口被调整到一个新的高度或宽度时，就会触发resize事件。这个事件在window上触发，因此可以通过js或者<body>元素中的onresize特性来指定事件处理程序

   ```js
   EventUtil.addHandler(window, "resize", function(event){
       alert("Resized");
   });
   ```

   >与其他发生在 window 上的事件类似，在兼容 DOM 的浏览器中，传入事件处理程序中的 event 对象有一个 target 属性，值为 document；而 IE8 及之前版本则未提供任何属性。

   IE、 Safari、 Chrome 和 Opera 会在浏览器窗口变化了 1 像素时就触发 resize 事件，然后随着变化不断重复触发。 Firefox 则只会在用户停止调整窗口大小时才会触发 resize 事件。(防止大量计算)

4. scroll事件

   在混杂模式下，可以通过<body>元素的 scrollLeft 和 scrollTop 来监控到这一变化；而在标准模式下，除 Safari 之外的所有浏览器都会通过<html>元素来反映这一变化（Safari 仍然基于<body>跟踪滚动位置），如下面的例子所示：

   ```js
   EventUtil.addHandler(window, "scroll", function(event){
   	if (document.compatMode == "CSS1Compat"){
   		alert(document.documentElement.scrollTop);
   	} else {
   		alert(document.body.scrollTop);
   	}
   });
   ```

   > scroll 事件也会在文档被滚动期间重复被触发，

### 焦点事件

焦点事件会在页面元素获得或失去焦点时触发。利用这些事件并与 document.hasFocus()方法及document.activeElement 属性配合，可以知晓用户在页面上的行踪。

* blur：在元素失去焦点时触发，这个事件不会冒泡；所有浏览器都支持它

* focus：在元素获得检点时触发，这个事件不会冒泡；所有浏览器都支持它

* focusin：在元素获得焦点时触发。这个事件与 HTML 事件 focus 等价，但它冒泡。支持这个事件的浏览器有 IE5.5+、 Safari 5.1+、 Opera 11.5+和 Chrome。

* focusout：在元素失去焦点时触发。这个事件是 HTML 事件 blur 的通用版本。支持这个事件的浏览器有 IE5.5+、 Safari 5.1+、 Opera 11.5+和 Chrome。

* ~~DOMFocusOut：在元素失去焦点时触发。这个事件是 HTML 事件 blur 的通用版本。只有 Opera支持这个事件。 DOM3 级事件废弃了 DOMFocusOut，选择了 focusout。~~

* ~~DOMFocusOut：在元素失去焦点时触发。这个事件是 HTML 事件 blur 的通用版本。只有 Opera支持这个事件。 DOM3 级事件废弃了 DOMFocusOut，选择了 focusout。~~

  当焦点从页面中过的一个元素移动到另外一个元素，会依次触发下列事件：

  (1) focusout 在失去焦点的元素上触发；
  (2) focusin 在获得焦点的元素上触发；
  (3) blur 在失去焦点的元素上触发；
  (4) DOMFocusOut 在失去焦点的元素上触发；
  (5) focus 在获得焦点的元素上触发；
  (6) DOMFocusIn 在获得焦点的元素上触发。

### 鼠标与滚轮事件

* click：在用户单击主鼠标按钮或者按下回车键时触发
* dblclick：在用户双击主鼠标按钮时触发。
* mousedown：在用户按下任意鼠标时触发。不能通过键盘触发
* mouseenter：在鼠标光标从元素外部首次移动到元素范围之内时触发。不冒泡且在光标移动到后代元素上不会触发。
* mouseleave：在位于元素上方的鼠标光标移动到元素范围之外时触发。不冒泡且在光标移动到后代元素上不会触发。
* mousemove：当鼠标指针在元素内部移动时重复地触发。不能通过键盘触发
* mouseout：在鼠标指针位于一个元素上方，然后用户将其移入另一个元素时触发。又移入的另一个元素可能位于前一个元素的外部。也可能时这个元素的子元素。
* mouseover：在鼠标指针位于一个元素外部，然后用户将其首次移入另一个元素边界之内时触发。
* mouseup：在用户回访鼠标按钮时触发。no键盘

页面上所有元素都支持鼠标时间。除了mouseenter和mouseleave,所有鼠标事件都会冒泡，也可以被取消，而取消鼠标事件将会影响浏览器默认行为。取消鼠标事件的默认行为会影响其他事件，因为鼠标事件与其他事件是密不可分的关系。

​	只有在同一个元素上相继触发 mousedown 和 mouseup事件，才会触发 click事件；如果mousedown 或 mouseup中的一个被取消，就不会触发click事件。类似地，只有触发两次click事件，才会触发一次dblclick事件。如果阻止了连续两次触发click事件，那么就不会触发dblclick事件了。

1. mousedown
2. mouseup
3. click
4. mousedown
5. mouseup
6. click
7. dblclick

> click和dblclick事件都会依赖于其他先行事件的触发；mousedown和mouseup则不受其他事件的影响

```js
// 检测浏览器是否支持DOM2级事件
var isSupported = document.implementation.hasFeature("MouseEvents", "2.0");
// 检测浏览器是否支持DOM3级事件
var isSupported = document.implementation.hasFeature("MouseEvent", "3.0")
```

* 鼠标滚轮事件 mousewheel。跟踪鼠标滚轮

1. 客户区坐标位置

   鼠标事件都是在浏览器视口中的特定位置上发生的。这个位置信息保存在事件对象的 clientX 和clientY 属性中。所有浏览器都支持这两个属性，它们的值表示事件发生时鼠标指针在视口中的水平和垂直坐标。

2. 页面坐标位置

   页面坐标通过事件对象的 pageX 和pageY 属性，能告诉你事件是在页面中的什么位置发生的。换句话说，这两个属性表示鼠标光标在页面中的位置，因此坐标是从页面本身而非视口的左边和顶边计算的。

   在页面没有滚动的情况下， pageX 和 pageY 的值与 clientX 和 clientY 的值相等。

   > IE8 及更早版本不支持事件对象上的页面坐标，不过使用客户区坐标和滚动信息可以计算出来。

3. 屏幕坐标位置

   相对于整个电脑屏幕的位置。而通过 screenX 和 screenY 属性就可以确定鼠标事件发生时鼠标指针相对于整个屏幕的坐标信息。

4. 修改键

   虽然鼠标事件主要是使用鼠标来触发的，但在按下鼠标时键盘上的某些键的状态也可以影响到所要采取的操作。这些修改键就是 Shift、 Ctrl、 Alt 和 Meta（在 Windows 键盘中是 Windows 键，在苹果机中是 Cmd 键） ，它们经常被用来修改鼠标事件的行为。 DOM 为此规定了 4 个属性，表示这些修改键的状态： **shiftKey、 ctrlKey、 altKey 和 metaKey**。这些属性中包含的都是布尔值，如果相应的键被按下了，则值为 true，否则值为 false。当某个鼠标事件发生时，通过检测这几个属性就可以确定用户是否同时按下了其中的键。

5. 相关元素

   在发生 mouseover 和 mouserout 事件时，还会涉及更多的元素。这两个事件都会涉及把鼠标指针从一个元素的边界之内移动到另一个元素的边界之内。对 mouseover 事件而言，事件的主目标是获得光标的元素，而相关元素就是那个失去光标的元素。类似地，对 mouseout 事件而言，事件的主目标是失去光标的元素，而相关元素则是获得光标的元素。

   DOM 通过 event 对象的 relatedTarget 属性提供了相关元素的信息。这个属性只对于 mouseover和 mouseout 事件才包含值；对于其他事件，这个属性的值是 null。IE8及之前版本不支持 relatedTarget属性，但提供了保存着同样信息的不同属性。在 mouseover 事件触发时， IE 的 fromElement 属性中保存了相关元素；在 mouseout 事件触发时， IE 的 toElement 属性中保存着相关元素。（IE9 支持所有这些
   属性。）

   ```js
   // 获取相关元素方法
   getRelatedTarget: function(event){
   	if (event.relatedTarget){
   		return event.relatedTarget;
   	} else if (event.toElement){
       	return event.toElement;
       } else if (event.fromElement){
       	return event.fromElement;
       } else {
       	return null;
       }
   },
   ```

6. 鼠标按钮

    只有在主鼠标按钮被单击（或键盘回车键被按下）时才会触发 click 事件，因此检测按钮的信息并不是必要的。但对于 mousedown 和 mouseup 事件来说，则在其 event 对象存在一个 button 属性，表示按下或释放的按钮。 DOM 的 button 属性可能有如下 3 个值： 0 表示主鼠标按钮， 1 表示中间的鼠标按钮（鼠标滚轮按钮） ， 2 表示次鼠标按钮。在常规的设置中，主鼠标按钮就是鼠标左键，而次鼠标按钮就是鼠标右键。

    > IE8 及之前版本也提供了 button 属性，但这个属性的值与 DOM 的 button 属性有很大差异

   ```js
   getButton: function(event){
       if (document.implementation.hasFeature("MouseEvents", "2.0")){
       	return event.button;
       } else {
           switch(event.button){
               case 0:
               case 1:
               case 3:
               case 5:
               case 7:
                   return 0;
               case 2:
               case 6:
                   return 2;
               case 4:
                   return 1;
           }
       }
   },
   ```

   ​

7. 更多事件信息

   “ DOM2 级事件”规范在 event 对象中还提供了 detail 属性，用于给出有关事件的更多信息。对于鼠标事件来说， detail 中包含了一个数值，表示在给定位置上发生了多少次单击。在同一个元素上相继地发生一次 mousedown 和一次 mouseup 事件算作一次单击。 detail 属性从 1 开始计数，每次单击发生后都会递增。如果鼠标在 mousedown 和 mouseup 之间移动了位置，则 detail 会被重置为 0。

8. 鼠标滚轮事件

   当用户通过鼠标滚轮与页面交互、在垂直方向上滚动页面时（无论向上还是向下），就会触发 mousewheel事件。这个事件可以在任何元素上面触发，最终会冒泡到 document（IE8）或 window（IE9、 Opera、Chrome 及 Safari）对象。与 mousewheel 事件对应的 event 对象除包含鼠标事件的所有标准信息外，还包含一个特殊的 wheelDelta 属性。当用户向前滚动鼠标滚轮时， wheelDelta 是 120 的倍数；当用户向后滚动鼠标滚轮时， wheelDelta 是-120 的倍数。

   多数情况下，只要知道鼠标滚轮滚动的方向就够了，而这通过检测 wheelDelta 的正负号就可以确定。在 Opera 9.5 之前的版本中， wheelDelta 值的正负号是颠倒的。

   Firefox 支持一个名为 DOMMouseScroll 的类似事件，也是在鼠标滚轮滚动时触发。与 mousewheel事件一样， DOMMouseScroll 也被视为鼠标事件，因而包含与鼠标事件有关的所有属性。而有关鼠标滚轮的信息则保存在 detail 属性中，当向前滚动鼠标滚轮时，这个属性的值是-3 的倍数，当向后滚动鼠标滚轮时，这个属性的值是 3 的倍数。

   ```js
   getWheelDelta: function(event){
       if (event.wheelDelta){
       	return (client.engine.opera && client.engine.opera < 9.5 ?-event.wheelDelta : event.wheelDelta);
       } else {
       	return -event.detail * 40;
       }
   }
   ```

9. 触摸设备

   * 不支持 dblclick 事件。双击浏览器窗口会放大画面，而且没有办法改变该行为。
   *  轻击可单击元素会触发 mousemove 事件。如果此操作会导致内容变化，将不再有其他事件发生；如果屏幕没有因此变化，那么会依次发生 mousedown、 mouseup 和 click 事件。轻击不可单击的元素不会触发任何事件。可单击的元素是指那些单击可产生默认操作的元素（如链接） ，或者那些已经被指定了 onclick 事件处理程序的元素。
   *  mousemove 事件也会触发 mouseover 和 mouseout 事件。
   * 两个手指放在屏幕上且页面随手指移动而滚动时会触发 mousewheel 和 scroll 事件。

10. 无障碍性问题

   我们不建议使用 click 之外的其他鼠标事件来展示功能或引发代码执行。因为这样会给盲人或视障用户造成极大不便。

   * 使用 click 事件执行代码。有人指出通过 onmousedown 执行代码会让人觉得速度更快，对视力正常的人来说这是没错的。但是，在屏幕阅读器中，由于无法触发 mousedown 事件，结果就会造成代码无法执行。
   * 不要使用 onmouseover 向用户显示新的选项。原因同上，屏幕阅读器无法触发这个事件。如果确实非要通过这种方式来显示新选项，可以考虑添加显示相同信息的键盘快捷方式。
   * 不要使用 dblclick 执行重要的操作。键盘无法触发这个事件。

### 键盘与文本事件

对键盘事件的支持主要遵循的是 DOM0 级。

* keydown：当用户按下键盘上的***任意键***时触发，而且如果按住不放的话，会重复触发此事件。
* keypress：当用户按下键盘上的***字符键***时触发，而且如果按住不放的话，会重复触发此事件。按下 Esc 键也会触发这个事件。Safari 3.1 之前的版本也会在用户按下非字符键时触发 keypress事件。


* keyup：当用户释放键盘上的键时触发。

虽然所有元素都支持以上 3 个事件，但只有在用户通过文本框输入文本时才最常用到。

只有一个文本事件： textInput。这个事件是对 keypress 的补充，用意是在将文本显示给用户之前更容易拦截文本。在文本插入文本框之前会触发 textInput 事件。

​	在用户按了一下键盘上的字符键时，首先会触发 keydown 事件，然后紧跟着是 keypress 事件，最后会触发 keyup 事件。 其中， keydown 和 keypress 都是在文本框发生变化之前被触发的；而 keyup事件则是在文本框已经发生变化之后被触发的。如果用户按下了一个字符键不放，就会重复触发keydown 和 keypress 事件，直到用户松开该键为止。

 	如果用户按下的是一个非字符键，那么首先会触发 keydown 事件，然后就是 keyup 事件。如果按住这个非字符键不放，那么就会一直重复触发 keydown 事件，直到用户松开这个键，此时会触发 keyup事件。

1. 键码

   在发生 keydown 和 keyup 事件时， event 对象的 keyCode 属性中会包含一个代码，与键盘上一个特定的键对应。对数字字母字符键， keyCode 属性的值与 ASCII 码中对应小写字母或数字的编码相同

2. 字符编码

   发生 keypress 事件意味着按下的键会影响到屏幕中文本的显示。在所有浏览器中，按下能够插入或删除字符的键都会触发 keypress 事件；

   IE9、 Firefox、 Chrome 和 Safari 的 event 对象都支持一个 charCode 属性，这个属性只有在发生keypress 事件时才包含值，而且这个值是按下的那个键所代表字符的 ASCII 编码。此时的 keyCode通常等于 0 或者也可能等于所按键的键码。IE8 及之前版本和 Opera 则是在 keyCode 中保存字符的 ASCII编码。要想以跨浏览器的方式取得字符编码，必须首先检测 charCode 属性是否可用，如果不可用则使用 keyCode.

   ```js
   getCharCode: function(event){
   	if (typeof event.charCode == "number"){
   		return event.charCode;
   	} else {
   		return event.keyCode;
   	}
   }
   // 在取得了字符编码之后，就可以使用 String.fromCharCode()将其转换成实际的字符
   ```

3. DOM3级变化(了解)

    DOM3级事件中的键盘事件，不再包含 charCode 属性，而是包含两个新属性： key 和 char。

    key 属性是为了取代 keyCode 而新增的，它的值是一个字符串。在按下某个字符键时， key的值就是相应的文本字符（如“ k”或“ M”）；在按下非字符键时， key 的值是相应键的名（如“ Shift”或“ Down”） 。而 char 属性在按下字符键时的行为与 key 相同，但在按下非字符键时值为 null

    DOM3 级事件还添加了一个名为 location 的属性，这是一个数值，表示按下了什么位置上的键：
      0 表示默认键盘， 1 表示左侧位置（例如左位的 Alt 键）， 2 表示右侧位置（例如右侧的 Shift 键）， 3 表示数字小键盘， 4 表示移动设备键盘（也就是虚拟键盘）， 5 表示手柄（如任天堂 Wii 控制器）

    event 对象添加了 getModifierState()方法。这个方法接收一个参数，即等于 Shift、
      Control、 AltGraph 或 Meta 的字符串，表示要检测的修改键。

4. textTnput事件

   DOM3级事件 规范中引入一个新事件，**textInput** 当用户在可编辑区域中输入字符时，就会触发这个事件。这个用于替代 keypress 的 textInput 事件的行为稍有不同。**区别之一**就是任何可以获得焦点的元素都可以触发 keypress 事件，但只有可编辑区域才能触发 textInput事件。**区别之二**是 textInput 事件只会在用户按下能够输入实际字符的键时才会被触发，而 keypress事件则在按下那些能够影响文本显示的键时也会触发（例如退格键）。由于 textInput 事件主要考虑的是字符，因此它的 event 对象中还包含一个 data 属性，这个属性的值就是用户输入的字符（而非字符编码）。换句话说，用户在没有按上档键的情况下按下了 S 键，data 的值就是"s"，而如果在按住上档键时按下该键， data 的值就是"S"。

    event 对象上还有一个属性，叫 inputMethod，表示把文本输入到文本框中的方式。

    0，表示浏览器不确定是怎么输入的。
    1，表示是使用键盘输入的。
    2，表示文本是粘贴进来的。
    3，表示文本是拖放进来的。
    4，表示文本是使用 IME 输入的。
    5，表示文本是通过在表单中选择某一项输入的。
    6，表示文本是通过手写输入的（比如使用手写笔）。
    7，表示文本是通过语音输入的。
    8，表示文本是通过几种方法组合输入的。
    9，表示文本是通过脚本输入的。
   使用这个属性可以确定文本是如何输入到控件中的，从而可以验证其有效性。支持 textInput 属性的浏览器有 IE9+、 Safari 和 Chrome。只有 IE 支持 inputMethod 属性。

5. 设备中的键盘事件

### 复合事件

复合事件（composition event）是DOM3级事件中新添加的一类事件。用于处理IME的输入序列。IME（input Method Editor 输入法编辑器）可以让用户输入在物理键盘上找不到的字符。IME通常需要同时按住多个键，但最终只输入一个字符。复合事件就是针对检测和处理这种输入而设计的。

* compositionstart：在 IME 的文本复合系统打开时触发，表示要开始输入了
* compositionupdate：在向输入字段中插入新字符时触发。
* compositionend：在 IME 的文本复合系统关闭时触发，表示返回正常键盘输入状态

复合事件与文本事件在很多方面都很相似。在触发复合事件时，目标是接收文本的输入字段。但它比文本事件的事件对象多一个属性 data

* 如果在 compositionstart 事件发生时访问，包含正在编辑的文本（例如，已经选中的需要马上替换的文本）；

* 如果在 compositionupdate 事件发生时访问，包含正插入的新字符；

* 如果在 compositionend 事件发生时访问，包含此次输入会话中插入的所有字符

  > 要确定浏览器是否支持复合事件，可以使用以下代码：

  ```js
  var isSupported = document.implementation.hasFeature("CompositionEvent", "3.0");
  ```

### 变动事件

DOM2 级的变动（mutation）事件能在 DOM 中的某一部分发生变化时给出提示。变动事件是为 XML或 HTML DOM 设计的，并不特定于某种语言。 DOM2 级定义了如下变动事件。

* DOMSubtreeModified：在 DOM 结构中发生任何变化时触发。这个事件在其他任何事件触发后都会触发。
* DOMNodeInserted：在一个节点作为子节点被插入到另一个节点中时触发。
* DOMNodeRemoved：在节点从其父节点中被移除时触发。
* DOMNodeInsertedIntoDocument：在一个节点被直接插入文档或通过子树间接插入文档之后触发。这个事件在 DOMNodeInserted 之后触发。
*  DOMNodeRemovedFromDocument：在一个节点被直接从文档中移除或通过子树间接从文档中移除之前触发。这个事件在 DOMNodeRemoved 之后触发。
*  DOMAttrModified：在特性被修改之后触发。
* DOMCharacterDataModified：在文本节点的值发生变化时触发。

 DOM3 级事件模块作废了很多变动事件

1. 删除节点

   在使用removeChild()或replaceChild()从 DOM中删除节点时，首先会触发DOMNodeRemoved事件。这个事件的目标（event.target）是被删除的节点，而 event.relatedNode 属性中包含着对目标节点父节点的引用。在这个事件触发时，节点尚未从其父节点删除，因此其 parentNode 属性仍然指向父节点（与 event.relatedNode 相同）。这个事件会冒泡，因而可以在 DOM 的任何层次上面处
   理它。

   如果被移除的节点包含子节点，那么在其所有子节点以及这个被移除的节点上会相继触发DOMNodeRemovedFromDocument 事件。但这个事件不会冒泡，所以只有直接指定给其中一个子节点的事件处理程序才会被调用。这个事件的目标是相应的子节点或者那个被移除的节点，除此之外 event对象中不包含其他信息。
   紧随其后触发的是 DOMSubtreeModified 事件。这个事件的目标是被移除节点的父节点；此时的event 对象也不会提供与事件相关的其他信息。

2. 插入节点

   在使用 appendChild()、 replaceChild()或 insertBefore()向 DOM 中插入节点时，首先会触发 DOMNodeInserted 事件。这个事件的目标是被插入的节点，而 event.relatedNode 属性中包含一个对父节点的引用。在这个事件触发时，节点已经被插入到了新的父节点中。这个事件是冒泡的，因此可以在 DOM 的各个层次上处理它。
   紧接着，会在新插入的节点上面触发 DOMNodeInsertedIntoDocument 事件。这个事件不冒泡，因此必须在插入节点之前为它添加这个事件处理程序。这个事件的目标是被插入的节点，除此之外event 对象中不包含其他信息。
   最后一个触发的事件是 DOMSubtreeModified，触发于新插入节点的父节点。

### HTML5事件

1. contextmenu 事件

    用以表示何时应该显示上下文菜单(右键菜单)，以便开发人员取消默认的上下文菜单而提供自定义的菜单。

   ```js
   EventUtil.addHandler(window, "load", function(event){
       var div = document.getElementById("myDiv");
       EventUtil.addHandler(div, "contextmenu", function(event){
           event = EventUtil.getEvent(event);
           EventUtil.preventDefault(event);
           var menu = document.getElementById("myMenu");
           menu.style.left = event.clientX + "px";
           menu.style.top = event.clientY + "px";
           menu.style.visibility = "visible";
       });
       EventUtil.addHandler(document, "click", function(event){
       	document.getElementById("myMenu").style.visibility = "hidden";
       });
   });
   ```

2. beforeunload 事件

   之所以有发生在 window 对象上的 beforeunload 事件，是为了让开发人员有可能在页面卸载前阻止这一操作。(关闭窗口是提示用户是否真的要关闭)

   为了显示这个弹出对话框，必须将 event.returnValue 的值设置为要显示给用户的字符串

   ```js
   EventUtil.addHandler(window, "beforeunload", function(event){
       event = EventUtil.getEvent(event);
       var message = "I'm really going to miss you if you go.";
       event.returnValue = message;
       return message;
   });
   ```

   ​

3. DOMContentLoaded 事件

    DOMContentLoaded 事件则在形成完整的 DOM 树之后就会触发，
   不理会图像、 JavaScript 文件、 CSS 文件或其他资源是否已经下载完毕。与 load 事件不同，DOMContentLoaded 支持在页面下载的早期添加事件处理程序，这也就意味着用户能够尽早地与页面进行交互。

   DOMContentLoaded 事件对象不会提供任何额外的信息（其 target 属性是document）。通常这个事件既可以添加事件处理程序，也可以执行其他 DOM 操作。这个事件始终都会在 load 事件之前触发。

4. readystatechange 事件

   IE 为 DOM 文档中的某些部分提供了 readystatechange 事件。这个事件的目的是提供与文档或元素的加载状态有关的信息，但这个事件的行为有时候也很难预料。

5. pageshow 和 pagehide 事件

6. hashchange 事件

   HTML5 新增了 hashchange 事件，以便在 URL 的参数列表（及 URL 中“ #”号后面的所有字符串）发生变化时通知开发人员。之所以新增这个事件，是因为在 Ajax 应用中，开发人员经常要利用 URL 参数列表来保存状态或导航信息。

### 设备事件

1. orientationchange 事件

   苹果公司为移动 Safari 中添加了 orientationchange 事件，以便开发人员能够确定用户何时将设备由横向查看模式切换为纵向查看模式。移动 Safari 的window.orientation 属性中可能包含 3 个值：0 表示肖像模式， 90 表示向左旋转的横向模式（“主屏幕”按钮在右侧）， -90 表示向右旋转的横向模式（“主屏幕”按钮在左侧）。相关文档中还提到一个值，即 180 表示 iPhone 头朝下；但这种模式至今
   尚未得到支持。

   只要用户改变了设备的查看模式，就会触发 orientationchange 事件。此时的 event 对象不包含任何有价值的信息，因为唯一相关的信息可以通过 window.orientation 访问到。

2. MozOrientation 事件

   Firefox 3.6 为检测设备的方向引入了一个名为 MozOrientation 的新事件。（前缀 Moz 表示这是特定于浏览器开发商的事件，不是标准事件。）当设备的加速计检测到设备方向改变时，就会触发这个事件。但这个事件与 iOS 中的 orientationchange 事件不同，该事件只能提供一个平面的方向变化。

   event 对象包含三个属性： x、 y 和 z。这几个属性的值都介于 1 到-1 之间，表示不同坐标轴上的方向。在静止状态下， x 值为 0， y 值为 0， z 值为 1（表示设备处于竖直状态）。如果设备向右倾斜， x 值会减小；反之，向左倾斜， x 值会增大。类似地，如果设备向远离用户的方向倾斜， y 值会减小，向接近用户的方向倾斜， y 值会增大。 z 轴检测垂直加速度度， 1 表示静止不动，在设备移动时值会减小。（失重状态下值为 0。）

   只有带加速计的设备才支持 MozOrientation 事件，

3. deviceorientation 事件

   DeviceOrientation Event 规范定义的 deviceorientation 事件与 MozOrientation 事件类似。它也是在加速计检测到设备方向变化时在 window 对象上触发，而且具有与 MozOrientation 事件相同的支持限制。不过， deviceorientation 事件的意图是告诉开发人员设备在空间中朝向哪儿，而不是如何移动。

   设备在三维空间中是靠 x、 y 和 z 轴来定位的。当设备静止放在水平表面上时，这三个值都是 0。 x轴方向是从左往右， y 轴方向是从下往上， z 轴方向是从后往前

   触发 deviceorientation 事件时，事件对象中包含着每个轴相对于设备静止状态下发生变化的信息。事件对象包含以下 5 个属性。
    alpha：在围绕 z 轴旋转时（即左右旋转时）， y 轴的度数差；是一个介于 0 到 360 之间的浮点数。
    beta：在围绕 x 轴旋转时（即前后旋转时） ， z 轴的度数差；是一个介于180 到 180 之间的浮点数。
    gamma：在围绕 y 轴旋转时（即扭转设备时） ， z 轴的度数差；是一个介于90 到 90 之间的浮点数。
    absolute：布尔值，表示设备是否返回一个绝对值。
    compassCalibrated：布尔值，表示设备的指南针是否校准过。

4.  devicemotion 事件

    devicemotion 事件。这个事件是要告诉开发人员设备
      什么时候移动，而不仅仅是设备方向如何改变。例如，通过 devicemotion 能够检测到设备是不是正在往下掉，或者是不是被走着的人拿在手里。

   触发 devicemotion 事件时，事件对象包含以下属性。
    acceleration：一个包含 x、 y 和 z 属性的对象，在不考虑重力的情况下，告诉你在每个方向上的加速度。
    accelerationIncludingGravity：一个包含 x、 y 和 z 属性的对象，在考虑 z 轴自然重力加速度的情况下，告诉你在每个方向上的加速度。
    interval：以毫秒表示的时间值，必须在另一个 devicemotion 事件触发前传入。这个值在每个事件中应该是一个常量

    rotationRate：一个包含表示方向的 alpha、 beta 和 gamma 属性的对象。

### 触摸与手势事件

1. 触摸事件

   触摸事件会在用户手指放在屏幕上面时、在屏幕上滑动时或从屏幕上移开时触发。

   * touchstart：当手指触摸屏幕时触发；即使已经有一个手指放在了屏幕上也会触发。
   *  touchmove：当手指在屏幕上滑动时连续地触发。在这个事件发生期间，调用 preventDefault()可以阻止滚动。
   *  touchend：当手指从屏幕上移开时触发。
   * touchcancel：当系统停止跟踪触摸时触发。关于此事件的确切触发时间，文档中没有明确说明。

   上面这几个事件都会冒泡，也都可以取消。虽然这些触摸事件没有在 DOM 规范中定义，但它们却是以兼容 DOM 的方式实现的。因此，每个触摸事件的 event 对象都提供了在鼠标事件中常见的属性：
   bubbles、cancelable、view、clientX、clientY、screenX、screenY、detail、altKey、shiftKey、ctrlKey 和 metaKey。

   除了常见的 DOM 属性外，触摸事件还包含下列三个用于跟踪触摸的属性。
    touches：表示当前跟踪的触摸操作的 Touch 对象的数组。
    targetTouchs：特定于事件目标的 Touch 对象的数组。
    changeTouches：表示自上次触摸以来发生了什么改变的 Touch 对象的数组。

   每个 Touch 对象包含下列属性。

    clientX：触摸目标在视口中的 x 坐标。
    clientY：触摸目标在视口中的 y 坐标。
    identifier：标识触摸的唯一 ID。
    pageX：触摸目标在页面中的 x 坐标。
    pageY：触摸目标在页面中的 y 坐标。
    screenX：触摸目标在屏幕中的 x 坐标。
    screenY：触摸目标在屏幕中的 y 坐标。
    target：触摸的 DOM 节点目标。

2. 手势事件

   iOS 2.0 中的 Safari 还引入了一组手势事件。当两个手指触摸屏幕时就会产生手势，手势通常会改变显示项的大小，或者旋转显示项。有三个手势事件，分别介绍如下。
    gesturestart：当一个手指已经按在屏幕上而另一个手指又触摸屏幕时触发。
    gesturechange：当触摸屏幕的任何一个手指的位置发生变化时触发。
    gestureend：当任何一个手指从屏幕上面移开时触发。

   event 对象都包含两个额外的属性： rotation 和 scale。其中， rotation 属性表
   示手指变化引起的旋转角度，负值表示逆时针旋转，正值表示顺时针旋转（该值从 0 开始）。而 scale属性表示两个手指间距离的变化情况（例如向内收缩会缩短距离）；这个值从 1 开始，并随距离拉大而增长，随距离缩短而减小。

