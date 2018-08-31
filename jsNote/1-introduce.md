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

总结：

1. <script>的六种标签属性

2. script标签放置位置

   正常放置在页面后面，因js解析执行时会阻滞页面加载

   页面渲染有关的js要放前面

3. script引用外部文件可缓存，可维护

   内部文件减少链接数，提升性能