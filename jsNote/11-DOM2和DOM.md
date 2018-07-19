##DOM变化

> DOM2 级和 3 级的目的在于扩展 DOM API，以满足操作 XML 的所有需求，同时提供更好的错误处
> 理及特性检测能力。从某种意义上讲，实现这一目的很大程度意味着对命名空间的支持。

> 可以通过下列代码来确定浏览器是否支持这些 DOM 模块。
>
> var supportsDOM2Core = document.implementation.hasFeature("Core", "2.0");
> var supportsDOM3Core = document.implementation.hasFeature("Core", "3.0");
> var supportsDOM2HTML = document.implementation.hasFeature("HTML", "2.0");
> var supportsDOM2Views = document.implementation.hasFeature("Views", "2.0");
> var supportsDOM2XML = document.implementation.hasFeature("XML", "2.0");

####针对XML命名空间的变化

> 有了 XML 命名空间，不同 XML 文档的元素就可以混合在一起，共同构成格式良好的文档，而不必担心发生命名冲突。HTML 不支持 XML 命名空间，但 XHTML 支持 XML 命名空间

> 命名空间要使用 xmlns 特性来指定。XHTML 的命名空间是 http://www.w3.org/1999/xhtml，

```xml
//所有元素默认都被视为 XHTML 命名空间中的元素
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
  	<title>Example XHTML page</title>
  </head>
  <body>
  	Hello world!
  </body>
</html>
//为 XML命名空间创建前缀，可以使用 xmlns 后跟冒号，再后跟前缀
<xhtml:html xmlns:xhtml="http://www.w3.org/1999/xhtml">
  <xhtml:head>
  	<xhtml:title>Example XHTML page</xhtml:title>
  </xhtml:head>
  <xhtml:body xhtml:class="home">//使用命名空间来限定特性
  	Hello world!
  </xhtml:body>
</xhtml:html>
//混合了 XHTML 和 SVG 语言的文档
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
  	<title>Example XHTML page</title>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" version="1.1"
    viewBox="0 0 100 100" style="width:100%; height:100%">
    	<rect x="0" y="0" width="100" height="100" style="fill:red"/>
    </svg>
  </body>
</html>
```

1. Node 类型的变化

   >  在 DOM2 级中， Node 类型包含下列特定于命名空间的属性。

   * localName：不带命名空间前缀的节点名称。
   * namespaceURI：命名空间 URI 或者（在未指定的情况下是） null。
   * prefix：命名空间前缀或者（在未指定的情况下是） null。

   ```xml
   //xml文件
   <html xmlns="http://www.w3.org/1999/xhtml">
     <head>
     	<title>Example XHTML page</title>
     </head>
     <body>
       <s:svg xmlns:s="http://www.w3.org/2000/svg" version="1.1"
       	viewBox="0 0 100 100" style="width:100%; height:100%">
       	<s:rect x="0" y="0" width="100" height="100" style="fill:red"/>
       </s:svg>
     </body>
   </html>
   ```

   ```js
   var htnlElement = document.getElementsByTagName('html')[0]
   htnlElement.localName  //html
   htnlElement.tagName // html
   htnlElement.namespaceURI // "http://www.w3.org/1999/xhtml"
   htnlElement.prefix // null
   var svgElement = document.getElementsByTagName('svg')[0]
   svgElement.localName //"svg"
   svgElement.tagName //"s:svg"
   svgElement.namespaceURI //"http://www.w3.org/2000/svg"
   svgElement.prefix //"s"
   ```

   >  DOM3 级在此基础上更进一步，又引入了下列与命名空间有关的方法。

   * isDefaultNamespace(namespaceURI)：在指定的 namespaceURI 是当前节点的默认命名空间的情况下返回 true。
   * lookupNamespaceURI(prefix)：返回给定 prefix 的命名空间。
   * lookupPrefix(namespaceURI)：返回给定 namespaceURI 的前缀。


   ```js
   document.body.isDefaultNamespace("http://www.w3.org/1999/xhtml") // true
   svgElement.lookupNamespaceURI("s") //"http://www.w3.org/2000/svg"
   svgElement.lookupPrefix("http://www.w3.org/2000/svg") // 's'
   ```

