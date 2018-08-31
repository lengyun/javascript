# 高级技巧

## 高级函数

函数是JavaScript中最有趣的部分之一。他们本质上是十分简单的过程话的，但也可以是非常复杂和动态的。一些额外的功能可以通过使用闭包来实现。此外，由于所有的函数都是对象，所以使用函数指针非常简单。这些令JavaScript函数不仅有趣而且强大。

### 1.安全的类型检测

JavaScript 内置的类型检测机制并非完全可靠。

typeof 操作符

instanceof 操作符

在任何值上调用 Object 原生的 toString()方法，都会返回一个[object NativeConstructorName]格式的字符串。每个类在内部都有一个[[Class]]属性，这个属性中就指定了上述字符串中的构造函数名。

由于原生数组的构造函数名与全局作用域无关，因此使用 toString()就能保证返回一致的值。

```js
function isArray(value){
	return Object.prototype.toString.call(value) == "[object Array]";
}
function isFunction(value){
	return Object.prototype.toString.call(value) == "[object Function]";
}
function isRegExp(value){
	return Object.prototype.toString.call(value) == "[object RegExp]";
}
```

这一技巧也广泛应用于检测原生 JSON 对象。 Object 的 toString()方法不能检测非原生构造函
数的构造函数名。因此，开发人员定义的任何构造函数都将返回[object Object]。有些 JavaScript 库会包含与下面类似的代码。

```js
var isNativeJSON = window.JSON && Object.prototype.toString.call(JSON) ==
"[object JSON]";
```

###2.作用域安全的构造函数

构造函数其实就是一个使用 new 操作符调用的函数。当使用 new 调用时，构造函数内用到的 this 对象会指向新创建的对象实例。

```js
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
}
var person = new Person("Nicholas", 29, "Software Engineer");
```

问题出在当没有使用 new操作符来调用该构造函数的情况上。由于该 this 对象是在运行时绑定的，所以直接调用 Person()，this 会映射到全局对象 window 上，导致错误对象属性的意外增加。

```js
var person = Person("Nicholas", 29, "Software Engineer");
alert(window.name); //"Nicholas"
alert(window.age); //29
alert(window.job); //"Software Engineer"
```

这个问题的解决方法就是创建一个作用域安全的构造函数。作用域安全的构造函数在进行任何更改前，首先确认 this 对象是正确类型的实例。如果不是，那么会创建新的实例并返回。

```js
function Person(name, age, job){
	if (this instanceof Person){
		this.name = name;
		this.age = age;
		this.job = job;
	} else {
		return new Person(name, age, job);
	}
}
var person1 = Person("Nicholas", 29, "Software Engineer");
alert(window.name); //""
alert(person1.name); //"Nicholas"
var person2 = new Person("Shelby", 34, "Ergonomist");
alert(person2.name); //"Shelby"
```

关于作用域安全的构造函数的贴心提示。实现这个模式后，你就锁定了可以调用构造函数的环境。如果你使用构造函数窃取模式的继承且不使用原型链，那么这个继承很可能被破坏。

```js
function Polygon(sides){
	if (this instanceof Polygon) {
        this.sides = sides;
        this.getArea = function(){
            return 0;
        };
	} else {
		return new Polygon(sides);
	}
}
function Rectangle(width, height){
	Polygon.call(this, 2);
    this.width = width;
    this.height = height;
    this.getArea = function(){
    	return this.width * this.height;
    };
}
//构造函数窃取结合使用原型链或者寄生组合则可以解决这个问题。
Rectangle.prototype = new Polygon();

var rect = new Rectangle(5, 10);
alert(rect.sides); //undefined
```



###3.惰性载入函数

因为浏览器之间行为的差异，多数 JavaScript 代码包含了大量的 if 语句，将执行引导到正确的代码中。即使只有一个 if 语句的代码，也肯定要比没有 if 语句的慢，所以如果 if 语句不必每次执行，那么代码可以运行地更快一些。

解决方案就是称之为惰性载入的技巧。惰性载入表示函数执行的分支仅会发生一次。有两种实现惰性载入的方式，

**第一种** 就是在函数被调用时再处理函数。

```js
function createXHR(){
    if (typeof XMLHttpRequest != "undefined"){
        createXHR = function(){
            return new XMLHttpRequest();
        };
    } else if (typeof ActiveXObject != "undefined"){
		createXHR = function(){
            if (typeof arguments.callee.activeXString != "string"){
            	var versions = ["MSXML2.XMLHttp.6.0", "MSXML2.XMLHttp.3.0", "MSXML2.XMLHttp"], i, len;
                for (i=0,len=versions.length; i < len; i++){
                    try {
                        new ActiveXObject(versions[i]);
                        arguments.callee.activeXString = versions[i];
                        break;
                    } catch (ex){
                    	//skip
                    }
				}
			}
			return new ActiveXObject(arguments.callee.activeXString);
		};
	} else {
		createXHR = function(){
			throw new Error("No XHR object available.");
		};
	}
	return createXHR();
}
```

> 在这个惰性载入的 createXHR()中， if 语句的每一个分支都会为 createXHR 变量赋值，有效覆盖了原有的函数。最后一步便是调用新赋的函数。下一次调用 createXHR()的时候，就会直接调用被分配的函数，这样就不用再次执行 if 语句了。

**第二种** 实现惰性载入的方式是在声明函数时就指定适当的函数。这样，第一次调用函数时就不会损失性能了，而在代码首次加载时会损失一点性能。

```js
var createXHR = (function(){
    if (typeof XMLHttpRequest != "undefined"){
        return function(){
        	return new XMLHttpRequest();
        };
    } else if (typeof ActiveXObject != "undefined"){
    	return function(){
            if (typeof arguments.callee.activeXString != "string"){
                var versions = ["MSXML2.XMLHttp.6.0", "MSXML2.XMLHttp.3.0", "MSXML2.XMLHttp"],
                i, len;
                for (i=0,len=versions.length; i < len; i++){
                    try {
                        new ActiveXObject(versions[i]);
                        arguments.callee.activeXString = versions[i];
                        break;
                    } catch (ex){
                    //skip
                    }
                }
    		}
    		return new ActiveXObject(arguments.callee.activeXString);
    	};
    } else {
        return function(){
    		throw new Error("No XHR object available.");
    	};
    }
})();
```

这个例子中使用的技巧是创建一个匿名、自执行的函数，用以确定应该使用哪一个函数实现。实际的逻辑都一样。不一样的地方就是第一行代码（使用var定义函数）、新增了自执行的匿名函数，另外每个分支都返回正确的函数定义，以便立即将其赋值给createCHR()。

###4.函数绑定

函数绑定要创建一个函数，可以在特定的this环境中以指定参数调用另一个函数。该技巧常常和回调函数与事件处理程序一起使用，以便在将函数作为变量传递的同时保留代码执行环境。

###5.函数柯里化

## 防篡改对象

## 高级定时器

## 自定义事件



