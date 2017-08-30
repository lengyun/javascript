DOM（文档对象模型）是针对 HTML 和 XML 文档的一个 API（应用程序编程接口）。 DOM 描绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分

## 节点层次

### Node类型

> DOM1级定义了一个Node接口，该接口将有DOM中的所有节点类型实现。这个Node接口在javascript中是作为Node类型实现的；javascript中的所有节点类型都继承自Node类型，因此所有节点类型都共享着相同的基本属性和方法。

> 每个节点都有一个nodeType属性，用于表明节点的类型。

* Node.ELEMENT_NODE(1) 元素节点
* Node.ATTRIBUTE_NODE(2)属性节点
* Node.TEXT_NODE(3) 文本节点
* Node.COMMENT_NODE(8) 注释节点
* Node.DOCUMENT_NODE(9)文档节点

> 通过比较节点的nodeType属性和这些常量可以确定节点是什么类型
>
> 兼容ie最后使用数字比较

* **节点关系**

  * childNodes属性

    > 每个节点都有一个childNode属性，其中保存着一个NodeList对象，它是一种类数组对象，用于保存一组有序的节点，可通过位置来访问这些节点。NodeList对象是基于DOM结构动态执行查询的结果

  * parentNode属性

    > 每个节点都有一个parentNode属性，该属性指向文档树中的父节点。

  * 同胞节点

    > childNodes列表中每个节点之间都是同胞节点，通过previousSibling和nextSibling属性，可以访问同一列表中的其他节点

  * 父亲的firstChild和lastChild属性

  * hasChildNodes()方法

    > 在节点包含一个或多个子节点的情况下返回true

  * ownerDocument属性

    > 所有即诶单都有一个ownerDocument属性，指向表示整个文档的文档节点。

* 操作节点

  * appendChild()

    > 用于向childNode列表的末尾添加一个节点。返回新增节点。如果传入appendChild()中的节点已经是文档的一部分，那结果就是将该节点从原来的位置转移到新位置

  * insertBefore()

    > 用于把节点放在childNodes列表中某个特定的位置上。两个参数：要插入的节点和作为参照的节点。

  * replaceChild()

    > 两个参数：要擦入的节点和要替换的节点。要替换的节点将由这个方法返回并从文档树中被移除，同时由要插入的节点占据其位置。

  * removeChild()

    > 只移除节点，一个参数：要移除的节点。

  * cloneNode()

    > 用于创建调用这个方法的节点的一个完全相同的副本。布尔值参数表示是否执行深复制。复制后节点属于文档但没有父节点

  * normalize()

    > 处理文档树中的文本节点。在某个节点上调用此方法，会在该即诶单的后代节点中查找空文本节点删除，相邻的文本节点合并为一个文本节点

### Document类型

> js通过Document类型表示文档。在浏览器中， document 对象是 HTMLDocument（继承自 Document 类型）的一个实例，表示整个 HTML 页面。而且，document对象是window对象的一个属性，因此可以将其作为全局对象来访问。

Document节点特征：

* nodeType 为 9
* nodeName为 #document
* nodeValue 为 null
* parentNode 为 null
* ownerDocument 为 null
* 子节点可能是DocumentType（最多一个）、Element (最多一个)、ProcessingInstruction或Comment

Document类型可以表示HTML页面或者其他基于XML的文档。不过最常见的应用还是最为HTMLDocument实例的document对象。

* 文档的子节点

  * document.childNodes[0]

  * document.firstChild

  * document.documentElement属性

    > 始终指向HTML页面的<html>元素

  * document.body属性

    > 直接指向<body>元素

  * document.doctype属性（用处很有限）

    > 取得<!DOCTYPE>  IE8及以下不支持

    > <html>元素外部的注释没有什么用处

