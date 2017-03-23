## 对象

js的对象是无序属性的集合，其属性可以包含基本值、对象或者函数。是一组没有特定顺序的值。对象的每个属性或方法都有一个名字，而每个名字都映射到一个值。

### 理解对象

* 属性类型

  * 数据属性

    > [[Configurable]]

    > [[Enumerable]]

    > [[Writable]]

    > [[Value]]

  * 访问器属性

    > [[Configurable]]

    > [[Enumerable]]

    > [[Get]]

    > [[Set]]

  * 修改属性的特性：Object.defineProperty() 

    三个参数：属性所在对象，属性名，描述符对象

    ```javascript
    var person = {};
    Object.definProperty(person,"name",{
      writable:false,
      value:"Nicholas"
    })
    ```

    ​

* 定义多个属性

  ```javascript
  var book = {};
  Object.defineProperties(book,{
    _year:{
      value:2004
    },
    edition:{
      value:1
    },
    year:{
      get:function(){
        return this._year;
      },
      set:function(newValue){
        if(newValue>2004){
          this._year =newValue;
          this.edition += newValue - 2004;
        }
      }
    }
  })
  ```

  > 两个对象参数：要添加的属性所在对象，属性对象

* 读取属性的特性

  > Object.getOwnPropertyDescriptor()
  >
  > 两个参数：属性所在对象，要读取其描述符的属性名称

### 创建对象

* 工厂模式

  ```javascript
  function createPerson(name,age,job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
      alert(this.name);
    }
    return o;
  }
  var person1 = createPerson("Nicholas",29,"Software Engineer");
  var person2 = createPerson("chris",32,"Doctor");
  ```

* 构造函数模式

  ```javascript
  function Person(name,age,job){
    this.name=name;
    this.age=age;
    this.job=job;
    this.sayName = function(){
      alert(this.name);
    };
  }
  //当做构造函数使用
  var person1 = new Person("Nicholas",29,"Software Engineer");
  var person2 = new Person("chris",32,"Doctor");
  //作为普通函数调用
  Person("Greg",29,"Software Engineer"); // 添加到window
  window.sayName();//Greg
  //在另一个对象的作用域中调用
  var o =new object();
  Persion.call(o,"kristem",25,"kurse");
  o.sayName(); //kristem
  ```
  > 缺点：每个方法都要在每个实例上重新创建一遍

* 原型模式

  ```javascript
  function Person(){}
  Person.prototype.name = "Nicholas";
  Person.prototype.age = 29;
  Person.prototype.job = "terchear";
  Person.prototype.sayName = function(){
   alert(this.name) 
  };
  var person1 = new Person();
  person1.sayName();//Nicholas
  var person2 = new Person();
  person2.sayName();//Nicholas
  person1.sayName == person2.sayName; //true
  ```

  ​

  * 理解原型对象

    > 创建的每个函数都有一个prototype(原型)属性，这个属性是个指针，指向一个对象。这个对象是通过调用构造函数创建的对象实例的原型对象。使用原型对象可以让所有对象实例共享它所包含的属性和方法。
    >
    > 默认情况下，所有原型对象都会自动获取一个constructor（构造函数）属性，这个属性包含一个指向prototype属性所在函数的指针。

    isPrototypeOf() 方法 判断实例对象是否指向这个构造函数的原型对象

    ```javascript
    Person.prototype.isPrototypeOf(person1) //true
    Person.prototype.isPrototypeOf(person2) //true
    ```

    Object.getPrototypeOf() 方法[ES5] 

    ```javascript
    Object.getPrototypeOf(preson1)==Person.prototype //true
    Object.getPrototypeOf(preson1).name  //"Nicholas"
    ```
    > 读取对象属性时，搜索首先从对象实例本身开始，实例中没有继续搜索指针指向的原型对象.
    >
    > 在实例对象中可以屏蔽原型中的属性访问，恢复的时候删除实例中的属性就行

    hasOwnProperty()方法可检测属性是否存在于实例中，还是原型中。

    ```javascript
    person1.hasOwnProperty("name") //person1的属性返回true
    ```

    Object.getOwnPropertyDescriptor() 方法[es5] 只能用于实例属性

  * 原型与in操作符

    * 单独使用in操作符会在通过对象能够访问给定属性时返回true，无论该属性是否存在于实例原型中

    * hasOwnProperty() 

      只在属性存在于实例中时才返回true

    * for-in 循环，返回的是所有能够通过对象访问的、可枚举的（enumerated）属性，其中即包括存在于实例中的属性，也包括存在于原型中的属性。

    * Object.keys() [ESC5] 

      接受一个对象作为参数，返回一个包含所有可枚举属性的字符串数组

    * Object.getOwnPropertyNames()

      得到所有实例属性，无论是否可枚举

  * 更简单的原型语法

    > 用一个包含所有属性和方法的对象字面量来重写整个原型对象

    ```javascript
    function Person{
    }
    Person.prototype = {
      constructor:Person, //重新定义构造函数的指向，用Object.defineProperty()设置不可枚举
      name:"nicholas",
      age:29,
      job:"Software Engineer",
      sayName:function(){
        alert(this.name)
      }
    }
    ```

    此用法constructor属性不再指向Person，相当于重写了默认的prototype对象，因此constructor属性也变成了新对象的constructor（指向Object构造函数）

  * 原型的动态性

    由于原型中查找值的过程是一次搜索，因此对原型对象所做的任何修改都能够立即从实例上反应出来。

    重写原型对象切换现有原型与任何之前已经存在的对象实例之间的联系；他们引用的仍然是最初的原型。

  * 原型对象的原型

    所有原生引用类型（Object、Array、String）都在其构造函数的原型上定义了方法

  * 原型对象的问题

    原型中所有属性是被很多实例共享的，这种共享对函数非常适合，对于那些包含基本值得属性倒也说得过去，可以在实例中覆盖原型中的属性。对于包含引用类型值的属性（数组）来说，问题比较严重

