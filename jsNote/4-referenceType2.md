### 基本包装类型

为了便于操作基本类型值，ECMAScript提供了三个特殊的引用类型：Boolean、Number和String。实际上，每当读取一个基本类型值得时候，后台就会创建一个对应的基本包装类型的对象，从而让我们能够调用一些方法来操作这些数据。

`var s1 = "some text";`
`var s2 = s1.substring(2);`

当第二行代码访问s1时，访问过程处于一种读取模式，也就是要从内存中读取这个字符串的值。而在读取模式中访问字符串时，后台会自动完成如下处理

（1）创建String类型的一个实例；

（2）在实例上调用指定的方法；

（3）销毁这个实例

```javascript
var s1 = new String("some text");
var s2 = s1.substring(2);
s1 = null;
```

引用类型与基本包装类型的主要区别是对象的生存期，自动创建的基本包装类型的对象，只存于一行代码的执行瞬间，然后立即被销毁。这意味着我们不能再运行时为基本类型值添加属性和方法。

* Boolean类型

  ```javascript
  var booleanObject = new Boolean(true);
  ```

  理解基本类型的布尔值与Boolean对象之间区别：

  布尔表达式中的所有对象都会被转化为true,typeof操作符对基本类型返回“Boolean”对引用类型返回“object”

* Number类型

  ```javascript
  var numberObject = new Number(10);
  ```

  * 继承方法：toString()方法传递表示基数的参数

  ```javascript
  var num = 10;
  num.toString(); //"10"
  num.toString(2); //"1010"
  ```

  * 将数值格式化为字符串的方法

    ``toFixed()`` 安照指定的小数位返回数值的字符串表示，自动四舍五入。

    ```toExponential()``` 返回以指数表示法（e表示法）表示的数值的字符串形式。也接收一个参数，而且该参数同样也是指定输出结果中的小数位数。

    ```toPrecision()``` 方法可能会返回固定大小（fixed）格式，也可能返回指数（exponential）格式；具体看哪种格式最合适，接受一个参数，表示数值的所有数字的位数（不包含指数部分）

    ```javascript
    var num = 10.005;
    alert(num.toFixed(2)); //"10.01"
    alert(num.toExponential(1)); //"1.0e+1"
    alert(num.toPrecision(1)); //"1e+1"
    alert(num.toPrecision(2)); //"10"
    alert(num.toPrecision(3)); //"10.0"
    alert(num.toPrecision(4)); //"10.01"
    ```