2. Document 类型的变化

   * createElementNS(namespaceURI, tagName)：使用给定的 tagName 创建一个属于命名空间 namespaceURI 的新元素。
   * createAttributeNS(namespaceURI, attributeName)：使用给定的 attributeName 创建一个属于命名空间 namespaceURI 的新特性。
   * getElementsByTagNameNS(namespaceURI, tagName)：返回属于命名空间 namespaceURI的 tagName 元素的 NodeList。


   ```js
   //创建一个新的 SVG 元素
   var svg = document.createElementNS("http://www.w3.org/2000/svg","svg");
   //创建一个属于某个命名空间的新特性
   var att = document.createAttributeNS("http://www.somewhere.com", "random");
   //取得所有 XHTML 元素
   var elems = document.getElementsByTagNameNS("http://www.w3.org/1999/xhtml", "*");
   ```

3. Element 类型的变化

   * getAttributeNS(namespaceURI,localName)：取得属于命名空间 namespaceURI 且名为localName 的特性。


   * getAttributeNodeNS(namespaceURI,localName)：取得属于命名空间 namespaceURI 且名为localName 的特性节点。


   * getElementsByTagNameNS(namespaceURI, tagName)：返回属于命名空间 namespaceURI的 tagName 元素的 NodeList。


   * hasAttributeNS(namespaceURI,localName)：确定当前元素是否有一个名为 localName的特性，而且该特性的命名空间是 namespaceURI。注意，“ DOM2 级核心”也增加了一个hasAttribute()方法，用于不考虑命名空间的情况。


   * removeAttriubteNS(namespaceURI,localName)：删除属于命名空间 namespaceURI 且名为 localName 的特性。


   * setAttributeNS(namespaceURI,qualifiedName,value)：设置属于命名空间 namespaceURI 且名为 qualifiedName 的特性的值为 value。
   * setAttributeNodeNS(attNode)：设置属于命名空间 namespaceURI 的特性节点

4. NamedNodeMap 类型的变化

   * getNamedItemNS(namespaceURI,localName)：取得属于命名空间 namespaceURI 且名为localName 的项。
   * removeNamedItemNS(namespaceURI,localName)：移除属于命名空间 namespaceURI 且名为 localName 的项。
   * setNamedItemNS(node)：添加 node，这个节点已经事先指定了命名空间信息。由于一般都是通过元素访问特性，所以这些方法很少使用。


####其他方面的变化

> 确保 API 的可靠性及完整性的变化

1. DocumentType 类型的变化

   > DocumentType 类型新增了 3 个属性： publicId、 systemId 和 internalSubset。

   ``` html
   <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd" [<!ELEMENT name (#PCDATA)>]>
   ```

   ```js
   publicId //"-//W3C//DTD HTML 4.01//EN"
   systemId //"http://www.w3.org/TR/html4/strict.dtd"
   internalSubset //<!ELEMENT name (#PCDATA)>
   //用于访问包含在文档类型声明中的额外定义，
   alert(document.doctype.publicId);
   alert(document.doctype.systemId);
   ```

2. Document 类型的变化

   * importNode()方法

   >  importNode()。这个方法的用途是从一个文档中取得一个节点，然后将其导入到另一个文档，使其成为这个文档结构的一部分。
   >
   >  在调用 importNode()时传入不同文档的节点则会返回一个新节点，这个新节点的所有权归当前文档所有。

   > 它接受两个参数：要复制的节点和一个表示是否复制子节点的布尔值。返回的结果是原来节点的副本，但能够在当前文档中使用

   ```js
   var newNode = document.importNode(oldNode, true); //导入节点及其所有子节点
   document.body.appendChild(newNode);
   ```

   * defaultView 属性

   > defaultView 的属性，其中保存着一个指针，指向拥有给定文档的窗口（或框架）。 IE 中有一个等价的属性名叫 parentWindow

   ```js
   var parentWindow = document.defaultView || document.parentWindow;
   ```

   *  createDocumentType()和 createDocument()

   >  document.implementation 对象规定了两个新方法：

   >  createDocumentType()用于创建一个新的 DocumentType节点，接受 3 个参数：文档类型名称、 publicId、 systemId。

   >  createDocument()方法。这个方法接受 3 个参数：针对文档中元素的 namespaceURI、文档元素的标签名、新文档的文档类型。

   ```js
   var doctype = document.implementation.createDocumentType("html",
   " -//W3C//DTD XHTML 1.0 Strict//EN",
   "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd");
   var doc = document.implementation.createDocument("http://www.w3.org/1999/xhtml",
   "html", doctype);
   ```

   *  createHTMLDocument()

   > 只有 Opera 和 Safari 支持这个方法

   >  createHTMLDocument()。这个方法的用途是创建一个完整的 HTML 文档，包括<html>、 <head>、 <title>和<body>元素。这个方法只接受一个参数，即新创建文档的标题（放在<title>元素中的字符串） ，返回新的 HTML 文档

   ``` js
   var htmldoc = document.implementation.createHTMLDocument("New Doc");
   alert(htmldoc.title); //"New Doc"
   alert(typeof htmldoc.body); //"object
   ```

