## 表单基础知识

在HTML中，表单是由<form>元素来表示的，而在JavaScript中，表单对应的则是HTMFormElement类型。HTMLFormElement继承了HTMLElement，因而与其他HTML元素具有相同的默认属性。HTMFormElement自己独有的属性和方法如下：

* acceptCharset：服务器能够处理的字符集；等价于 HTML 中的 accept-charset 特性。
* action：接受请求的 URL；等价于 HTML 中的 action 特性。
* elements：表单中所有控件的集合（HTMLCollection）。
* enctype：请求的编码类型；等价于 HTML 中的 enctype 特性。
* length：表单中控件的数量。
* method：要发送的 HTTP 请求类型，通常是"get"或"post"；等价于 HTML 的 method 特性。
* name：表单的名称；等价于 HTML 的 name 特性。
* reset()：将所有表单域重置为默认值。
* submit()：提交表单。
* target：用于发送请求和接收响应的窗口名称；等价于 HTML 的 target 特性。

获得<form>元素引用的方式：

* 为其添加id属性，使用getElementByid()方法

* 通过 document.forms 取得页面中所有的表单。在这个集合中通过数值索引或name值来取得特定的表单

  ```js
  var firstForm = document.forms[0]; //取得页面中的第一个表单
  var myForm = document.forms["form2"]; //取得页面中名称为"form2"的表单
  ```

### 提交表单

1.用户单击提交按钮或图像按钮时，就会提交表单。使用<input>或<button>都可以定义提交按钮，只要将其 type 特性的值设置为"submit"即可，而图像按钮则是通过将<input>的 type 特性值设置为"image"来定义的。

```html
<!-- 通用提交按钮 -->
<input type="submit" value="Submit Form">
<!-- 自定义提交按钮 -->
<button type="submit">Submit Form</button>
<!-- 图像按钮 -->
<input type="image" src="graphic.gif">
```

只要表单中存在上面列出的任何一种按钮，那么在相应表单控件拥有焦点的情况下，按回车键就可以提交该表单。（textarea 是一个例外，在文本区中回车会换行。）如果表单里没有提交按钮，按回车键不会提交表单。以这种方式提交表单时，浏览器会在将请求发送给服务器之前触发 submit 事件。这样，我们就有机会验证表单数据，并据以决定是否允许表单提交。阻止这个事件的默认行为就可以取消表单提交。

2.在 JavaScript 中，以编程方式调用 submit()方法也可以提交表单。

```js
var form = document.getElementById("myForm");
//提交表单
form.submit();
```

在以调用 submit()方法的形式提交表单时，不会触发 submit 事件，因此要记得在调用此方法之前先验证表单数据。

3.重复提交表单

在第一次提交表单后就禁用提交按钮，或者利用 onsubmit 事件处理程序取消后续的
表单提交操作。

### 重置表单

 type 特性值为"reset"的<input>或<button>都可以创建重置按钮。在重置表单时，所有表单字段都会恢复到页面刚加载完毕时的初始值。如果某个字段的初始值为空，就会恢复为空；而带有默认值的字段，也会恢复为默认值。

用户单击重置按钮重置表单时，会触发 reset 事件。利用这个机会，我们可以在必要时取消重置操作。

与提交表单一样，也可以通过 JavaScript 来重置表单，如下面的例子所示。

```js
var form = document.getElementById("myForm");
//重置表单
form.reset();
```

与调用 submit()方法不同，调用 reset()方法会像单击重置按钮一样触发 reset 事件。

### 表单字段

每个表单都有elements属性，该属性是表单中所有表单元素（字段）的集合。这个 elements 集合是一个有序列表，其中包含着表单中的所有字段，例如<input>、 <textarea>、 <button>和<fieldset>。每个表单字段在 elements 集合中的顺序，与它们出现在标记中的顺序相同，可以按照位置和 name 特性来访问它们。

```js
var form = document.getElementById("form1");
//取得表单中的第一个字段
var field1 = form.elements[0];
//取得名为"textbox1"的字段
var field2 = form.elements["textbox1"];
//取得表单中包含的字段的数量
var fieldCount = form.elements.length;
```

如果有多个表单控件都在使用一个 name（如单选按钮），那么就会返回以该 name 命名的一个NodeList。elements["color"]时，就会返回一个 NodeList，其中包含这 3 个元素；不过，如果访问elements[0]，则只会返回第一个元素

1. 共有的表单字段属性

   * disabled：布尔值，表示当前字段是否被禁用。


   * form：指向当前字段所属表单的指针；只读。


   * name：当前字段的名称。

   * readOnly：布尔值，表示当前字段是否只读。

   * tabIndex：表示当前字段的切换（tab）序号。

   * type：当前字段的类型，如"checkbox"、 "radio"，等等。

   * value：当前字段将被提交给服务器的值。对文件字段来说，这个属性是只读的，包含着文件在计算机中的路径。

     > 除了 form 属性之外，可以通过 JavaScript 动态修改其他任何属性。

     > 不能通过 onclick 事件处理程序来实现这个功能，原因是不同浏览器之间存在“时差”：有的浏览器会在触发表单的 submit 事件之前触发 click 事件，而有的浏览器则相反。对于先触发 click 事件的浏览器，意味着会在提交发生之前禁用按钮，结果永远都不会提交表单。因此，最好是通过 submit 事件来禁用提交按钮。不过，这种方式不适合表单中不包含提交按钮的情况；如前所述，只有在包含提交按钮的情况下，才有可能触发表单的 submit事件

2. 共有的表单字段方法

   * focus()方法用于将浏览器的焦点设置到表单字段，即激活表单字段，使其可以响应键盘事件。

     HTML5 为表单字段新增了一个 **autofocus** 属性。在支持这个属性的浏览器中，只要设置这个属性，不用 JavaScript 就能自动把焦点移动到相应字段。

     > 在默认情况下，只有表单字段可以获得焦点。对于其他元素而言，如果先将其tabIndex 属性设置为1，然后再调用 focus()方法，也可以让这些元素获得焦点。

   *  blur()方法，它的作用是从元素中移走焦点。在调用 blur()方法时，并不会把焦点转移到某个特定的元素上；仅仅是将焦点从调用这个方法的元素上面移走而已。

3. 共有的表单字段事件

   除了支持鼠标、键盘、更改和 HTML 事件之外，所有表单字段都支持下列 3 个事件。

   * blur：当前字段失去焦点时触发。
   * change：对于<input>和<textarea>元素，在它们失去焦点且 value 值改变时触发；对于<select>元素，在其选项改变时触发。


   * focus：当前字段获得焦点时触发。

   可以使用 focus 和 blur 事件来以某种方式改变用户界面，要么是向用户给出视觉提示，要么是向界面中添加额外的功能（例如，为文本框显示一个下拉选项菜单）。而 change 事件则经常用于验证用户在字段中输入的数据。

