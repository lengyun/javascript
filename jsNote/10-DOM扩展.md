## 选择符API

### querySelector()方法

> querySelector()方法接收一个 CSS 选择符，返回与该模式匹配的第一个元素，如果没有找到匹
> 配的元素，返回 null。

```js
//取得 body 元素
var body = document.querySelector("body");
//取得 ID 为"myDiv"的元素
var myDiv = document.querySelector("#myDiv");
//取得类为"selected"的第一个元素
var selected = document.querySelector(".selected");
//取得类为"button"的第一个图像元素
var img = document.body.querySelector("img.button");
```

> 通过 Document 类型调用 querySelector()方法时，会在文档元素的范围内查找匹配的元素。而

> 通过 Element 类型调用 querySelector()方法时，只会在该元素后代元素的范围内查找匹配的元素。

### querySelectorAll()方法

>querySelectorAll()方法接收的参数与 querySelector()方法一样，都是一个 CSS 选择符，但返回的是所有匹配的元素而不仅仅是一个元素。这个方法返回的是一个 NodeList 的实例

```js
//取得某<div>中的所有<em>元素（类似于 getElementsByTagName("em")）
var ems = document.getElementById("myDiv").querySelectorAll("em");
//取得类为"selected"的所有元素
var selecteds = document.querySelectorAll(".selected");
//取得所有<p>元素中的所有<strong>元素
var strongs = document.querySelectorAll("p strong");
```

> 要取得返回的 NodeList 中的每一个元素，可以使用 item()方法，也可以使用方括号语法

```js
var i, len, strong;
for (i=0, len=strongs.length; i < len; i++){
	strong = strongs[i]; //或者 strongs.item(i)
	strong.className = "important";
}
```

### matchesSelector()方法

> 这个方法接收一个参数，即 CSS 选择符，如果调用元素与该选择符匹配，返回 true；否则，返回 false。
>
> 实验性的实现
>
> ```js
> function matchesSelector(element, selector){
>   if (element.matchesSelector){
>       return element.matchesSelector(selector);
>   } else if (element.msMatchesSelector){
>       return element.msMatchesSelector(selector);
>   } else if (element.mozMatchesSelector){
>       return element.mozMatchesSelector(selector);
>   } else if (element.webkitMatchesSelector){
>       return element.webkitMatchesSelector(selector);
>   } else {
>       throw new Error("Not supported.");
>   }
> }
> if (matchesSelector(document.body, "body.page1")){
> //执行操作
> }
> ```

## 元素遍历(IE 9+)

 childElementCount：返回子元素（不包括文本节点和注释）的个数。
 firstElementChild：指向第一个子元素； firstChild 的元素版。
 lastElementChild：指向最后一个子元素； lastChild 的元素版。
 previousElementSibling：指向前一个同辈元素； previousSibling 的元素版。
 nextElementSibling：指向后一个同辈元素； nextSibling 的元素版。

## HTML5

### 与类相关的扩展

* getElementsByClassName()方法( IE 9+)

  > 可以通过 document对象及所有 HTML 元素调用该方法

  > 方法接收一个参数，即一个包含一或多个类名的字符串，返回带有指定类的所有元素的 NodeList

  ```js
  //取得所有类中包含"username"和"current"的元素，类名的先后顺序无所谓
  var allCurrentUsernames = document.getElementsByClassName("username current");
  //取得 ID 为"myDiv"的元素中带有类名"selected"的所有元素
  var selected = document.getElementById("myDiv").getElementsByClassName("selected");
  ```

  > 使用这个方法可以更方便地为带有某些类的元素添加事件处理程序，从而不必再局限于使用 ID 或标签名。不过别忘了，因为返回的对象是 NodeList，所以使用这个方法与使用 getElementsByTagName()以及其他返回 NodeList 的 DOM 方法都具有同样的性能问题

* classList 属性 ( Firefox 3.6+和 Chrome)

  >  classList 属性是新集合类型 DOMTokenList 的实例,与其他 DOM 集合类似。
  >
  >  DOMTokenList 有一个表示自己包含多少元素的 length 属性，而要取得每个元素可以使用 item()方法，也可以使用方括号语法。此外，这个新类型还定义如下方法:

   add(value)：将给定的字符串值添加到列表中。如果值已经存在，就不添加了。
   contains(value)：表示列表中是否存在给定的值，如果存在则返回 true，否则返回 false。
   remove(value)：从列表中删除给定的字符串。
   toggle(value)：如果列表中已经存在给定的值，删除它；如果列表中没有给定的值，添加它。

  ```js
  //删除"disabled"类
  div.classList.remove("disabled");
  //添加"current"类
  div.classList.add("current");
  //切换"user"类
  div.classList.toggle("user");
  //确定元素中是否包含既定的类名
  if (div.classList.contains("bd") && !div.classList.contains("disabled")){
  //执行操作
  )
  //迭代类名
  for (var i=0, len=div.classList.length; i < len; i++){
  doSomething(div.classList[i]);
  }
  ```

