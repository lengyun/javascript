## 富文本编辑

富文本编辑，又称为 WYSIWYG（What You See Is What You Get，所见即所得）。

这一技术的本质，就是在页面中嵌入一个包含空 HTML 页面的 iframe。通过设置 designMode 属性，这个空白的 HTML 页面可以被编辑，而编辑对象则是该页面<body>元素的 HTML 代码。 designMode 属性有两个可能的值： "off"（默认值）和"on"。在设置为"on"时，整个文档都会变得可以编辑（显示插入符号），然后就可以像使用字处理软件一样，通过键盘将文本内容加粗、变成斜体等等。

只有在页面完全加载之后才能设置这个属性。因此，在包含页面中，需要使用 onload 事件
处理程序来在恰当的时刻设置 designMode，如下面的例子所示：

```html
<iframe name="richedit" style="height:100px;width:100px;" src="blank.htm"></iframe>
<script type="text/javascript">
	EventUtil.addHandler(window, "load", function(){
		frames["richedit"].document.designMode = "on";
	});
</script>
```

### 使用contenteditable属性

可以把 contenteditable 属性应用给页面中的任何元素，然后用户立即就可以编辑该元素。
这种方法之所以受到欢迎，是因为它不需要 iframe、空白页和 JavaScript，只要为元素设置
contenteditable 属性即可。这样，元素中包含的任何文本内容就都可以编辑了，就好像这个元素变成了<textarea>元素一样。

```html
<div class="editable" id="richedit" contenteditable></div>
```

通过在这个元素上设置 contenteditable 属性，也能打开或关闭编辑模式。

```js
var div = document.getElementById("richedit");
div.contentEditable = "true";
```

contenteditable 属性有三个可能的值： "true"表示打开、 "false"表示关闭， "inherit"表示
从父元素那里继承（因为可以在 contenteditable 元素中创建或删除元素）。支持 contenteditable属性的元素有 IE、 Firefox、 Chrome、 Safari 和 Opera。

###操作富文本

与富文本编辑器交互的主要方式，就是使用document.execCommand()。这个方法可以对文档执行预定义的命令，而且可以应用大多数格式。可以为 document.execCommand()方法传递 3 个参数：**要执行的命令名称**、**表示浏览器是否应该为当前命令提供用户界面的一个布尔值**和**执行命令必须的一个值**（如果不需要值，则传递 null）。为了确保跨浏览器的兼容性，第二个参数应该始终设置为 false，因为 Firefox 会在该参数为 true 时抛出错误。

可以在任何时候使用这些命令来修改富文本区域的外观，如下面的例子所示:

```js
//转换粗体文本
frames["richedit"].document.execCommand("bold", false, null);
//转换斜体文本
frames["richedit"].document.execCommand("italic", false, null);
//创建指向 www.wrox.com 的链接
frames["richedit"].document.execCommand("createlink", false,
"http://www.wrox.com");
//格式化为 1 级标题
frames["richedit"].document.execCommand("formatblock", false, "<h1>");
```

同样的方法也适用于页面中 contenteditable 属性为"true"的区块，只要把对框架的引用替换
成当前窗口的 document 对象即可。

```js
//转换粗体文本
document.execCommand("bold", false, null);
//转换斜体文本
document.execCommand("italic", false, null);
//创建指向 www.wrox.com 的链接
document.execCommand("createlink", false, "http://www.wrox.com");
//格式化为 1 级标题
document.execCommand("formatblock", false, "<h1>");
```

除了命令之外，还有一些与命令相关的方法。第一个方法就是 queryCommandEnabled()，可以用它来检测是否可以针对当前选择的文本，或者当前插入字符所在位置执行某个命令。这个方法接收一个参数，即要检测的命令。如果当前编辑区域允许执行传入的命令，这个方法返回 true，否则返回 false。

```js
var result = frames["richedit"].document.queryCommandEnabled("bold");
```

最后一个方法是 queryCommandValue()，用于取得执行命令时传入的值（即前面例子中传给
document.execCommand()的第三个参数）。例如，在对一段文本应用"fontsize"命令时如果传入了7，那么下面的代码就会返回"7"：

```js
var fontSize = frames["richedit"].document.queryCommandValue("fontsize");
```

###富文本选区

在富文本编辑器中，使用框架（iframe）的 getSelection()方法，可以确定实际选择的文本。
这个方法是 window 对象和 document 对象的属性，调用它会返回一个表示当前选择文本的 Selection对象。每个 Selection 对象都有下列属性。

 anchorNode：选区起点所在的节点。
 anchorOffset：在到达选区起点位置之前跳过的 anchorNode 中的字符数量。
 focusNode：选区终点所在的节点。
 focusOffset： focusNode 中包含在选区之内的字符数量。
 isCollapsed：布尔值，表示选区的起点和终点是否重合。
 rangeCount：选区中包含的 DOM 范围的数量。

 addRange(range)：将指定的 DOM 范围添加到选区中。
 collapse(node, offset)：将选区折叠到指定节点中的相应的文本偏移位置。
 collapseToEnd()：将选区折叠到终点位置。
 collapseToStart()：将选区折叠到起点位置。
 containsNode(node)：确定指定的节点是否包含在选区中。
 deleteFromDocument()：从文档中删除选区中的文本，与document.execCommand("delete",
false, null)命令的结果相同。
 extend(node, offset)：通过将 focusNode 和 focusOffset 移动到指定的值来扩展选区。
 getRangeAt(index)：返回索引对应的选区中的 DOM 范围。
 removeAllRanges()：从选区中移除所有 DOM 范围。实际上，这样会移除选区，因为选区中
至少要有一个范围。
 reomveRange(range)：从选区中移除指定的 DOM 范围。
 selectAllChildren(node)：清除选区并选择指定节点的所有子节点。
 toString()：返回选区所包含的文本内容

###表单与富文本

由于富文本编辑是使用 iframe 而非表单控件实现的，因此从技术上说，富文本编辑器并不属于表单。换句话说，富文本编辑器中的 HTML 不会被自动提交给服务器，而需要我们手工来提取并提交HTML。为此，通常可以添加一个隐藏的表单字段，让它的值等于从 iframe 中提取出的 HTML。具体来说，就是在提交表单之前，从 iframe 中提取出 HTML，并将其插入到隐藏的字段中。

```js
EventUtil.addHandler(form, "submit", function(event){
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    target.elements["comments"].value = frames["richedit"].document.body.innerHTML;
});
```

