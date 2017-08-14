### window对象

> 在浏览器中， window 对象有双重角色，它既是通过 JavaScript 访问浏览器窗口的一个接口，又是 ECMAScript 规定的 Global 对象

* 全局作用域

  > 所有在全局作用域中声明的变量、函数都会变成 window 对象的属性和方法

  > 全局变量不能通过 delete 操作符删除，而直接在 window 对象上的定义的属性可以。

  > 尝试访问未声明的变量会抛出错误，但是通过查询 window 对象，可以知道某个可能未声明的变量是否存在

* 窗口关系及框架

  > 如果页面中包含框架，则每个框架都拥有自己的 window 对象，并且保存在 frames 集合中。在 frames集合中，可以通过数值索引（从 0 开始，从左至右，从上到下）或者框架名称来访问相应的 window 对象。

  > **top对象** 始终指向最高（最外）层框架，也就是浏览器窗口

  ```js
  window.frames[0]
  window.frames["topFrame"]
  top.frames[0]
  top.frames["topFrame"]
  //省略window
  frames[0]
  frames["topFrame"]
  ```

  > **parent对象**  始终指向当前框架的直接上层框架。在某些情况下， parent 有可能等于 top；但在没有框架的情况下， parent 一定等于
  > top（此时它们都等于 window）。

  > **self对象** 它始终指向 window，self 和 window 对象可以互换使用

* 窗口位置

  > screenLeft 和 screenTop 属性，分别用于表示窗口相对于屏幕左边和上边的位置。 Firefox 则在screenX 和 screenY 属性中提供相同的窗口位置信息

  ```js
  var leftPos = (typeof window.screenLeft == "number") ?
  window.screenLeft : window.screenX;
  var leftPos = (typeof window.screenLeft == "number") ?
  window.screenLeft : window.screenX;
  ```

  > 在 IE、 Opera 中， screenLeft 和 screenTop 中保存的是从屏幕左边和上边到由 window 对象表示的页面可见区域的距离。在 IE、 Opera 中， screenLeft 和 screenTop 中保存的是从屏幕左边和上边到由 window 对象表示的页面可见区域的距离。
  >
  > 最终结果，就是无法在跨浏览器的条件下取得窗口左边和上边的精确坐标值。然而，使用 **moveTo()**和 **moveBy()**方法倒是有可能将窗口精确地移动到一个新位置。这两个方法都接收两个参数，moveTo()接收的是新位置的 x 和 y 坐标值，而 moveBy()接收的是在水平和垂直方向上移动的像素数。

  ```js
  //将窗口移动到屏幕左上角
  window.moveTo(0,0);
  //将窗向下移动 100 像素
  window.moveBy(0,100);
  //这两个方法可能会被浏览器禁用；而且，在 Opera 和 IE 7（及更高版本）中默认就
  //是禁用的。另外，这两个方法都不适用于框架，只能对最外层的 window 对象使用。
  ```

* 窗口大小

   outerWidth 和 outerHeight 返回浏览器窗口本身的尺寸

   innerWidth 和 innerHeight 则表示该容器中页面视图区的大小（减去边框宽度）

   document.documentElement.clientWidth 和
   document.documentElement.clientHeight 标准模式

   document.body.clientWidth 和 document.body.
   clientHeight混杂模式

  > 最终无法确定浏览器窗口本身的大小，但却可以取得页面视口的大小

  ```js
  var pageWidth = window.innerWidth,
  pageHeight = window.innerHeight;
  //IE8及更早版本
  if (typeof pageWidth != "number"){
      if (document.compatMode == "CSS1Compat"){//标准模式
          pageWidth = document.documentElement.clientWidth;
          pageHeight = document.documentElement.clientHeight;
      } else {//混杂模式
          pageWidth = document.body.clientWidth;
          pageHeight = document.body.clientHeight;
      }
  }
  ```

  > 使用 resizeTo()和 resizeBy()方法可以调整浏览器窗口的大小。这两个方法都接收两个参数，其中 resizeTo()接收浏览器窗口的新宽度和新高度，而 resizeBy()接收新窗口与原窗口的宽度和高度之差。
  >
  > >>可能被浏览器禁用不适用于框架,在 Opera和 IE7（及更高版本）中默认就是禁用的

* 导航和打开窗口

  > window.open()方法既可以导航到一个特定的 URL，也可以打开一个新的浏览器窗口。这个方法可以接收 4 个参数：要加载的 **URL**、**窗口目标**、一个**特性字符串**以及一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值。通常只须传递第一个参数，最后一个参数只在不打开新窗口的情况下使用

  > window.open()方法会返回一个指向新窗口的引用。引用的对象与其他 window 对象大致相似，但我们可以对其进行更多控制,调整大小或移动位置

  ```js
  var wroxWin = window.open("http://www.wrox.com/","wroxWindow",
  "height=400,width=400,top=10,left=10,resizable=yes");
  //调整大小
  wroxWin.resizeTo(500,500);
  //移动位置
  wroxWin.moveTo(100,100);
  //调用 close()方法还可以关闭新打开的窗口,仅适用于通过 window.open()打开的弹出窗口。
  wroxWin.close();
  //弹出窗口可以调用 top.close()在不经用户允许的情况下关闭自己,关闭之后，窗口的引用仍然还在。可在父窗口中判断弹出窗口是否关闭
  alert(wroxWin.closed); //true

  //新创建的 window 对象有一个 opener 属性，其中保存着打开它的原始窗口对象
  var wroxWin = window.open("http://www.wrox.com/","wroxWindow",
  "height=400,width=400,top=10,left=10,resizable=yes");
  alert(wroxWin.opener == window); //true
  ```