3. Node 类型的变化

   * isSupported()方法

   > 用于确定当前节点具有什么能力。这个方法也接受相同的两个参数：特性名和特性版本号。

   * isSameNode()和 isEqualNode()

   >  这两个方法都接受一个节点参数，并在传入节点与引用的节点相同或相等时返回 true。

   *  setUserData()方法

   > 会将数据指定给节点，它接受 3 个参数：要设置的键、实际的数据（可以是任何数据类型）和处理函数。

   ```js
   document.body.setUserData("name", "Nicholas", function(){});
   // getUserData()并传入相同的键，就可以取得该数据
   var value = document.body.getUserData("name");
   ```

   > setUserData()中的处理函数会在带有数据的节点被复制、删除、重命名或引入一个文档时调用，因而你可以事先决定在上述操作发生时如何处理用户数据。
   >
   > 处理函数接受 5 个参数：表示操作类型的数值（1 表示复制， 2 表示导入， 3 表示删除， 4 表示重命名）、数据键、数据值、源节点和目标节点。在删除节点时，源节点是 null；除在复制节点时，目标节点均为 null。

4.  框架的变化

> 框架和内嵌框架分别用 HTMLFrameElement 和 HTMLIFrameElement 表示，它们在 DOM2 级中都有了一个新属性，名叫 contentDocument。这个属性包含一个指针，指向表示框架内容的文档对象。

```js
var iframe = document.getElementById("myIframe");
var iframeDoc = iframe.contentDocument; //在 IE8 以前的版本中无效
```

##样式

 HTML 中定义样式的方式有 3 种：

* 通过<link/>元素包含外部样式表文件
* 使用<style/>元素定义嵌入式样式
* 使用 style 特性定义针对特定元素的样式

确定浏览器是否支持 DOM2 级定义的 CSS 能力

```js
var supportsDOM2CSS = document.implementation.hasFeature("CSS", "2.0");
var supportsDOM2CSS2 = document.implementation.hasFeature("CSS2", "2.0")
```

### 访问元素的样式

任何支持 style 特性的 HTML 元素在 JavaScript 中都有一个对应的 style 属性。 这个 style 对象是 CSSStyleDeclaration 的实例，包含着通过 HTML 的 style 特性指定的所有样式信息，但不包含与外部样式表或嵌入样式表经层叠而来的样式。在 style 特性中指定的任何 CSS 属性都将表现为这个style 对象的相应属性。对于使用短划线（分隔不同的词汇，例如 background-image）的 CSS 属性名，必须将其转换成驼峰大小写形式，才能通过 JavaScript 来访问。

```
background-image  // style.backgroundImage
```

float不能直接转换 。由于 float 是 JavaScript 中的保留字。应该用“ cssFloat”而 IE支持的则是 styleFloat

#### 1.DOM 样式属性和方法

“ DOM2 级样式”规范还为 style 对象定义了一些属性和方法。这些属性和方法在提供元素的 style特性值的同时，也可以修改样式。

* cssText：如前所述，通过它能够访问到 style 特性中的 CSS 代码。
* item(index)：返回给定位置的 CSS 属性的名称。
* length：应用给元素的 CSS 属性的数量。
* getPropertyValue(propertyName)：返回给定属性值的字符串表示。
* getPropertyCSSValue(propertyName)：返回包含给定属性值的 CSSValue 对象。
* removeProperty(propertyName)：从样式中删除给定属性。
* parentRule：表示 CSS 信息的 CSSRule 对象。本节后面将讨论 CSSRule 类型。
* getPropertyPriority(propertyName)：如果给定的属性使用了!important 设置，则返回"important"；否则，返回空字符串。


* setProperty(propertyName,value,priority)：将给定属性设置为相应的值，并加上优先权标志（"important"或者一个空字符串）。

