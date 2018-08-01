## 选择框脚本

选择框是通过<select>和<option>元素创建的。为了方便与这个控件交互，除了所有表单字段共有的属性和方法外， HTMLSelectElement 类型还提供了下列属性和方法。

* add(newOption, relOption)：向控件中插入新<option>元素，其位置在相关项（relOption）之前。


* multiple：布尔值，表示是否允许多项选择；等价于 HTML 中的 multiple 特性。
* options：控件中所有<option>元素的 HTMLCollection。
* remove(index)：移除给定位置的选项。
* selectedIndex：基于 0 的选中项的索引，如果没有选中项，则值为-1。对于支持多选的控件，只保存选中项中第一项的索引。


* size：选择框中可见的行数；等价于 HTML 中的 size 特性。
* 选择框的 type 属性不是"select-one"，就是"select-multiple"，这取决于 HTML 代码中有没有 multiple 特性。选择框的 value 属性由当前选中项决定，相应规则如下。


* 如果没有选中的项，则选择框的 value 属性保存空字符串。
* 如果有一个选中项，而且该项的 value 特性已经在 HTML 中指定，则选择框的 value 属性等于选中项的 value 特性。即使 value 特性的值是空字符串，也同样遵循此条规则。


* 如果有一个选中项，但该项的 value 特性在 HTML 中未指定，则选择框的 value 属性等于该项的文本。


* 如果有多个选中项，则选择框的 value 属性将依据前两条规则取得第一个选中项的值。

在 DOM 中，每个<option>元素都有一个 HTMLOptionElement 对象表示。为便于访问数据，**HTMLOptionElement 对象添加了下列属性：**

* index：当前选项在 options 集合中的索引。
* label：当前选项的标签；等价于 HTML 中的 label 特性。
* selected：布尔值，表示当前选项是否被选中。将这个属性设置为 true 可以选中当前选项。
* text：选项的文本。
* value：选项的值（等价于 HTML 中的 value 特性）。

```js
var selectbox = document.forms[0]. elements["location"];
//不推荐
var text = selectbox.options[0].firstChild.nodeValue; //选项的文本
var value = selectbox.options[0].getAttribute("value"); //选项的值
//推荐
var text = selectbox.options[0].text; //选项的文本
var value = selectbox.options[0].value; //选项的值
```

### 选择选项

对于只允许选择一项的选择框，访问选中项的最简单方式，就是使用选择框的 selectedIndex 属性。

```js
var selectedIndex = selectbox.selectedIndex;
var selectedOption = selectbox.options[selectedIndex];
alert("Selected index: " + selectedIndex + "\nSelected text: " +
selectedOption.text + "\nSelected value: " + selectedOption.value);
```

选择多项的选择框：在允许多选的选择框中设置选项的 selected 属性，不会取消对其他选中项的选择，因而可以动态选中任意多个项。但是，如果是在单选选择框中，修改某个选项的 selected 属性则会取消对其他选项的选择。需要注意的是，将 selected 属性设置为 false 对单选选择框没有影响。实际上， selected 属性的作用主要是确定用户选择了选择框中的哪一项。要取得所有选中的项，可以循环遍历选项集合，然后测试每个选项的selected 属性。

```js
function getSelectedOptions(selectbox){
    var result = new Array();
    var option = null;
    for (var i=0, len=selectbox.options.length; i < len; i++){
        option = selectbox.options[i];
        if (option.selected){
        	result.push(option);
        }
    }
    return result;
}
```

###添加选项

可以使用 JavaScript 动态创建选项，并将它们添加到选择框中。添加选项的方式有很多

* 第一种方式就是使用如下所示的 DOM 方法。

```js
var newOption = document.createElement("option");
newOption.appendChild(document.createTextNode("Option text"));
newOption.setAttribute("value", "Option value");
selectbox.appendChild(newOption);
```

* 第二种方式是使用 Option 构造函数来创建新选项，这个构造函数是 DOM 出现之前就有的，一直遗留到现在。 

```js
var newOption = new Option("Option text", "Option value");
selectbox.appendChild(newOption); //在 IE8 及之前版本中有问题
```

* 第三种添加新选项的方式是使用选择框的 add()方法。 

```js
var newOption = new Option("Option text", "Option value");
selectbox.add(newOption, undefined); //最佳方案
```

在 IE 和兼容 DOM 的浏览器中，上面的代码都可以正常使用。如果你想将新选项添加到其他位置（不是最后一个），就应该使用标准的 DOM 技术和 insertBefore()方法。

###移除选项

* 首先，可以使用 DOM 的 removeChild()方法，为其传入要移除的选项。

```js
selectbox.removeChild(selectbox.options[0]); //移除第一个选项
```

* 其次，可以使用选择框的 remove()方法。

```js
selectbox.remove(0); //移除第一个选项
```

* 最后一种方式，就是将相应选项设置为 null。这种方式也是 DOM 出现之前浏览器的遗留机制。

```js
selectbox.options[0] = null; //移除第一个选项
```

###移动和重排选项

在 DOM 标准出现之前，将一个选择框中的选项移动到另一个选择框中是非常麻烦的。整个过程要涉及从第一个选择框中移除选项，然后以相同的文本和值创建新选项，最后再将新选项添加到第二个选择框中。而使用 DOM 的 appendChild()方法，就可以将第一个选择框中的选项直接移动到第二个选择框中。我们知道，如果为 appendChild()方法传入一个文档中已有的元素，那么就会先从该元素的父节点中移除它，再把它添加到指定的位置。

```js
var selectbox1 = document.getElementById("selLocations1");
var selectbox2 = document.getElementById("selLocations2");
selectbox2.appendChild(selectbox1.options[0]);
```

> 将第一个选择框中的第一个选项移动到第二个选择框中

移动选项与移除选项有一个共同之处，即会重置每一个选项的 index 属性。
重排选项次序的过程也十分类似，最好的方式仍然是使用 DOM 方法。要将选择框中的某一项移动到特定位置，最合适的 DOM 方法就是 insertBefore()； appendChild()方法只适用于将选项添加到选择框的最后。要在选择框中向前移动一个选项的位置，可以使用以下代码：

```js
var optionToMove = selectbox.options[1];
selectbox.insertBefore(optionToMove,selectbox.options[optionToMove.index-1]);
```



