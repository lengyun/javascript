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

* 窗口大小

* 导航和打开窗口

* 间歇调用和超时调用

* 系统对话框

### location对象

* 查询字符串参数
* 位置操作

### navigator对象

* 检测插件
* 注册处理程序

### screen对象

### history对象



