* 本章内容
  *  使用对象
  *  创建并操作数组
  *  理解基本的 JavaScript 类型
  *   使用基本类型和基本包装类型

### Object类型

在应用程序中存储和传输数据而言，Object是非常理想的选择。

* 创建方法:
  * new操作符

  ```javascript
    var person = new Object();
    person.name="suo"
  ```

  *   对象字面量

  ```javascript
  var person = {
    name : "suo",
    age : 30
  }

  var person = {} //与new Object()相同

  // 向函数传递大量参数
  displayInfo({
    name:"Nice",
    age:29
  })
  ```

* 对象属性访问

  * 点表示法

    ```javascript
    alert(person.name);
    ```

  * 方括号表示法

    ```JavaScript
    alert(person["name"]);//访问属性要以字符串形式

    var aa= "name";
    alert(person[aa]); //属性可以通过变量形式访问

    //特殊属性名，如：包含字符，关键字等
    person["first name"]="chris"
    ```

### Array类型

ECMAScript数组每一项可以保存任何类型的数据，数组大小可以动态调整，可随数据的添加自动增长以容纳新增数据。

* 创建方法

  * Array构造函数

    ```javascript
    var colors = new Array();
    var colors = new Array(20);//设置数组长度
    var colors = new Array("red","blue","green");//传入数组项目
    //只有一个项时，数值被认为是数组长度，和其他类型是数组项
    var colors = Array(3);//可省略"new"
    ```

  * 数组字面量

    ```javascript
    var colors = ["red","blue","green"];
    var value = [1,2,3,] //注意" ，" 错误❌
    ```

* 读取和设置数组

  * 方括号

    ```javascript
    var colors = ["red", "blue", "green"] 
    alert(colors[0]); // 显示第一项
    colors[2] = "black"; // 修改第三项
    colors[3] = "brown"; // 新增第四项
    ```

  * length属性

    length不是只读的，可通过length属性从数组末尾移除项或向数组添加新项

    ```JavaScript
    var colors = ["red", "blue", "green"] 
    colors.length = 2; //删除第三项
    colors.length = 4; //新增第三第四两项，值为undefined
    colors[colors.length] = "black";//数组末尾添加新项
    ```

* 检测数组

  * instanceof操作符

    instanceof它假设只有一个全局执行环境，多个框架情况下判断不准确。

    ```	javascript
    if(value instanceof Array){
      //数组操作
    }
    ```

  * Array.isArray()方法

    ECMAScript5新增 IE9+

    ```javascript
    if (Array.isArray(value)){
    //对数组执行某些操作
    }
    ```

* 转换方法

  * toString()
  * toLocaleString()
  * valueOf()
  * join("-")

* 栈方法

  * push()
  * pop()

* 队列方法

  * shift()
  * unshift()

* 重排序方法

  * reverse()
  * sort()

* 操作方法

  * concat()

    先创建当前数组一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。

  * slice()

    它能够基于当前数组中的一或多个项创建一个新数组。 接受一或两个参数，即要返回项的起始和结束位置。

  * splice()

    * 删除

      2 个参数：要删除的第一项的位置和要删除的项数“ splice(0,2)”

    * 插入

      3 个参数：起始位置、 0（要删除的项数）和要插入的项“splice(2,0,"red","green")”

    * 替换

      3 个参数：起始位置、要删除的项数和要插入的任意数量的项

      splice (2,1,"red","green")

* 位置方法

  * indexOf()

  * lastIndexOf()

    这两个方法都接收两个参数：要查找的项和表示查找起点位置的索引（可选）。

* 迭代方法

  每个方法都接收两个参数：要在每一项上运行的函数和（可选的）运行该函数的作用域对象——影响 this 的值。传入这些方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身。

  ```javascript
  var numbers = [1,2,3,4,5,4,3,2,1];
  var everyResult = numbers.every(function(item, index, array){
  	return (item > 2);
  });
  ```

  * every()

    对数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true。

  * filter()

    对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。

  * forEach()

    对数组中的每一项运行给定函数。这个方法没有返回值。

  * map()

    对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。

  * some()

    对数组中的每一项运行给定函数，如果该函数对任一项返回 true，则返回 true。

* 归并方法

   reduce()和 reduceRight()。这两个方法都会迭代数组的所有项，然后构建一个最终返回的值.

  接受两个参数：一个在每一项上调用的函数和（可选的）作为归并基础的初始值

  函数接收 4 个参数：前一个值、当前值、项的索引和数组对象。这
  个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第 一个参数是数组的第一项，第二个参数就是数组的第二项。

*   reduce()
  * reduceRight()

  ```javascript
  var values = [1,2,3,4,5];
  var sum = values.reduce(function(prev, cur, index, array){
  	return prev + cur;
  });
  alert(sum); //15
  ```

  ​


### Date类型

创建日期对象：

`var now = new Date();`

根据特定日期和时间创建日期对象：

*Date.pares()*	

> 接收一个表示日期的字符串参数，然后尝试根据这个字符串返回相应日期的毫秒数(参数格式因地区而异)
>
> `var someDate = new Date(Date.parse("May 25, 2004"));`
>
> 等价于`var someDate = new Date("May 25, 2004");`

*Date.UTC()*

> 参数分别是年份、基于 0的月份、月中的哪一天（1 到 31）、小时数（0 到 23）、分钟、秒以及毫秒数。只有前两个参数（年和月）是必需的。
>
> `var allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));`
>
> 等价于`var allFives = new Date(2005, 4, 5, 17, 55, 55);`

*Date.now()  *E5*

> 返回表示调用这个方法时的日期和时间的毫秒数.
>
> `var start = Date.now();`
>
> 不支持Date.now()的浏览器使用+操作符把Date对象转换成字符串
>
> `var start = +new Date();`

### RegExp类型

​	一个正则表达式就是一个模式与 3 个标志的组合体。

​	 `var expression = / pattern / flags ;`

1. 模式（pattern）部分可以是任何简单或复杂的正则表达式，可以包含字符类、限定符、分组、向前查找以及反向引用

2. 标志（flags）

   * g：全局（global）模式
   * i：不区分大小写模式
   * m：多行模式

3. 元字符

   `( [ { \ ^ $ | ) ? * + .]}`

   ```javascript
   // 匹配第一个"bat"或"cat"，不区分大小写
   var pattern1 = /[bc]at/i;
   //匹配所有"at"结尾的三个字符的组合，不区分大小写
   var pattern3 = /.at/gi;
   ```

4. 使用RegExp构造函数创建正则表达式

   > 接收两个参数：一个是要匹配的字符串模式，另一个是可选的标志字符串。参数都是字符串，所有需转义的元字符都必须双重转义。

* RegExp实例属性

  > RegExp 的每个实例都具有下列属性

  *  global：布尔值，表示是否设置了 g 标志。
  *  ignoreCase：布尔值，表示是否设置了 i 标志。
  *  lastIndex：整数，表示开始搜索下一个匹配项的字符位置，从 0 算起。


  *  multiline：布尔值，表示是否设置了 m 标志。


  *  source：正则表达式的字符串表示，按照字面量形式而非传入构造函数中的字符串模式返回

* RegExp实例方法

  * exec()

    > 该方法是专门为捕获组二设计的，

  * test()

* RegExp构造函数属性