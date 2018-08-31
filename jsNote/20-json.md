## json

关于 JSON，最重要的是要理解它是一种数据格式，不是一种编程语言。

### 语法

JSON 的语法可以表示以下三种类型的值。

* **简单值**：使用与 JavaScript 相同的语法，可以在 JSON 中表示字符串、数值、布尔值和 null。但 JSON 不支持 JavaScript 中的特殊值 undefined。


* **对象**：对象作为一种复杂数据类型，表示的是一组无序的键值对儿。而每个键值对儿中的值可以是简单值，也可以是复杂数据类型的值。


* **数组**：数组也是一种复杂数据类型，表示一组有序的值的列表，可以通过数值索引来访问其中的值。数组的值也可以是任意类型——简单值、对象或数组。

JSON 不支持变量、函数或对象实例，它就是一种表示结构化数据的格式，虽然与 JavaScript 中表示数据的某些语法相同，但它并不局限于 JavaScript 的范畴。

```js
// javascript 对象
var person = {
    name: "Nicholas",
    age: 29
};
// json对象
{
"name": "Nicholas",
"age": 29
}
```

两者比较：首先，JSON没有声明变量。其次，JSON没有尾部的分号。

JSON对象的属性必须加双引号，这在JSON中是必须的。属性的值可以是简单值，也可以是复杂类型值。

JSON 中对象的属性名任何时候都必须加双引号。手工编写 JSON 时，忘了给对象属性名加双引号或者把双引号写成单引号都是常见的错误。

### 解析与序列化

JSON 之所以流行，拥有与 JavaScript 类似的语法并不是全部原因。更重要的一个原因是，可以把JSON 数据结构解析为有用的 JavaScript 对象。与 XML 数据结构要解析成 DOM 文档而且从中提取数据极为麻烦相比， JSON 可以解析为JavaScript 对象的优势极其明显。

#### 1. JSON对象

早期的 JSON 解析器基本上就是使用 JavaScript 的 eval()函数。由于 JSON 是 JavaScript 语法的子集，因此 eval()函数可以解析、解释并返回 JavaScript 对象和数组。 ECMAScript 5 对解析 JSON 的行为进行规范，定义了全局对象 JSON。支持这个对象的浏览器有 IE 8+、 Firefox 3.5+、 Safari 4+、 Chrome和 Opera 10.5+。对于较早版本的浏览器，可以使用一个 shim：https://github.com/douglascrockford/JSON-js。在旧版本的浏览器中，使用 eval()对 JSON 数据结构求值存在风险，因为可能会执行一些恶意代码。对于不能原生支持 JSON 解析的浏览器，使用这个 shim 是最佳选择。

JSON 对象有两个方法： **stringify()**和 **parse()**。在最简单的情况下，这两个方法分别用于把JavaScript 对象序列化为 JSON 字符串和把 JSON 字符串解析为原生 JavaScript 值。

```js
var book = {
    title: "Professional JavaScript",
    authors: [
    	"Nicholas C. Zakas"
    ],
    edition: 3,
    year: 2011
};
var jsonText = JSON.stringify(book);
// 默认情况下， JSON.stringify()输出的 JSON 字符串不包含任何空格字符或缩进
{"title":"Professional JavaScript","authors":["Nicholas C. Zakas"],"edition":3,
"year":2011}
```

在序列化 JavaScript 对象时，所有函数及原型成员都会被有意忽略，不体现在结果中。此外，值为undefined 的任何属性也都会被跳过。结果中最终都是值为有效 JSON 数据类型的实例属性。

JSON 字符串直接传递给 JSON.parse()就可以得到相应的 JavaScript 值。

```js
var bookCopy = JSON.parse(jsonText);
```

 book 与 bookCopy 具有相同的属性，但它们是两个独立的、没有任何关系的对象。

#### 2. 序列化选项

 JSON.stringify()除了要序列化的 JavaScript 对象外，还可以接收另外两个参数，这两个参数用于指定以不同的方式序列化 JavaScript 对象。**第一个参数**是个**过滤器**，可以是一个数组，也可以是一个函数；**第二个参数**是一个选项，表示是否在 JSON 字符串中保留缩进。单独或组合使用这两个参数，可以更全面深入地控制 JSON 的序列化。

* 过滤结果

  如果过滤器参数是数组，那么 JSON.stringify()的结果中将只包含数组中列出的属性。

  ```js
  var book = {
      "title": "Professional JavaScript",
      "authors": [
      	"Nicholas C. Zakas"
      ],
      edition: 3,
      year: 2011
  };
  var jsonText = JSON.stringify(book, ["title", "edition"]);
  // 返回结果
  {"title":"Professional JavaScript","edition":3}
  ```

  如果第二个参数是函数。传入的函数接收两个参数，属性名和属性值。根据属性名可以知道应该如何处理要序列化的对象中的属性。属性名只能是字符串，而在值并非键值对格式的值时，键名可以是空字符串。

  为了改变序列化对象的结果，函数返回的值就是相应键的值。如果函数返回了undefined，那么相应的属性会被忽略。

  ```js
  var book = {
      "title": "Professional JavaScript",
      "authors": [
      	"Nicholas C. Zakas"
      ],
      edition: 3,
      year: 2011
  };
  var jsonText = JSON.stringify(book, function(key, value){
      switch(key){
          case "authors":
          	return value.join(",")
          case "year":
          	return 5000;
          case "edition":
          	return undefined;
          default:
          	return value;
      }
  });
  // 过滤结果
  {"title":"Professional JavaScript","authors":"Nicholas C. Zakas","year":5000}
  ```

  > 这里，函数过滤器根据传入的键来决定结果。如果键为“authors”，就将数组链接为一个字符串；如果键为“year”，就将其值设置为5000；如果键为‘edition’，通过返回undefined删除该属性。最后一定要提供default项，此时返回传入的值，以便其他值都能正常出现在结果中。实际上，第一次调用这个函数过滤器，传入的键是一个空字符串，而值就是book对象。

