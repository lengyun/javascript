## 文本框脚本

在 HTML 中，有两种方式来表现文本框：一种是使用<input>元素的单行文本框，另一种是使用<textarea>的多行文本框。

要表现文本框，必须将<input>元素的 type 特性设置为"text"。而通过设置 size 特性，可以指定文本框中能够显示的字符数。通过 value 特性，可以设置文本框的初始值，而 maxlength 特性则用于指定文本框可以接受的最大字符数。如果要创建一个文本框，让它能够显示 25 个字符，但输入不能超过 50 个字符，可以使用以下代码：

```html
<input type="text" size="25" maxlength="50" value="initial value">
```

<textarea>元素则始终会呈现为一个多行文本框。要指定文本框的大小，可以使用 rows
和 cols 特性。其中， rows 特性指定的是文本框的字符行数，而 cols 特性指定的是文本框的字符列数（类似于<inpu>元素的 size 特性）。与<input>元素不同， <textarea>的初始值必须要放在<textarea>和</textarea>之间，如下面的例子所示。

```html
<textarea rows="25" cols="5">initial value</textarea>
```

不能在 HTML 中给<textarea>指定最大字符数。

通过value属性读取和设置文本框的值，如下面的例子所示：

```js
var textbox = document.forms[0].elements["textbox1"];
alert(textbox.value);
textbox.value = "Some new value";
```

> 不要使用 setAttribute()设置<input>元素的 value 特性，

### 选择文本

select()方法，用于选择文本框中的所有文本。在调用 select()方法时，大多数浏览器（Opera 除外）都会将焦点设置到文本框中。这个方法不接受数，可以在任何时候被调用。

在文本框获得焦点时选择其所有文本:

```js
EventUtil.addHandler(textbox, "focus", function(event){
	event = EventUtil.getEvent(event);
	var target = EventUtil.getTarget(event);
	target.select();
});
```

1. 选择（select）事件

   与 select()方法对应的，是一个 select 事件。

   * 在选择了文本框中的文本时，（IE9+释放鼠标触发，IE8-选择即触发）就会触发 select事件。
   * 在调用 select()方法时也会触发 select 事件。

2. 取得选择的文本

   HTML5给select 事件添加两个属性： selectionStart 和 selectionEnd。这两个属性中保存的是基于 0 的数值，表示所选择文本的范围（即文本选区开头和结尾的偏移量）。

   IE8 及更早的版本中有一个 document.selection 对象，其中保存着用户在整个文档范围内选择的文本信息；

   ```js
   function getSelectedText(textbox){
       if (typeof textbox.selectionStart == "number"){
           return textbox.value.substring(textbox.selectionStart,
           textbox.selectionEnd);
       } else if (document.selection){
       	return document.selection.createRange().text;
       }
   }
   ```

3. 选择部分文本

   HTML5 也 为 选 择 文 本 框 中 的 部 分 文 本 提 供 了 解 决 方 案 ， 即 最 早 由 Firefox 引 入 的setSelectionRange()方法。现在除select()方法之外，所有文本框都有一个setSelectionRange()方法。这个方法接收两个参数：要选择的第一个字符的索引和要选择的最后一个字符之后的字符的索引

   ```js
   textbox.value = "Hello world!"
   //选择所有文本
   textbox.setSelectionRange(0, textbox.value.length); //"Hello world!"
   //选择前 3 个字符
   textbox.setSelectionRange(0, 3); //"Hel"
   //选择第 4 到第 6 个字符
   textbox.setSelectionRange(4, 7); //"o w"
   ```

   IE8 及更早版本支持使用范围（第 12 章讨论过）选择部分文本。

   （待选择学习）

### 过滤输入

####1.屏蔽字符

响应向文本框中插入字符操作的是 keypress 事件。因此，可以通过阻止这个事件的默
认行为来屏蔽此类字符。

想屏蔽特定的字符，则需要检测 keypress 事件对应的字符编码，然后再决定如何响应

```js
EventUtil.addHandler(textbox, "keypress", function(event){
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    var charCode = EventUtil.getCharCode(event);
    if (!/\d/.test(String.fromCharCode(charCode))){
    	EventUtil.preventDefault(event);
    }
});
```

####2.操作剪切板

### 自动切换焦点

### HTML5 约束验证API