* 文档信息

  > 作为HTMLDocument 的一个实例，document对象还有一些标准的Document对象所没有的属性。

  ```js
  //取得文档标题
  var originalTitle = document.title;
  //设置文档标题
  document.title = "New page title";
  //取得完整的 URL
  var url = document.URL;
  //取得来源页面的 URL
  var referrer = document.referrer;
  //取得域名
  var domain = document.domain;
  //设置域名地址
  document.domain = "baidu.com" 
  //不能将这个属性设置为 URL 中不包含的域，
  //////////////////////////////////////
  //域名一开始是“松散的”（loose），那么不能将它再设置为“紧绷的”（tight）
  //假设页面来自于 p2p.wrox.com 域
  document.domain = "wrox.com"; //松散的（成功）
  document.domain = "p2p.wrox.com"; //紧绷的（出错！）
  ```

  > 可解决子域之间跨域问题：
  >
  > 而 通 过 将 每 个 页 面 的document.domain 设置为相同的值，这些页面就可以互相访问对方包含的 JavaScript 对象了

* 查找元素

  * getElementById()

    > 一个参数：元素ID 
    >
    > 返回：找到的元素，否则返回null
    >
    > IE8及以下，不区分大小写。多个只取第一个元素

  * getElementByTagName()

    > 一个参数：要取得元素的标签名
    >
    > 返回：包含零或多个元素的NodeList。HTML文档中，返回HTMLCollection对象

    HTMLCollection对象是一个“动态”集合

    ```js
    var images = document.getElementsByTagName("img");
    alert(images.length); //输出图像的数量
    alert(images[0].src); //输出第一个图像元素的 src 特性
    alert(images.item(0).src); //输出第一个图像元素的 src 特性
    ```

    > HTMLCollection 对象还有一个方法，叫做 namedItem()，使用这个方法可以通过元素的 name特性取得集合中的项。

    ```js
    //<img src="myimage.gif" name="myImage">
    var myImage = images.namedItem("myImage");
    ```

    ​

    > HTMLCollection 还支持按名称访问项

    ```js
    var myImage = images["myImage"]
    ```

    > 对 HTMLCollection 而言，我们可以向方括号中传入数值或字符串形式的索引值。在后台，对数值索引就会调用 item()，而对字符串索引就会调用 namedItem()。

    ​

    ```js
    var allElements = document.getElementsByTagName("*");
    //想取得文档中的所有元素
    ```

  * getElementsByName()

    > HTMLDocument 类型才有的方法。
    >
    > 返回带有给定name特性的所有元素的HTMLCollectioin对象。

* 特殊集合

  > 这些集合都是 HTMLCollection 对象

  * document.anchors

    > 这些集合都是 HTMLCollection 对象

  * document.applets

    > 包含文档中所有的<applet>元素，因为不再推荐使用<applet>元素，所以这个集合已经不建议使用了；

  * document.forms

    > 包含文档中所有的<form>元素，与document.getElementsByTagName("form")得到的结果相同；

  * document.imags

    > 包含文档中所有的<img>元素，与 document.getElementsByTagName("img")得到的结果相同；

  * document.links

    > 包含文档中所有的<img>元素，与 document.getElementsByTagName("img")得到的结果相同；

* DOM一致性检测

  ```js
  var hasXmlDom = document.implementation.hasFeature("XML", "1.0");
  ```

* 文档写入

  * write()  接受一个字符串参数，即要写入到输出流中的文本。
  * writeln() 同上，会在字符串末尾添加一个换行符（\n）
  * open()
  * close()

### Element类型

> Element 类型用于表现 XML 或 HTML 元素，提供了对元素标签名、子节点及特性的访问

1. nodeType的值为1
2. nodeName的值为元素的标签名
3. nodeValue的值为null
4. parentNode 可能是Document 或 Element
5. 其子节点可能是 Element、 Text、 Comment、 ProcessingInstruction、 CDATASection 或EntityReference。

> 元素标签名：nodeName属性 或tagName属性

> 在 HTML 中，标签名始终都以全部大写表示


```js
if (element.tagName == "div"){ //不能这样比较，很容易出错！
//在此执行某些操作
}
if (element.tagName.toLowerCase() == "div"){ //这样最好（适用于任何文档）
//在此执行某些操作
}
```