###焦点管理

*  document.activeElement 属性

>  document.activeElement 属性，这个属性始终会引用 DOM 中当前获得了焦点的元素。
>
>  文档刚刚加载完成时， document.activeElement 中保存的是 document.body 元素的引用。文档加载期间， document.activeElement 的值为 null

*  document.hasFocus()方法

> document.hasFocus()方法，这个方法用于确定文档是否获得了焦点。
>
> 通过检测文档是否获得了焦点，可以知道用户是不是正在与页面交互。

###HTMLDocument的变化

* readyState属性

   loading，正在加载文档；
   complete，已经加载完文档。

  ```js
  if (document.readyState == "complete"){
  //执行操作
  }
  ```

  > 支持 readyState 属性的浏览器有 IE4+、 Firefox 3.6+、 Safari、 Chrome 和 Opera 9+。

* 兼容模式

  * 标准模式下： document.compatMode = CSS1Compat 
  * 混杂模式下： document.compatMode = BackCompat

* head属性

  ```js
  var head = document.head || document.getElementsByTagName("head")[0];
  ```

###字符集属性

* charset 

>  charset 属性表示文档中实际使用的字符集，也可以用来指定新字符集。默认情况下，这个属性的值为"UTF-16"，但可以通过<meta>元素、响应头部或直接设置 charset 属性修改这个值

* defaultCharset

>  defaultCharset，表示根据默认浏览器及操作系统的设置，当前文档默认的字符集应该是什么

###自定义数据属性

> HTML5 规定可以为元素添加非标准的属性，但要添加前缀 data-，目的是为元素提供与渲染无关的
> 信息，或者提供语义信息。这些属性可以任意添加、随便命名，只要以 data-开头即可
>
> ```html
> <div id="myDiv" data-appId="12345" data-myname="Nicholas"></div>
> ```
>
> 可以通过元素的 dataset 属性来访问自定义属性的值。dataset 属性的值是 DOMStringMap 的一个实例，也就是一个名值对儿的映射。在这个映射中，每个 data-name 形式的属性都会有一个对应的属性，只不过属性名没有 data-前缀

```js
var div = document.getElementById("myDiv");
//取得自定义属性的值
var appId = div.dataset.appId;
var myName = div.dataset.myname;
//设置值
div.dataset.appId = 23456;
div.dataset.myname = "Michael";
//有没有"myname"值呢？
if (div.dataset.myname){
	alert("Hello, " + div.dataset.myname);
}
```

###插入标记

*  innerHTML 属性

  * 在读模式下， innerHTML 属性返回与调用元素的所有子节点（包括元素、注释和文本节点）对应的 HTML 标记。
  * 在写模式下， innerHTML 会根据指定的值创建新的 DOM 树，然后用这个 DOM 树完全
    替换调用元素原先的所有子节点

  > 通过 innerHTML 插入<script>元素并不会执行其中的脚本。


* outerHTML 属性

  * 在读模式下， outerHTML 返回调用它的元素及所有子节点的 HTML 标签。
  * 在写模式下， outerHTML会根据指定的 HTML 字符串创建新的 DOM 子树，然后用这个 DOM 子树完全替换调用元素。

* insertAdjacentHTML()方法

  > 接收两个参数：插入位置和要插入的 HTML 文本。第一个参数必须是下列值之一：

   "beforebegin"，在当前元素之前插入一个紧邻的同辈元素；

   "afterend"，在当前元素之后插入一个紧邻的同辈元素

   "afterbegin"，在当前元素之下插入一个新的子元素或在第一个子元素之前再插入新的子元素；
   "beforeend"，在当前元素之下插入一个新的子元素或在最后一个子元素之后再插入新的子元素；

* 内存与性能问题

  * 在使用 innerHTML、outerHTML 属性和 insertAdjacentHTML()方法时，最好先手工删除要被替换的元素的所有事件处理程序和 JavaScript 对象属性
  * 在插入大量新 HTML 标记时，使用 innerHTML 属性与通过多次 DOM 操作先创建节点再指定它们之间的关系相比，效率要高得多

###scrollIntoView()方法

> scrollIntoView()可以在所有 HTML 元素上调用，通过滚动浏览器窗口或某个容器元素，调用
> 元素就可以出现在视口中。

