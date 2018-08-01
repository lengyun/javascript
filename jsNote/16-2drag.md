## 原生拖放

### 拖放事件

通过拖放事件，可以控制拖放相关的各个方面。其中最关键的地方在于确定哪里发生了拖放事件，有些事件是在被拖动的元素上触发的，而有些事件是在防治目标上触发的。拖动某元素时，将依次触发下列事件：

1. dragstart
2. drag
3. dragend

按下鼠标键并开始移动鼠标时，会在被拖放的元素上触发dragstart事件。拖动开始时，可以通过ondragstart事件处理程序来运行JavaScript代码。

触发dragstart事件后，随机会触发drag事件，而且在元素被拖动期间会持续触发该事件。

当拖动停止时（无论是把元素放到了有效的放置目标，还是放到了无效的放置目标上），会触发dragend事件。

当某个元素被拖动到一个有效的放置目标上时，下列事件会依次发生：

1. dragenter
2. dragover
3. dragleave或 drop

只要有元素被拖动到放置目标上，就会触发dragenter事件（类似mouseover事件）。紧随其后的是dragover事件，而且在被拖动的元素还在放置目标的范围内移动时，就会持续触发该事件。如果元素被拖出了放置目标，dragover事件不再发生，但会触发dragleave事件（类似mouseout事件）。如果元素被放到了放置目标中，则会触发drop事件而不是dragleave事件。上述三个事件的目标都是作为放置目标的元素。

### 自定义放置目标

在拖动元素经过某些无效方式目标时，可以看到一种特殊的光标（圆环中有一条反斜杠），表示不能放置。虽然所有元素都支持放置目标事件，但这些元素默认是不允许放置的。如果拖动元素经过不允许放置的元素，无论用户如何操作，都不会发生drop事件。不过，你可以把任何元素变成有效的放置目标，方法是重写dragenter 和 dragover事件的默认行为。

```js
var droptarget = document.getElementById('droptarget');
EventUtil.addHandler(droptarget,'dragenter',function(event){
    EventUtile.preventDefault(event);
})
EventUtil.addHandler(droptarget,'dragover',function(event){
    EventUtil.preventDefault(event);
})
```

在 Firefox 3.5+中，放置事件的默认行为是打开被放到放置目标上的 URL。换句话说，如果是把图像拖放到放置目标上，页面就会转向图像文件；而如果是把文本拖放到放置目标上，则会导致无效 URL错误。因此，为了让 Firefox 支持正常的拖放，还要取消 drop 事件的默认行为，阻止它打开 URL：

```js
EventUtil.addHandler(droptarget, "drop", function(event){
	EventUtil.preventDefault(event);
});
```

### dataTransfer对象

只有简单的拖放而没有数据变化是没有什么用的。为了在拖放操作时实现数据交换， IE 5 引入了**dataTransfer 对象**，它是事件对象的一个属性，用于从被拖动元素向放置目标传递字符串格式的数据。因为它是事件对象的属性，所以只能在拖放事件的事件处理程序中访问 dataTransfer 对象。在事件处理程序中，可以使用这个对象的属性和方法来完善拖放功能。

dataTransfer 对象有两个主要方法： getData()和 setData()。不难想象， getData()可以取
得由 setData()保存的值。 setData()方法的第一个参数，也是 getData()方法唯一的一个参数，是
一个字符串，表示保存的数据类型，取值为"text"或"URL"

```js
//设置和接收文本数据
event.dataTransfer.setData("text", "some text");
var text = event.dataTransfer.getData("text");
//设置和接收 URL
event.dataTransfer.setData("URL", "http://www.wrox.com/");
var url = event.dataTransfer.getData("URL");
```

IE只定义了"text"和"URL"两种有效的数据类型，而 HTML5则对此加以扩展，允许指定各种 MIME类型。考虑到向后兼容， HTML5 也支持"text"和"URL"，但这两种类型会被映射为"text/plain"和
"text/uri-list"。

实际上， dataTransfer 对象可以为每种 MIME 类型都保存一个值。换句话说，同时在这个对象中保存一段文本和一个 URL 不会有任何问题。不过，保存在 dataTransfer 对象中的数据只能在 drop事件处理程序中读取。如果在 ondrop 处理程序中没有读到数据，那就是 dataTransfer 对象已经被销毁，数据也丢失了。

在拖动文本框中的文本时，浏览器会调用 setData()方法，将拖动的文本以"text"格式保存在
dataTransfer 对象中。类似地，在拖放链接或图像时，会调用 setData()方法并保存 URL。然后，
在这些元素被拖放到放置目标时，就可以通过 getData()读到这些数据。当然，作为开发人员，你也可以在 dragstart 事件处理程序中调用 setData()，手工保存自己要传输的数据，以便将来使用。