* 间歇调用和超时调用

  * 超时调用

    `setTimeout(function(){alert('aa')},500)`两个参数：要执行的代码和需等待时间毫秒

    ***js是单线程序的解释器***，一定时间内只执行一段代码。为了控制执行代码，就有个叫***javascript任务队列*** 任务会按照她们添加到队列的顺序执行。setTimeout()的第二个参数告诉js再过多久把当前任务添加到列队中。如果列队是空的，那么就会立即执行，如果列队不空，要等前面的代码执行完了以后再执行。

    > 调用setTimeout()后，该方法会返回一个数值ID，表示超时调用。是计划执行代码的唯一标示。可通过它来取笑超时调用。取消方法，clearTimeout(timeoutId)

  * 间歇性调用

    `setInterval()`参数同上，也返回一个间歇调用数值ID。`clearInterval(IntervalId)`取消间歇性调用

    > 它会按照指定的时间间隔重复执行代码，直至间歇调用被取消或者页面被卸载

    > 一般认为，使用超时调用来模拟间歇调用的是一种最佳模式。在开发环境下，很少使用真正的间歇调用，原因是后一个间歇调用可能会在前一个间歇调用结束之前启动

* 系统对话框

  * alert()

  * confirm()

    > 字符串参数显示在提示框中，根据用户选择返回布尔值，用作条件判断

  * prompt()

    > 接受两个参数，要显示给用户的文本提示和文本输入域的默认值
    >
    >  用户点击OK 按钮，则 prompt()返回文本输入域的值；如果用户单击了 Cancel 或没有单击OK 而是通过其他方式关闭了对话框，则该方法返回 null。

### location对象

| 属性名      | 例子                   | 说明                                    |
| -------- | -------------------- | ------------------------------------- |
| hash     | "#contents"          | 返回URL中的hash（#号后跟零或多个字符），如果URL         |
| host     | "www.wrox.com:80"    | 返回服务器名称和端口号（如果有）                      |
| hostname | "www.wrox.com"       | 返回不带端口号的服务器名称                         |
| href     | "http:/www.wrox.com" | 返回当前加载页面的完整URL。而location对象的           |
| pathname | "/WileyCDA/"         | 返回URL中的目录和（或）文件名                      |
| port     | "8080"               | 返回URL中指定的端口号。如果URL中不包含端口号，则这个属性返回空字符串 |
| protocol | "http:"              | 返回页面使用的协议。通常是http:或https:             |
| search   | "?q=javascript"      | 返回URL的查询字符串。这个字符串以问号开头                |

* 查询字符串参数

  ```js
  function getQueryStringArgs(){
  	//取得查询字符串并去掉开头的问号
  	var qs = (location.search.length > 0 ? location.search.substring(1) : ""),
  	//保存数据的对象
  	args = {},
  	//取得每一项
  	items = qs.length ? qs.split("&") : [],
  	item = null,
  	name = null,
      value = null,
  	//在 for 循环中使用
  	i = 0,
  	len = items.length;
  	//逐个将每一项添加到 args 对象中
  	for (i=0; i < len; i++){
  		item = items[i].split("=");
  		name = decodeURIComponent(item[0]);
  		value = decodeURIComponent(item[1]);
  		if (name.length) {
  			args[name] = value;
  		}
  	}
  	return args;
  }
  ```

  ​

* 位置操作

  * assign()方法

    `location.assign("http://www.wrox.com");`

    `location.href = "http://www.wrox.com";`也会调用`assign()`方法

  * replace()方法

    > 每次修改 location 的属性（低版本hash 除外），页面都会以新 URL 重新加载。修改 URL 之后，浏览器的历史记录中就会生成一条新记录，因此用户通过单击“后退”按钮都会导航到前一个页面。要禁用这种行为，可以使用 **replace()**方法。这个方法只接受一个参数，即要导航到的 URL；结果虽然会导致浏览器位置改变，但不会在历史记录中生成新记录。在调用 replace()方法之后，用户不能回到前一个页面

  * reload()方法

    > 重新加载当前显示的页面，如果要强制从服务器重新加载，则需要像下面这样为该方法传递参数 true

### navigator对象

* 检测插件
* 注册处理程序

### screen对象

### history对象

> go()方法可以在用户的历史记录中任意跳转

```js
//后退一页
history.go(-1);
//前进一页
history.go(1);
//前进两页
history.go(2);
//跳转到最近的 wrox.com 页面
history.go("wrox.com");
//跳转到最近的 nczonline.net 页面
history.go("nczonline.net");
//后退一页
history.back();
//前进一页
history.forward();
```