>  true 作为参数，或者不传入任何参数，那么窗口滚动之后会让调用元素的顶部与视口顶部尽可能平齐。
>
>  false 作为参数，调用元素会尽可能全部出现在视口中，（可能的话，调用元素的底部会与视口顶部平齐。）不过顶部不一定平齐

## 专有扩展

###文档模式(ie)

> 强制浏览器以某种模式渲染页面,可以使用 HTTP 头部信息 X-UA-Compatible，或通过等价的
> <meta>标签来设置：
>
> ```html
> <meta http-equiv="X-UA-Compatible" content="IE=IEVersion">
> ```

 Edge：始终以最新的文档模式来渲染页面。忽略文档类型声明。对于 IE8，始终保持以 IE8 标
准模式渲染页面。对于 IE9，则以 IE9 标准模式渲染页面。
 EmulateIE9：如果有文档类型声明，则以 IE9 标准模式渲染页面，否则将文档模式设置为 IE5。
 EmulateIE8：如果有文档类型声明，则以 IE8 标准模式渲染页面，否则将文档模式设置为 IE5。
 EmulateIE7：如果有文档类型声明，则以 IE7 标准模式渲染页面，否则将文档模式设置为 IE5。
 9：强制以 IE9 标准模式渲染页面，忽略文档类型声明。
 8：强制以 IE8 标准模式渲染页面，忽略文档类型声明。
 7：强制以 IE7 标准模式渲染页面，忽略文档类型声明。
 5：强制将文档模式设置为 IE5，忽略文档类型声明

> 通过 document.documentMode 属性可以知道给定页面使用的是什么文档模式。这个属性是 IE8
> 中新增的，它会返回使用的文档模式的版本号（在 IE9 中，可能返回的版本号为 5、 7、 8、 9）：

``` json
var mode = document.documentMode;
```

### children属性

> 由于 IE9 之前的版本与其他浏览器在处理文本节点中的空白符时有差异

> 这个属性是 HTMLCollection 的实例，只包含元素中同样还是元素的子节点。除此之外，children 属性与 childNodes 没有什么区别，即在元素只包含元素子节点时，这两个属性的值相同。

###contains()方法 

> 在实际开发中，经常需要知道某个节点是不是另一个节点的后代。  contains()方法，以便不通过在 DOM 文档树中查找即可获得这个信息。

>  contains()方法的应该是祖先节点，也就是搜索开始的节点，这个方法接收一个参数，即要检测的后代节点。如果被检测的节点是后代节点，该方法返回 true；否则，返回 false

* DOM Level 3 compareDocumentPosition()

> 这个方法用于确定两个节点间的关系，返回一个表示该关系的位掩码（bitmask）
>
> 1 无关（给定的节点不在当前文档中）
> 2 居前（给定的节点在DOM树中位于参考节点之前）
> 4 居后（给定的节点在DOM树中位于参考节点之后）
> 8 包含（给定的节点是参考节点的祖先）
> 16 被包含（给定的节点是参考节点的后代）

###插入文本

* innerText 属性

  *  读取值时，它会按照由浅入深的顺序，将子文档树中的所有文本拼接起来返回
  *  写入值时，结果会删除元素的所有子节点，插入包含相应文本值的文本节点

  > `div.innerText = div.innerText`
  > 执行这行代码后，就用原来的文本内容替换了容器元素中的所有内容（包括子节点，因而也就去掉了 HTML 标签）。

  >  Firefox 虽然不支持innerText，但支持作用类似的 textContent 属性

  类型检查函数：

   ```jsx
  function getInnerText(element){
  	return (typeof element.textContent == "string") ?
  	element.textContent : element.innerText;
  }
  function setInnerText(element, text){
  	if (typeof element.textContent == "string"){
  		element.textContent = text;
  	} else {
  		element.innerText = text;
  	}
  }
   ```


* outerText 属性

  > 除了作用范围扩大到了包含调用它的节点之外， outerText 与 innerText 基本上没有多大区别

###滚动

* scrollIntoViewIfNeeded(alignCenter)：只在当前元素在视口中不可见的情况下，才滚

动浏览器窗口或容器元素，最终让它可见。如果当前元素在视口中可见，这个方法什么也不做。
如果将可选的 alignCenter 参数设置为 true，则表示尽量将元素显示在视口中部（垂直方向）。
Safari 和 Chrome 实现了这个方法。

* scrollByLines(lineCount)：将元素的内容滚动指定的行高， lineCount 值可以是正值，

也可以是负值。 Safari 和 Chrome 实现了这个方法。

* scrollByPages(pageCount)：将元素的内容滚动指定的页面高度，具体高度由元素的高度决

定。 Safari 和 Chrome 实现了这个方法。