将数据保存为文本和保存为 URL 是有区别的。如果将数据保存为文本格式，那么数据不会得到任
何特殊处理。而如果将数据保存为 URL，浏览器会将其当成网页中的链接。换句话说，如果你把它放置到另一个浏览器窗口中，浏览器就会打开该 URL。

```js
var dataTransfer = event.dataTransfer;
//读取 URL
var url = dataTransfer.getData("url") ||dataTransfer.getData("text/uri-list");
//读取文本
var text = dataTransfer.getData("Text");
```

> Firefox 在 其 第 5 个 版 本 之 前 不 能 正 确 地 将 "url" 和 "text" 映 射 为 "text/uri-list" 和"text/plain"。但是却能把"Text"（T 大写）映射为"text/plain"。为了更好地在跨浏览器的情况下从 dataTransfer 对象取得数据，最好在取得 URL 数据时检测两个值，而在取得文本数据时使用"Text"。

> 注意，一定要把短数据类型放在前面，因为 IE 10 及之前的版本仍然不支持扩展的 MIME 类型名，而它们在遇到无法识别的数据类型时，会抛出错误。

### dropEffect与effectAllowed

利用dataTransfer对象，可不光是能够传输数据，还能通过它来确定被拖动的元素以及作为放置目标的元素能够接收什么操作。为此，需要访问dataTransfer对象的两个属性：dropEffect和effectAllowed。

通过dropEffect属性可以知道被拖动的元素能够执行那种放置行为。

* “none”：不能把拖放的元素放在这里。这是除文本框之外的所有元素的默认值。
* “move”：应该把拖动的元素移动到放置目标。
* “copy”：应该把拖动的元素复制到放置目标。
* “link”：表示放置目标会打开拖动的元素（但拖动的元素必须是一个链接，有URL）

要使用dropEffect属性，必须在ondragenter事件处理程序中针对放置目标来设置它。

dropEffect属性只有搭配effectAllowed属性才有用。effectAllowed属性表示允许拖动元素的那种DropEffect，effectAllowd属性可能的值如下：

* uninitialized: 没有给被拖动的元素设置任何放置行为。
* none： 被拖动的元素不能有任何行为。
* copy：只允许值为“copy”的dropEffect。
* link：只允许值为“link”的dropEffect。
* move：只允许值为“move”的dropEffect。
* copyLink：允许值为"copy"和"link"的 dropEffect。
* copyMove：允许值为"copy"和"move"的 dropEffect。
* linkMove：允许值为"link"和"move"的 dropEffect。
* all：允许任意 dropEffect。

必须在 ondragstart 事件处理程序中设置 effectAllowed 属性

###可拖动

默认情况下，图像、链接和文字是可以拖动的，也就是说，不用额外编写代码，用户就可以拖动他们。文本只有在被选中的情况下才能拖动，而图像和链接在任何时候都可以拖动。

让其他元素可以拖动也是可能的。HTML5为所有HTML元素规定了一个draggable属性，表示元素是否可以拖动。图像和链接的droggable属性自动被设置成了true，而其他元素这个属性的默认值都是false。要想让其他元素可拖动，或者让图像和链接不能拖动，都可以设置设置这个属性。

```html
<!-- 让这个图像不可以拖动 -->
<img src="smile.gif" draggable="false" alt="Smiley face">
<!-- 让这个元素可以拖动 -->
<div draggable="true">...</div>
```

### 其他成员

HTML5 规范规定 dataTransfer 对象还应该包含下列方法和属性。

* addElement(element)：为拖动操作添加一个元素。添加这个元素只影响数据（即增加作为拖动源而响应回调的对象），不会影响拖动操作时页面元素的外观。在写作本书时，只有 Firefox 3.5+实现了这个方法。
*  clearData(format)：清除以特定格式保存的数据。实现这个方法的浏览器有 IE、Fireforx 3.5+、Chrome 和 Safari 4+。
* setDragImage(element, x, y)：指定一幅图像，当拖动发生时，显示在光标下方。这个方法接收的三个参数分别是要显示的 HTML 元素和光标在图像中的 x、 y 坐标。其中， HTML 元素可以是一幅图像，也可以是其他元素。是图像则显示图像，是其他元素则显示渲染后的元素。实现这个方法的浏览器有 Firefox 3.5+、 Safari 4+和 Chrome。
*  types：当前保存的数据类型。这是一个类似数组的集合，以"text"这样的字符串形式保存着
  数据类型。实现这个属性的浏览器有 IE10+、 Firefox 3.5+和 Chrome。