* 组合构造函数和原型模式

  > 构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。结果，每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，最大限度地节省了内存。另外，这种混成模式还支持向构造函数传递参数；可谓是集两种模式之长。

  ```javascript
  function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["Shelby","Court"]
  }
  Person.prototype = {
    constructor : Person,
    sayName : function(){
      alert(this.name);
    }
  }
  var person1 = new Person("Nicholas", 29, "Software Engineer");
  var person2 = new Person("Greg", 27, "Doctor");
  person1.friends.push("Van");
  alert(person1.friends); //"Shelby,Count,Van"
  alert(person2.friends); //"Shelby,Count"
  alert(person1.friends === person2.friends); //false
  alert(person1.sayName === person2.sayName); //true
  ```

* 动态原型模式

  > 把所有信息都封装在了构造函数中，而通过在构造函数中初始化原型（仅在必要的情况下），又保持了同时使用构造函数和原型的优点。换句话说，可以通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型

  ```javascript
  function Person(name, age, job){
  	//属性
  	this.name = name;
  	this.age = age;
  	this.job = job;
    	//方法
  	if (typeof this.sayName != "function"){
  		Person.prototype.sayName = function(){
  		alert(this.name);
  		};
  	}
  }
  ```

* 寄生构造函数模式

  > 这种模式的基本思想是创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象

  ```javascript
  function Person(name, age, job){
      var o = new Object();
      o.name = name;
      o.age = age;
      o.job = job;
      o.sayName = function(){
      alert(this.name);
      };
      return o;
  }
  var friend = new Person("Nicholas", 29, "Software Engineer");
  friend.sayName(); //"Nicholas"
  ```

* 稳妥构造函数模式

  > 所谓稳妥对象，指的是没有公共属性，而且其方法也不引用 this 的对象。稳妥对象最适合在一些安全的环境中（这些环境中会禁止使用 this 和 new），或者在防止数据被其他应用程序（如 Mashup程序）改动时使用。稳妥构造函数遵循与寄生构造函数类似的模式，但有两点不同：一是新创建对象的实例方法不引用 this；二是不使用 new 操作符调用构造函数

  ```javascript
  function Person(name, age, job){
      //创建要返回的对象
      var o = new Object();
      //可以在这里定义私有变量和函数
      //添加方法
      o.sayName = function(){
      	alert(name);
      };
      //返回对象
      return o;
  }
  var friend = Person("Nicholas", 29, "Software Engineer");
  friend.sayName(); //"Nicholas"
  ```

### 继承

* 原型链

  > 基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法.
  >
  > 构造函数、原型和实例的关系：
  >
  > > 每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针

  ```javascript
  function SuperType() {
  	this.property = true;
  }
  SuperType.prototype.getSuperValue = function() {
  	return this.property;
  };

  function SubType() {
  	this.subproperty = false;
  }
  //继承了 SuperType
  SubType.prototype = new SuperType();
  SubType.prototype.getSubValue = function() {
  	return this.subproperty;
  };
  var instance = new SubType();
  console.log(instance.getSuperValue());​
  ```

* 借用构造函数

* 组合继承

* 原型式继承

* 寄生式继承

* 寄生组合式继承