* String类型

  ```javascript
  var stringObject = new String("hello world");
  ```

  length属性，表示字符串中包含多个字符

  1.字符方法

  * charAt()

  * charCodeAt()

  * 方括号加数字索引访问字符串特定字符（IE8+）

    ```javascript
    var stringValue = "hello world";
    alert(stringValue.charAt(1)); //"e"
    alert(stringValue.charCodeAt(1)); //输出"101" "e"的字符编码
    alert(stringValue[1]); //"e"
    ```

  2.字符串操作方法

  * concat()  用于将一或多个字符串拼接起来，返回拼接后的新字符串同“+”

  * slice()

  * substr()

  * substring()

    这三个方法都返回被操作字符串的一个子字符串，而且都接受一个或两个参数。第一个参数指定子字符串的开始位置，第二个参数表示子字符串到哪里结束。具体说，slice() 和substring()的第二个参数指定的是子字符串最后一个字符后面的位置。而substr()的第二个参数指定的则是返回的字符个数。如果没有第二个参数，则将字符串的长度作为结束位置。

    ```javascript
    var stringValue = "hello world";
    alert(stringValue.slice(3)); //"lo world" 片
    alert(stringValue.substring(3)); //"lo world" 子串
    alert(stringValue.substr(3)); //"lo world"下部结构
    alert(stringValue.slice(3, 7)); //"lo w"
    alert(stringValue.substring(3,7)); //"lo w"
    alert(stringValue.substr(3, 7)); //"lo worl" 7个字符
    //第一个参数为负
    alert(stringValue.slice(-3)); //"rld" 长度11+(-3)=8
    alert(stringValue.substring(-3)); //"hello world" -3转为0
    alert(stringValue.substr(-3)); //"rld"长度11+(-3)=8
    //第二个参数为负
    alert(stringValue.slice(3, -4)); //"lo w" 长度11+(-4)=7取值为(3，7)
    alert(stringValue.substring(3, -4)); //"hel"-4转为0，小值会作为开始变为substring(0, 3)
    alert(stringValue.substr(3, -4)); //""（空字符串）-4转为0，返回0个字符所以为空
    ```

  3.字符串位置方法

  * indexOf()

  * lastIndexOf()

    这两个方法都是从一个字符串中搜索给定的子字符串，然后返回子字符串的位置，没找到返回-1。indexOf()从开头找，lastIndexOf()从尾找。

    可选的第二个参数，表示从字符串中的那个位置开始搜索。

  4.trim()方法ECMA5

  ​	会创建一个字符串的副本，删除前置及后缀的所有空格，然后返回结果

  5.字符串大小写转换方法

  * toLowerCase()   转换为小写
  * toLocaleLowerCase() 特定地区的实现。
  * toUpperCase()  转换为大写
  * toLocaleUpperCase() 特定地区的实现。

  6.**字符串的模式匹配方法**

  * match()

    查找字符串中特定的字符，并且如果找到的话，则返回这个字符

    只接受一个参数，要么是一个正则表达式，要么是一个RegExp对象

  * search()

    参数同上，返回字符串中第一个匹配项的索引；如果没有找到则返回-1，始终从字符串开头向后查找

  * replace()

    在字符串中用某些字符替换另一些字符

    两个参数，第一个参数可以是一个RegExp对象或者字符串，第二个参数可以是一个字符串或者一个函数。

  7.localeCompare()方法

     比较两个字符串，返回1、0、-1。

  ​	字符串在字母表中应该排在字符串参数之后，则返回一个正数

  8.fromCharCode()方法

     这个方法的任务是接收一或多个字符编码，然后将它们转换成一个字符串

  9.HTML方法

### 单体内置对象

* Global对象
  * URL编码方法
    * encodeURL() 

      主要用于整个 URI,不会对本身属于URL的特殊字符进行编码

    * encodeURIComponent() 

      主要用于对 URI 中的某一段，会对它发现的任何非标准字符进行编码

    * decodeURL() 解码encodeURL()

    * decodeURLComponent() 解码encodeURIComponent() 

  * eval()方法

    类似完整的 ECMAScript 解析器，非严格模式下被执行的代码与该执行环境相同的作用域链。

  * Global对象的属性

  * window对象
* Math对象
  * Math对象的属性

  * min()和max()方法

    用于确定一组数值中的最小值和最大值，可以接受任意多个数值参数

    ```javascript
    var max = Math.max(3,54,32,16);
    alert(max);//54
    var min = Math.min(3,54,32,16);
    alert(min);//3
    //获取数组中的最大和最小值
    var values = [1, 2, 3, 4, 5, 6, 7, 8];
    var max = Math.max.apply(Math,values);
    ```

  * 舍入方法

    Math.ceil()  执行向上舍入，即它总是将数值向上舍入为最接近的整数；

    Math.floor()  执行向下舍入，即它总是将数值向下舍入为最接近的整数

    Math.round() 执行标准舍入，即它总是将数值四舍五入为最接近的整数

  * random()方法

    返回大于等于 0 小于 1 的一个随机数

    从某个整数范围内随机选择一个值

    ```javascript
    //值 = Math.floor(Math.random() * 可能值的总数 + 第一个可能的值)
    var num = Math.floor(Math.random()*10+1);//1到10之间的数值

    function selectFrom(lowerValue,upperValue){
      var choices = upperValue - lowerValue+1;
      return Math.floor(Math.random() * choices + lowerValue);
    }
    var num = selectFrom(2,10);
    var colors = ["red", "green", "blue", "yellow", "black", "purple", "brown"];
    var color = colors[selectFrom(0, colors.length-1)];
    alert(color); // 可能是数组中包含的任何一个字符串
    ```

    ​

  * 其他方法

