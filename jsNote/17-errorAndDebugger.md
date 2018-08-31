## 错误处理

### try-catch语句

```js
try{
	// 可能会导致错误的代码
    window.someNonexistentFunction();
} catch(error){
	// 在错误发生时怎么处理
    alert(error.message)
}
```

如果 try 块中的任何代码发生了错误，就会立即退出代码执行过程，然后接着执行 catch 块。此时， catch 块会接收到一个包含错误信息的对象。与在其他语言中不同的是，即使你不想使用这个错误对象，也要给它起个名字。这个对象中包含的实际信息会因浏览器而异，但共同的是有一个保存着错误消息的 message 属性。 

#### 1.finally子句

虽然在 try-catch 语句中是可选的，但 finally 子句一经使用，其代码无论如何都会执行。换句话说， try 语句块中的代码全部正常执行， finally 子句会执行；如果因为出错而执行了 catch 语句块， finally 子句照样还会执行。只要代码中包含 finally 子句，则无论 try 或 catch 语句块中包含什么代码——甚至 return 语句，都不会阻止 finally 子句的执行。

```js
function testFinally(){
    try {
    	return 2;
    } catch (error){
    	return 1;
    } finally {
    	return 0;
    }
}
```

如果提供 finally 子句，则 catch 子句就成了可选的（catch 或 finally 有一个即可）。 

#### 2.错误类型

ECMA-262 定义了下列 7 种错误类型：

 Error
 EvalError
 RangeError
 ReferenceError
 SyntaxError
 TypeError
 URIError

* Error 是基类型，其他错误类型都继承自该类型。这个基类型的主要目的是供开发人员抛出自定义错误。
* EvalError 类型的错误会在使用 eval()函数而发生异常时被抛出。 简单地说，如果没有把 eval()当成函数调用，就会抛出错误
* RangeError 类型的错误会在数值超出相应范围时触发。
* ReferenceError 在找不到对象的情况下，会发生 ReferenceError错误。通常，在访问不存在的变量时，就会发生这种错误。


* SyntaxError，当我们把语法错误的 JavaScript 字符串传入 eval()函数时，就会导致此类错误。


* TypeError 类型在 JavaScript 中会经常用到，在变量中保存着意外的类型时，或者在访问不存在的方法时，都会导致这种错误。错误的原因虽然多种多样，但归根结底还是由于在执行特定于类型的操作时，变量的类型并不符合要求所致。
* URIError 在使用 encodeURI()或 decodeURI()，而 URI 格式不正确时，就会导致 URIError 错误。

想知道错误的类型，可以像下面这样在 try-catch 语句的 catch 语句中使用 instanceof 操作符。

```js
try {
	someFunction();
} catch (error){
    if (error instanceof TypeError){
    	//处理类型错误
    } else if (error instanceof ReferenceError){
    	//处理引用错误
    } else {
       //处理其他类型的错误
    }
}
```

#### 3.合理使用try-catch

当 try-catch 语句中发生错误时，浏览器会认为错误已经被处理了，因而不会通过本章前面讨论的机制记录或报告错误。对于那些不要求用户懂技术，也不需要用户理解错误的 Web 应用程序，这应该说是个理想的结果。

 try-catch 最适合处理那些我们无法控制的错误。假设你在使用一个大型 JavaScript 库中的函数，该函数可能会有意无意地抛出一些错误。由于我们不能修改这个库的源代码，所以大可将对该函数的调用放在 try-catch 语句当中，万一有什么错误发生，也好恰当地处理它们。

### 抛出错误

与 try-catch 语句相配的还有一个 throw 操作符，用于随时抛出自定义错误。抛出错误时，必须要给 throw 操作符指定一个值，这个值是什么类型，没有要求。

```js
throw 12345;
throw "Hello world!";
throw true;
throw { name: "JavaScript"};
```

在遇到 throw 操作符时，代码会立即停止执行。仅当有 try-catch 语句捕获到被抛出的值时，代码才会继续执行。

通过使用某种内置错误类型，可以更真实地模拟浏览器错误。

1. 抛出错误的时机

   要针对函数为什么会执行失败给出更多信息，抛出自定义错误是一种很方便的方式。应该在出现某种特定的已知错误条件，导致函数无法正常执行时抛出错误。换句话说，浏览器会在某种特定的条件下执行函数时抛出错误。

   ```js
   function process(values){
       if (!(values instanceof Array)){
       	throw new Error("process(): Argument must be an array.");
       }
       values.sort();
       for (var i=0, len=values.length; i < len; i++){
           if (values[i] > 100){
           	return values[i];
       	}
       }
       return -1;
   }
   ```

   > 如果 values 参数不是数组，就会抛出一个错误。错误消息中包含了函数的名称，以及为什么会发生错误的明确描述。

2. 抛出错误与使用try-catch

