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
    if (!/\d/.test(String.fromCharCode(charCode)) && charCode > 9 && !event.ctrlKey){
    	EventUtil.preventDefault(event);
    }
});
```

####2.操作剪切板

* beforecopy：在发生复制操作前触发。
* copy：在发生复制操作时触发。
* beforecut：在发生剪切操作前触发。
* cut：在发生剪切操作时触发。
* beforepaste：在发生粘贴操作前触发。
* paste：在发生粘贴操作时触发。

beforecopy、 beforecut 和 beforepaste 事件 chrome,safari,firefox 只会在显示针对文本框的上下文菜单的情况下触发。IE则会在触发copy，cut和paste事件之前先行触发

 copy、 cut 和 paste 事件，只要是在上下文菜单中选择了相应选项，或者使用了相应的键盘组合键，所有浏览器都会触发它们。

只有取消 copy、 cut 和 paste 事件，才能阻止相应操作发生。

访问剪切板中的数据：***clipboardData*** 对象：

 IE 中，这个对象是 window 对象的属性；而在 Firefox 4+、 Safari 和 Chrome 中，这个对象是相应 event 对象的属性。但是，在 Firefox、Safari 和 Chorme 中，只有在处理剪贴板事件期间 clipboardData 对象才有效，这是为了防止对剪贴板的未授权访问；在 IE 中，则可以随时访问 clipboardData 对象。为了确保跨浏览器兼容性，最好只在发生剪贴板事件期间使用这个对象。

 clipboardData 对象有三个方法：***getData()、setData()和 clearData()***。

getData()用于从剪贴板中取得数据，它接受一个参数，即要取得的数据的格式。在 IE 中，有两种数据格式： "text"和"URL"。在 Firefox、 Safari 和 Chrome 中，这个参数是一种 MIME 类型；不过，可以用"text"代表"text/plain"。

setData()方法的第一个参数也是数据类型，第二个参数是要放在剪贴板中的文本。对于
第一个参数， IE 照样支持"text"和"URL"，而 Safari 和 Chrome 仍然只支持 MIME 类型。但是，与getData()方法不同的是， Safari 和 Chrome 的 setData()方法不能识别"text"类型。这两个浏览器在成功将文本放到剪贴板中后，都会返回 true；否则，返回 false。

```js
var EventUtil = {
//省略的代码
    getClipboardText: function(event){
        var clipboardData = (event.clipboardData || window.clipboardData);
        return clipboardData.getData("text");
    },
    //省略的代码
    setClipboardText: function(event, value){
        if (event.clipboardData){
            return event.clipboardData.setData("text/plain", value);
        } else if (window.clipboardData){
            return window.clipboardData.setData("text", value);
        }
    },
//省略的代码
};
```

在需要确保粘贴到文本框中的文本中包含某些字符，或者符合某种格式要求时，能够访问剪贴板是非常有用的。

```js
EventUtil.addHandler(textbox, "paste", function(event){
    event = EventUtil.getEvent(event);
    var text = EventUtil.getClipboardText(event);
    if (!/^\d*$/.test(text)){
    	EventUtil.preventDefault(event);
    }
});
```

### 自动切换焦点

使用 JavaScript 可以从多个方面增强表单字段的易用性。其中，最常见的一种方式就是在用户填写完当前字段时，自动将焦点切换到下一个字段。

```js
(function(){
    function tabForward(event){
        event = EventUtil.getEvent(event);
        var target = EventUtil.getTarget(event);
        if (target.value.length == target.maxLength){
            var form = target.form;
            for (var i=0, len=form.elements.length; i < len; i++) {
                if (form.elements[i] == target) {
                    if (form.elements[i+1]){
                    	form.elements[i+1].focus();
                    }
                    return;
                }
            }
        }
    }
    var textbox1 = document.getElementById("txtTel1");
    var textbox2 = document.getElementById("txtTel2");
    var textbox3 = document.getElementById("txtTel3");
    EventUtil.addHandler(textbox1, "keyup", tabForward);
    EventUtil.addHandler(textbox2, "keyup", tabForward);
    EventUtil.addHandler(textbox3, "keyup", tabForward);
})();
```

### HTML5 约束验证API

为了在将表单提交到服务器之前验证数据，HTML5 新增了一些功能。有了这些功能，即便 JavaScript被禁用或者由于种种原因未能加载，也可以确保基本的验证。换句话说，浏览器自己会根据标记中的规则执行验证，然后自己显示适当的错误消息（完全不用 JavaScript 插手）。

#### 1.必填字段

第一种情况是在表单字段中指定了 required 属性，如下面的例子所示：
```<input type="text" name="username" required>```

任何标注有 required 的字段，在提交表单时都不能空着。这个属性适用于<input>、 <textarea>和<select>字段（Opera 11 及之前版本还不支持<select>的 required 属性）。在 JavaScript 中，通过对应的 required 属性，可以检查某个表单字段是否为必填字段。

```js
var isUsernameRequired = document.forms[0].elements["username"].required;
//测试浏览器是否支持 required 属性。
var isRequiredSupported = "required" in document.createElement("input");
```

对于空着的必填字段，不同浏览器有不同的处理方式。 Firefox 4 和 Opera 11 会阻止表单提交并在相应字段下方弹出帮助框，而 Safari（5 之前）和 Chrome（9 之前）则什么也不做，而且也不阻止表单提交

#### 2.其他输入类型

HTML5 为<input>元素的 type 属性又增加了几个值。这些新的类型不仅能反映数据类型的信息，而且还能提供一些默认的验证功能。其中， "email"和"url"是两个得到支持最多的类型，各浏览器也都为它们增加了定制的验证机制。

```html
<input type="email" name ="email">
<input type="url" name="homepage">
```

不支持它们的旧版本浏览器会自动将未知的值设置为"text"，而支持的浏览器则会返回正确的值。

#### 3.数值范围

除了"email"和"url"， HTML5 还定义了另外几个输入元素。这几个元素都要求填写某种基于数字的值： "number"、 "range"、 "datetime"、 "datetime-local"、 "date"、 "month"、 "week"，还有"time"。

对所有这些数值类型的输入元素，可以指定 min 属性（最小的可能值）、 max 属性（最大的可能值）和 step 属性（从 min 到 max 的两个刻度间的差值）。例如，想让用户只能输入 0 到 100 的值，而且这个值必须是 5 的倍数，可以这样写代码：

```html
<input type="number" min="0" max="100" step="5" name="count">
```

 stepUp()和 stepDown()，接收一个可选的参数：要在当前值基础上加上或减去的数值。（默认是加或减 1。）暂未被浏览器支持

```js
input.stepUp(); //加 1
input.stepUp(5); //加 5
input.stepDown(); //减 1
input.stepDown(10); //减 10
```

#### 4.输入模式

HTML5 为文本字段新增了 pattern 属性。这个属性的值是一个正则表达式，用于匹配文本框中的值。例如，如果只想允许在文本字段中输入数值，可以像下面的代码一样应用约束：

```html
<input type="text" pattern="\d+" name="count">
```

注意，模式的开头和末尾不用加^和$符号（假定已经有了）。这两个符号表示输入的值必须从头到尾都与模式匹配。

#### 5.检测有效性

使用 checkValidity()方法可以检测表单中的某个字段是否有效。所有表单字段都有个方法，如果字段的值有效，这个方法返回 true，否则返回 false。字段的值是否有效的判断依据是本节前面介绍过的那些约束。换句话说，必填字段中如果没有值就是无效的，而字段中的值与 pattern 属性不匹配也是无效的。

```js
if (document.forms[0].elements[0].checkValidity()){
//字段有效，继续
} else {
//字段无效
}
```

要检测整个表单是否有效，可以在表单自身调用 checkValidity()方法。如果所有表单字段都有效，这个方法返回 true；即使有一个字段无效，这个方法也会返回 false。

```js
if(document.forms[0].checkValidity()){
//表单有效，继续
} else {
//表单无效
}
```

 validity 属性则会告诉你为什么字段有效或无效。这个对象中包含一系列属性，每个属性会返回一个布尔值。

* customError ：如果设置了 setCustomValidity()，则为 true，否则返回 false。
* patternMismatch：如果值与指定的 pattern 属性不匹配，返回 true。
* rangeOverflow：如果值比 max 值大，返回 true。
* rangeUnderflow：如果值比 min 值小，返回 true。
* stepMisMatch：如果 min 和 max 之间的步长值不合理，返回 true。
* tooLong：如果值的长度超过了 maxlength 属性指定的长度，返回 true。有的浏览器（如 Firefox 4）会自动约束字符数量，因此这个值可能永远都返回 false。


* typeMismatch：如果值不是"mail"或"url"要求的格式，返回 true。
* valid：如果这里的其他属性都是 false，返回 true。 checkValidity()也要求相同的值。
* valueMissing：如果标注为 required 的字段中没有值，返回 true。

#### 6.禁用验证

通过设置 novalidate 属性，可以告诉表单不进行验证。

```html
<form method="post" action="signup.php" novalidate>
<!--这里插入表单元素-->
</form>
```

在 JavaScript 中使用 noValidate 属性可以取得或设置这个值，如果这个属性存在，值为 true，
如果不存在，值为 false。
```document.forms[0].noValidate = true; //禁用验证```

如果一个表单中有多个提交按钮，为了指定点击某个提交按钮不必验证表单，可以在相应的按钮上添加 formnovalidate 属性。

```html
<form method="post" action="foo.php">
    <!--这里插入表单元素-->
    <input type="submit" value="Regular Submit">
    <input type="submit" formnovalidate name="btnNoValidate"
    value="Non-validating Submit">
</form>
```

```js
//使用 JavaScript 也可以设置这个属性。
//禁用验证
document.forms[0].elements["btnNoValidate"].formNoValidate = true;
```