* HTML元素

  > 所有 HTML 元素都由 HTMLElement 类型表示，不是直接通过这个类型，也是通过它的子类型来表示。HTMLElement 类型直接继承自 Element 并添加了一些属性。添加的这些属性分别对应于每个 HTML元素中都存在的下列标准特性

  *  id，元素在文档中的唯一标识符。
  *  title，有关元素的附加说明信息，一般通过工具提示条显示出来。
  *  className，与元素的 class 特性对应，即为元素指定的 CSS 类。没有将这个属性命名为 class，是因为 class 是 ECMAScript 的保留字
  *  lang，元素内容的语言代码，很少使用。
  * dir，语言的方向，值为"ltr"（left-to-right，从左至右）或"rtl"（right-to-left，从右至左） ， 也很少使用。

  ```js
  <div id="myDiv" class="bd" title="Body text" lang="en" dir="ltr"></div>
    //获取元素指定信息
    var div = document.getElementById("myDiv");
    alert(div.id); //"myDiv""
    alert(div.className); //"bd"
    alert(div.title); //"Body text"
    alert(div.lang); //"en"
    alert(div.dir); //"ltr"
    //为属性赋予新值
    div.id = "someOtherId";
    div.className = "ft";
    div.title = "Some other text";
    div.lang = "fr";
    div.dir ="rtl";
  ```




> 每个元素都有一或多个特性，这些特性的用途是给出相应元素或其内容的附加信息。操作特性的DOM 方法主要有如下三个 getAttribute()、 setAttribute()和 removeAttribute()这三个方法可以针对任何特性使用，包括那些以 HTMLElement 类型属性的形式定义的特性。

* 取得特性

  * getAttribute() 

    ```js
    var div = document.getElementById("myDiv");
    alert(div.getAttribute("id")); //"myDiv"
    alert(div.getAttribute("class")); //"bd"特性名与实际的特性名相
    alert(div.getAttribute("title")); //"Body text"
    alert(div.getAttribute("lang")); //"en"
    alert(div.getAttribute("dir")); //"ltr"
    //也可以取得自定义属性
    div.getAttribute("my_special_attribute")
    ```

    任何元素的所有特性，也都可以通过 DOM 元素本身的属性来访问。

    ```js
    <div id="myDiv" align="left" my_special_attribute="hello!"></div>
    alert(div.id); //"myDiv"
    alert(div.my_special_attribute); //undefined（ IE 除外）
    alert(div.align); //"lef

    ```

    * 特殊的特性

      * style

        > getAttribute() 返回css文本，元素属性访问返回对象

      * onclock事件处理程序 

        > getAttribute()返回代码字符串，元素访问返回js函数

* 设置特性

  * setAttribute()

  ```js
  div.setAttribute("id", "someOtherId");
  div.setAttribute("class", "ft");
  div.setAttribute("title", "Some other text");
  div.setAttribute("lang","fr");
  div.setAttribute("dir", "rtl");
  //可以操作 HTML 特性也可以操作自定义特性。通过这个方法设置的特性名会被统一转换为小写形式，
  ```

  直接给属性赋值可以设置特性的值

  ```js
  div.id = "someOtherId";
  div.align = "left";
  //不能添加自定义特性
  ```

  * removeAttribute()

    > 这个方法用于彻底删除元素的特性。调用这个方法不仅会清除特性的值，而且也会从元素中完全删除特性

    `div.removeAttribute("class");`