* 字符串缩进

  JSON.stringify()方法的第三个参数用于控制结果中的缩进和空白符。如果这个参数是一个数值，那它表示的是每个级别缩进的空格数。

  ```js
  var book = {
      "title": "Professional JavaScript",
      "authors": [
     	 	"Nicholas C. Zakas"
      ],
      edition: 3,
      year: 2011
  };
  var jsonText = JSON.stringify(book, null, 4);
  //返回结果
  {
      "title": "Professional JavaScript",
      "authors": [
      	"Nicholas C. Zakas"
      ],
      "edition": 3,
      "year": 2011
  }
  ```

   JSON.stringify()也在结果字符串中插入了换行符以提高可读性。只要传入有效的控制缩进的参数值，结果字符串就会包含换行符。（只缩进而不换行意义不大。）最大缩进空格数为 10，所有大于 10 的值都会自动转换为 10。

  如果缩进参数是一个字符串而非数值，则这个字符串将在 JSON 字符串中被用作缩进字符（不再使用空格）。在使用字符串的情况下，可以将缩进字符设置为制表符，或者两个短划线之类的任意字符。

  ```js
  var jsonText = JSON.stringify(book, null, " - -");
  // 返回结果
  {
  --"title": "Professional JavaScript",
  --"authors": [
  ----"Nicholas C. Zakas"
  --],
  --"edition": 3,
  --"year": 2011
  }
  ```

* toJSON()方法

  toJSON()方法，返回其自身的 JSON 数据格式。原生 Date 对象有一个 toJSON()方法，能够将 JavaScript的 Date 对象自动转换成 ISO 8601日期字符串（与在 Date 对象上调用 toISOString()的结果完全一样）

  可以为任何对象添加toJSON()方法

  ```js
  var book = {
      "title": "Professional JavaScript",
      "authors": [
      	"Nicholas C. Zakas"
      ],
      edition: 3,
      year: 2011,
      toJSON: function(){
      	return this.title;
      }
  };
  var jsonText = JSON.stringify(book);
  ```

> 以上代码在book对象上定义了一个toJSON()方法，该方法返回图书的书名。与Date对象类似，这个对象也将被序列化为一个简单的字符串而非对象。可以让toJSON()方法返回任意值，它都能正常工作。比如，可以让这个方法返回undefined，此时如果包含它的对象嵌入在另外一个对象中，会导致它的值变成null，而如果它是顶级对象，结果就是undefined。
>
> toJSON()可以作为函数过滤器的补充，因此理解序列化的内部顺序十分重要。

序列化该对象的顺序如下:

1. 如果存在toJSON() 方法而且能通过它取得有效的值，则调用该方法。否则，返回对象本身。
2. 如果提供了第二个参数，应用这个函数过滤器。传入函数过滤器的值是第一步返回的值。
3. 对第二步返回的每个值进行相应的序列化。
4. 如果提供了第三个参数，执行相应的格式化。

#### 3. 解析选项

JSON.parse()方法也可以接收另一个参数，该参数是一个函数，将在每个键值对儿上调用。为了区别 JSON.stringify()接收的替换（过滤）函数（replacer），这个函数被称为还原函数（reviver），但实际上这两个函数的签名是相同的——它们都接收两个参数，一个键和一个值，而且都需要返回一个值.

如果还原函数返回 undefined，则表示要从结果中删除相应的键；如果返回其他值，则将该值插入到结果中。在将日期字符串转换为 Date 对象时，经常要用到还原函数。例如：

```js
var book = {
    "title": "Professional JavaScript",
    "authors": [
    	"Nicholas C. Zakas"
    ],
    edition: 3,
    year: 2011,
    releaseDate: new Date(2011, 11, 1)
};
var jsonText = JSON.stringify(book);
var bookCopy = JSON.parse(jsonText, function(key, value){
    if (key == "releaseDate"){
        return new Date(value);
    } else {
        return value;
    }
});
alert(bookCopy.releaseDate.getFullYear());
```

以上代码先是为 book 对象新增了一个 releaseDate 属性，该属性保存着一个 Date 对象。这个对象在经过序列化之后变成了有效的 JSON 字符串，然后经过解析又在 bookCopy 中还原为一个 Date对象。还原函数在遇到"releaseDate"键时，会基于相应的值创建一个新的 Date 对象。结果就是bookCopy.releaseDate 属性中会保存一个 Date 对象。正因为如此，才能基于这个对象调用getFullYear()方法。




