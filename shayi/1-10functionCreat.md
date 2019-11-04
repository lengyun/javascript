# 函数的创建

对象是很多属性和方法的合集。函数是一个特殊对象，函数在js中是代码的合集。

把一段代码抽象成一个功能模块，然后把它定义出来，这就是一个函数。

**函数特点：** 一处定义多处执行。

**函数的参数化：**在函数的定义和使用的过程中是可以传递参数到函数中。传递参数的这个过程就叫做函数的参数化。函数定义的时候包含的参数叫**形参**，形参在函数定义的时候是需要传递给函数的。而它在函数的使用过程当中，也就是在函数的函数体内部是以**局部变量的形式存在**的。在函数真正运行的时候接收到的参数叫**实参**

* 函数声明语句

  function  函数名  (形参) { 函数体，函数运行时要执行的代码}

  结尾不需要使用分号 ；

* 匿名函数表达式语句

  var a = function (形参) {}

  相当于创建没有名字的函数（匿名函数），同时把这个函数赋值给一个变量、参数或者属性。

  * 把函数当成一个值，赋值给一个变量。
  * 创建一个对象，并且在这个对象我们需要创建一个方法的时候。
  * 当一个函数作为参数传递给另外一个函数，或者作为回调函数使用的时候。

  **不同点：**

  函数声明语句，整个函数都会**声明提前**。

  函数表达式语句，只会把声明的变量提前。

  **语句块**中定义函数：严格模式下不允许使用函数声明语句定义,浏览器兼容性问题。可以使用匿名函数表达式语句定义函数

  ```js
  // 不同情况下对函数就行修改
  if(borrel){
      function fun1(){} // 错误的
      var aa = function (){}
  }else{
  	function fun1(){} // 错误的
      var aa = function (){}
  }
  ```

* 命名函数表达式语句

  var a = function 函数名 (形参) {函数体}

  与匿名函数表达式的区别：

  1. 命名函数有个name属性 ，属性的值就是函数的名字。

  2. 命名函数表达式的函数体中有一个和函数名称相同的变量，这个变量在函数体中任何地方都可以访问到的，而在函数外面是访问不到的。

     > 这个特性在使用递归函数的时候非常有用。针对调试时可以定位到报错的函数。

     ```js
     var a = function b (){ 
         return typeof b  
         // 'function' 可以直接调用函数b本身，在函数外不能访问
     }
     ```

     在IE8中使用命名函数表达式时，命名函数后面使用到的命名函数的部分会被当作普通函数声明来解析，解析后又把这个函数当作一个值赋值给这个变量。这样容造成在所在作用域命名污染，可以把名字跟前面的变量定义相同的名字避免这个问题。

* ES6 箭头函数

  > 匿名函数的简写方式

  ```js
  ("参数")=>{"函数体"}
  // 参数只有一个时，省略（）
  "参数"=>{"函数体"}
  // 函数体只包含一个声明，省略大括号
  "参数" => "函数体"
  ```

  > 判断数组中是否包含0

  ```js
  // 普通写法
  var arr= [0,9,9,8,1,123,1]
  arr.some(function(item, index, array){
      return item ===0
  })
  // 箭头函数
  arr.some(item => item ===0)
  ```

  箭头函数不同特点：

  1. 箭头函数在创建的时候，时不会创建自己的上下文执行环境的
  2. 所有的匿名函数都可以用箭头函数表示，箭头函数的name属性时空字符串。
  3. 在箭头函数对象中是没有 arguments 对象的，其他所有的函数在定义的时候传进去的参数，最后表示出来的都是一个数组。这个数组表示出来就是arguments 对象。

* ES6函数生成器

  > 跟函数声明语句相比多了一个 *****，函数生成器没有return，有一个yield，类事return。return在函数中一般只有一个。函数生成器中可以有多个yield

  ```js
  //function * Name(obj){ console.log('sss')}
  function* fn() {
      console.log(1);
      //暂停！
      yield;
      //调用next方法继续执行
      console.log(2);
  }
  var iter = fn();
  iter.next(); //1 {value: undefined, done: false}
  iter.next(); //2 {value: undefined, done: true}
  ```

  函数生成器返回的不是一个函数，而是迭代器对象。当你要使用这个函数的时候需要先运行下函数生成器，然后把结果赋值给一个变量或者属性。当你完成了赋值的过程之后。你最后得到还是一个对象这是一个迭代器对象，这个迭代器对象有一个next方法，这个next方法是固定的，当你运行这个next方法的时候整个函数会从你函数生成器最顶上开始执行一个代码，一直执行到第一个yield，然后执行完成后把执行结果以一个对象的形式返还过来。这个返回过来的对象包括两项，第一项的叫做value，第二项的值叫done。value是yield后面的返回值，done是布尔类型，表示函数是否执行到了最后。执行过的为false，最后为true。

  主要用途可以保存当前的状态。

* 函数构造器

  > Function 构造函数可以接收任意数量的参数，但最后一个参数始终都被看成是函数体，而前面的参数则枚举出了新函数的参数。

  ```js
  var sum = new Function("num1", "num2", "return num1 + num2"); // 不推荐
  ```

  1. 写的复杂
  2. 会调用eveo函数
  3. 要在全局作用域下创建

**总结：**

* ES6函数生成器：

​	需要对某些状态进行保存，并且依次执行时使用。

* 箭头函数：

  匿名函数的另外一种写法。在闭包中使用this的时候，对外层的this指针进行引用的时候，推荐使用箭头函数。它不存在本身的上下文环境，可以减少代码量。	

* 声明函数：

  通常情况项如果只时创建一个函数，并且在后面需要调用的话，直接使用声明函数。声明卸载代码最顶上，逻辑清晰。

* 函数表达式:

  在代码块中如if else语句中需要对函数进行修改的时候需要使用函数表达式。给对象的某个属性赋值，而且赋值为函数即给对象创建方法的时候也要使用函数表达式。回调函数中也需要使用函数表达式。也可以用箭头函数简写。

* 命名函数表达式：

  代码调试的时候，匿名函数报错打出anonoymous，使用命名函数后可更清晰定位错误。可起一个跟变量相同的名字兼容IE8。可读性更好，调试也方便。

  使用递归函数的时候