* attributes属性

  > Element 类型是使用 attributes 属性的唯一一个 DOM 节点类型。attributes 属性中包含一个NamedNodeMap，与 NodeList 类似，也是一个“动态”的集合。元素的每一个特性都由一个 Attr 节点表示，每个节点都保存在 NamedNodeMap 对象中。

  NamedNodeMap 对象拥有下列方法:

  * getNamedItem(name)：返回 nodeName 属性等于 name 的节点；
  *  removeNamedItem(name)：从列表中移除 nodeName 属性等于 name 的节点；
  *  setNamedItem(node)：向列表中添加节点，以节点的 nodeName 属性为索引；
  *  item(pos)：返回位于数字 pos 位置处的节点。

  ```js
  var element = document.getElementById("content")
  element.attributes.getNamedItem("id") //id= "content"
  element.attributes.getNamedItem("id").nodeName //"id"
  element.attributes.getNamedItem("id").nodeValue //"content"
  element.attributes["id"].nodeValue; //content
  element.attributes["id"].nodeValue = "someOtherId";//设置
  ```

  > attributes 的 方 法 不 够 方 便 ， 因 此 开 发 人 员 更 多 的 会 使 用getAttribute()、 removeAttribute()和 setAttribute()方法。

  > 遍历元素的特性， attributes 属性倒是可以派上用场。

  > 因E7 及更早的版本会返回 HTML 元素中所有可能的特性,所以使用specified属性，过滤下
  >
  > 每个特性节点都有一个名为 specified 的属性，这个属性的值如果为 true，则意味着要么是在 HTML 中指定了相应特性，要么是通过 setAttribute()方法设置了该特性

  ```js
  function outputAttributes(element){
    var pairs = new Array(),
        attrName,
        attrValue,
        i,
        len;
    for (i=0, len=element.attributes.length; i < len; i++){
        attrName = element.attributes[i].nodeName;
        attrValue = element.attributes[i].nodeValue;
        if (element.attributes[i].specified) {
            pairs.push(attrName + "=\"" + attrValue + "\"");
        }
  }
  return pairs.join(" ");
  }
  ```

* 创建元素

  >  document.createElement()方法可以创建新元素。这个方法只接受一个参数，即要创建元素的标签名。这个标签名在 HTML 文档中不区分大小写，而在 XML（包括 XHTML）文档中，则是区分大小写的。

  ``` js
  var div = document.createElement("div");
  div.id = "myNewDiv";
  div.className = "box";
  document.body.appendChild(div);//把新创建的元素添加到文档的<body>元素中

  //IE中可以这样用
  var div = document.createElement("<div id=\"myNewDiv\" class=\"box\"></div >");
  ```

* 元素的子节点

  > 元 素 可 以 有 任 意 数 目 的 子 节 点 和 后 代 节 点 ， 因 为 元 素 可 以 是 其 他 元 素 的 子 节 点 。 元 素 的childNodes 属性中包含了它的所有子节点，这些子节点有可能是元素、文本节点、注释或处理指令。

  > 元 素 也 支 持getElementsByTagName()方法。在通过元素调用这个方法时，除了搜索起点是当前元素之外，其他方面都跟通过 document 调用这个方法相同

  ```js
  var ul = document.getElementById("myList");
  var items = ul.getElementsByTagName("li");
  ```

### Text类型

> 文本节点由 Text 类型表示，包含的是可以照字面解释的纯文本内容。纯文本中可以包含转义后的HTML 字符，但不能包含 HTML 代码

1.  nodeType 的值为 3；
2.  nodeName 的值为"#text"；
3.  nodeValue 的值为节点所包含的文本；
4.  parentNode 是一个 Element；

​        不支持（没有）子节点

> 操作节点中的文本:
>
> * appendData(text)：将 text 添加到节点的末尾。
> *  deleteData(offset, count)：从 offset 指定的位置开始删除 count 个字符。
> *  insertData(offset, text)：在 offset 指定的位置插入 text。
> *  replaceData(offset, count, text)：用 text 替换从 offset 指定的位置开始到 offset+count 为止处的文本。
> *  splitText(offset)：从 offset 指定的位置将当前文本节点分成两个文本节点。
> *  substringData(offset, count)：提取从 offset 指定的位置开始到 offset+count 为止处的字符串。

* 创建文本节点

  *  document.createTextNode()

    > 参数——要插入节点中的文本

     normalize()   

    > 将相邻文本节点合并

    ​

* 规范化文本节点

* 分割文本节点


* Comment类型
* DocumentType类型
* DocumentFragment类型
* Attr类型

## DOM操作技术

* 动态脚本
* 动态样式
* 操作表格
* 使用NodeList

