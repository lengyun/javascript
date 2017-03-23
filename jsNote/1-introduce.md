##JavaScript实现

JavaScript实现包括如下三个部分

|     核心     |      文档对象模型       |    浏览器对象模型     |
| :--------: | :---------------: | :------------: |
| ECMAScript |        DOM        |      BOM       |
|  提供核心语言功能  | 提供访问和操作网页内容的方法和接口 | 提供与浏览器交互的方法和接口 |

###  <script>元素属性

* async ：可选，表示应该立即下载脚本，但不应妨碍页面中的其他操作。

* defer：表示脚本可以延迟到文档完全被解析和显示之后再执行。

  延迟脚本：

  ```html
  <script type="text/javascript" defer="defer" src="example1.js"></script>
  ```

  异步脚本：

  ```html
  <script type="text/javascript" async src="example1.js"></script>
  ```