设置 cssText 是为元素应用多项变化最快捷的方式，因为可以一次性地应用所有变化。在读取模式下，cssText 返回浏览器对 style特性中 CSS 代码的内部表示。在写入模式下，赋给 cssText 的值会重写整个 style 特性的值；

```js
myDiv.style.cssText = "width: 25px; height: 100px; background-color: green";
alert(myDiv.style.cssText);
```

设计 length 属性的目的，就是将其与 item()方法配套使用，以便迭代在元素中定义的 CSS 属性。

取得 CSS 属性名（"background-color"，不是"backgroundColor"）。然后，就可以在 getPropertyValue()中使用取得的属性名进一步取得属性的值,如下：

```js
var prop, value, i, len;
for (i=0, len=myDiv.style.length; i < len; i++){
    prop = myDiv.style[i]; //或者 myDiv.style.item(i)
    value = myDiv.style.getPropertyValue(prop);
    alert(prop + " : " + value);
}
```

getPropertyValue()方法取得的始终都是 CSS 属性值的字符串表示。 getPropertyCSSValue()方法，它返回一个包含两个属性的 CSSValue 对象，对象包含 cssText 和 cssValueType。其中， cssText 属性的值与 getPropertyValue()返回的值相同，而 cssValueType 属性则是一个数值常量，表示值的类型： 0 表示继承的值， 1 表示基本的值， 2 表示值列表， 3 表示自定义的值。

removeProperty()方法。使用这个方法移除一个属性，意味着将会为该属性应用默认的样式```myDiv.style.removeProperty("border");```

#### 2.计算的样式

虽然 style 对象能够提供支持 style 特性的任何元素的样式信息，但它不包含那些从其他样式表层叠而来并影响到当前元素的样式信息。“ DOM2 级样式”增强了 document.defaultView，提供了getComputedStyle()方法。这个方法接受两个参数：要取得计算样式的元素和一个伪元素字符串（例如":after"）。如果不需要伪元素信息，第二个参数可以是 null。 getComputedStyle()方法返回一个 CSSStyleDeclaration 对象（与 style 属性的类型相同），其中包含当前元素的所有计算的样式。

```html
<!DOCTYPE html>
<html>
<head>
    <title>Computed Styles Example</title>
    <style type="text/css">
        #myDiv {
        background-color: blue;
        width: 100px;
        height: 200px;
        }
    </style>
</head>
<body>
	<div id="myDiv" style="background-color: red; border: 1px solid black"></div>
    <script>
    var myDiv = document.getElementById("myDiv");
	var computedStyle = document.defaultView.getComputedStyle(myDiv, null);
    alert(computedStyle.backgroundColor); // "red"
    alert(computedStyle.width); // "100px"
    alert(computedStyle.height); // "200px"
    alert(computedStyle.border); // 在某些浏览器中是"1px solid black"
    </script>
</body>
</html>
```

> IE 不支持 getComputedStyle()方法。在 IE 中，每个具有 style 属性的元素还有一个 currentStyle 属性。这个属性是 CSSStyleDeclaration 的实例，包含当前元素全部计算后的样式

```js
var myDiv = document.getElementById("myDiv");
var computedStyle = myDiv.currentStyle;
alert(computedStyle.backgroundColor); //"red"
alert(computedStyle.width); //"100px"
alert(computedStyle.height); //"200px"
alert(computedStyle.border); //undefined
```

所有计算的样式都是只读的；不能修改计算后样式对象中的 CSS 属性。此外，计算后的样式也包含属于浏览器内部样式表的样式信息，因此任何具有默认值的 CSS 属性都会表现在计算后的样式中。

### 操作样式表

#### 1.css规则

#### 2.创建规则

#### 3.删除规则

### 元素大小

####1.偏移量

####2.客户区大小

####3.滚动大小

####4.确定元素大小

##遍历

###NodeIterator

###TreeWalker

##范围

###DOM中的范围

####1.用 DOM 范围实现简单选择

####2.用 DOM 范围实现复杂选择

####3.操作 DOM 范围中的内容

####4.插入 DOM 范围中的内容

####5.折叠 DOM 范围

####6.比较 DOM 范围

####7.复制 DOM 范围

####8.清理 DOM 范围

###IE8及更早版本中的范围

####1.用 IE 范围实现简单的选择

####2.使用 IE 范围实现复杂的选择

####3.操作 IE 范围中的内容

####4.折叠 IE 范围

####5.比较 IE 范围

####6.复制 IE 范围