关于何时该抛出错误，而何时该使用 try-catch 来捕获它们，是一个老生常谈的问题。一般来说，应用程序架构的较低层次中经常会抛出错误，但这个层次并不会影响当前执行的代码，因而错误通常得不到真正的处理。如果你打算编写一个要在很多应用程序中使用的 JavaScript 库，甚至只编写一个可能会在应用程序内部多个地方使用的辅助函数，我都强烈建议你在抛出错误时提供详尽的信息。然后，即可在应用程序中捕获并适当地处理这些错误。

说到抛出错误与捕获错误，我们认为只应该捕获那些你确切地知道该如何处理的错误。捕获错误的目的在于避免浏览器以默认方式处理它们；而抛出错误的目的在于提供错误发生具体原因的消息。

### 错误（error）事件

任何没有通过try-catch处理的错误都会触发window对象的error事件。这个事件时web浏览器早起支持的事件之一。在任何web浏览器中，onerror事件处理程序都不会创建event对象，但它接受三个参数：错误消息、错误所在URL和行号。

要指定 onerror 事件处理程序，必须使用如下所示的 DOM0 级技术，它没有遵循“ DOM2 级事件”的标准格式。

```js
window.onerror = function(message, url, line){
	alert(message);
    return false; //阻止浏览器报告错误的默认行为
};
```

只要发生错误，无论是不是浏览器生成的，都会触发 error 事件，并执行这个事件处理程序。然后，浏览器默认的机制发挥作用，像往常一样显示出错误消息。像下面这样在事件处理程序中返回false，可以阻止浏览器报告错误的默认行为。

通过返回 false，这个函数实际上就充当了整个文档中的 try-catch 语句，可以捕获所有无代码处理的运行时错误。这个事件处理程序是避免浏览器报告错误的最后一道防线，理想情况下，只要可能就不应该使用它。只要能够适当地使用 try-catch 语句，就不会有错误交给浏览器，也就不会触发error 事件。

图像也支持 error 事件。只要图像的 src 特性中的 URL 不能返回可以被识别的图像格式，就会触发 error 事件。此时的 error 事件遵循 DOM 格式，会返回一个以图像为目标的 event 对象。

```js
var image = new Image();
EventUtil.addHandler(image, "load", function(event){
	alert("Image loaded!");
});
EventUtil.addHandler(image, "error", function(event){
	alert("Image not loaded!");
});
image.src = "smilex.gif"; //指定不存在的文件
```

###处理错误的策略

###常见的错误类型

* 类型转换错误
* 数据类型错误
* 通信错误

1. 类型转换错误

   类型转换错误发生在使用某个操作符，或者使用其他可能会自动转换值的数据类型的语言结构时。

   **操作符：**在使用相等（==）和不相等（!=）操作符，或者在 if、 for 及 while 等流控制语句中使用非布尔值时，最常发生类型转换错误。

   **流控制语句：**像 if 之类的语句在确定下一步操作之前，会自动把任何值转换成布尔值。尤其是 if 语句，如果使用不当，最容易出错。

   在流控制语句中使用非布尔值，是极为常见的一个错误来源。为避免此类错误，就要做到在条件比较时切实传入布尔值。实际上，执行某种形式的比较就可以达到这个目的。

2. 数据类型错误

   JavaScript 是松散类型的，也就是说，在使用变量和函数参数之前，不会对它们进行比较以确保它们的数据类型正确。为了保证不会发生数据类型错误，只能依靠开发人员编写适当的数据类型检测代码。在将预料之外的值传递给函数的情况下，最容易发生数据类型错误。

   ```js
   function getQueryString(url){
       if (typeof url == "string"){ //通过检查类型确保安全
           var pos = url.indexOf("?");
           if (pos > -1){
           	return url.substring(pos +1);
           }
       }
       return "";
   }
   //安全，非数组值将被忽略
   function reverseSort(values){
       if (values instanceof Array){ //问题解决了
           values.sort();
           values.reverse();
       }
   }
   ```

   大体上来说，基本类型的值应该使用 typeof 来检测，而对象的值则应该使用 instanceof 来检测。根据使用函数的方式，有时候并不需要逐个检测所有参数的数据类型。但是，面向公众的 API 则必须无条件地执行类型检查，以确保函数始终能够正常地执行。

3. 通信错误

   第一种通信错误与格式不正确的 URL 或发送的数据有关。最常见的问题是在将数据发送给服务器之前，没有使用 encodeURIComponent()对数据进行编码。

   ```js
   http://www.yourdomain.com/?redir=http://www.someotherdomain.com?a=b&c=d
   http://www.yourdomain.com/?redir=http%3A%2F%2Fwww.someotherdomain.com%3Fa%3Db%26c%3Dd
   // 处理查询字符串的函数
   function addQueryStringArg(url, name, value){
       if (url.indexOf("?") == -1){
           url += "?";
       } else {
           url += "&";
       }
       url += encodeURIComponent(name) + "=" + encodeURIComponent(value);
       return url;
   }
   ```

###区分致命错误和非致命错误

###把错误记录到服务器



