# 基本概念

## 语法

- 区分大小写

  ECMAScript 中的一切（变量、函数名和操作符）都区分大小写。

  HTML和CSS中不区分大小写。

  js在HTML中当作HTML标签的**属性**使用时不区分大小写的，但属性内容要区分大小写

  ```html
  <div onclick="alert("aa")"></div>
  <div onClick="alert("aa")"></div>
  //上面两种写法没有区别
  <div data-type="鸟类">喜鹊</div>
  <div data-Type="鸟类">喜鹊</div>
  //取值时只能用小写取值
  ```

  在HTML中涉及到中划线(-)命名的属性时，“-”后面的不管大小写，取值时都只能用小写去取。

- 标识符

  所谓标识符，就是指变量、函数、属性的名字，或者函数的参数。

  * 第一个字符必须是一个字母(**包含任何语言**汉字)、下划线（_）或一个美元符号（$）

- 注释

- 严格模式

  `"use strict";`

  最好在函数模式下使用严格模式，防止压缩后全局都成了严格模式

- 语句

  * 分号问题：

    js解释器处理没有分号语句的原则：

    **先尝试放在一起，不行再加分号，还不行就报错。**

    **利：**if语句条件可以换行显示提高可读性，

    **弊：**如果第二行是"()" 开始就有可能被当成函数执行

    可以以“；”分号开头，可以避免跟其他代码发生未知错误。

    **例外:** 

    1. return break continue 时不会尝试和下面代码合并执行。
    2. ++ --会根据后面语句一起执行

    ```js
    a=1
    b=2
    a
    ++
    b
    // 执行结果如下
    a=1;
    b=2;
    a;
    ++b;
    ```

  * 括号问题：

    能用就用

## 关键字和保留字

避免关键字方法

1. 全记住
2. 驼峰命名
3. 拼音

## 变量

```js
var message = "hi"
,found = false
,age = 29;
// 建议逗号写前面
```



1. 初始化变量并不会定义变量类型，初始化的过程就是给变量赋一个值那么简单。**运行时才会动态取变量类型**。
2. 使用var局部变量 ，不使用var 成为全局变量