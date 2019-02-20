---
title: let 与 const 的用法
data: 2018-12-05 10:28:37
tags: ES6
categories: javascript 
toc: true
cover: /images/cover/architecture-black-building.jpg

---
ES6 新增了两个声明两个声明标识符的方式： let 和 const。

<!-- more -->



 
- let 用来声明变量，并且会在当前作用域形成 代码块
- conts 用来声明常量，所谓常量就是物理指针不可以更改的变量。
   
## 1、let

使用 let 声明的变量，只能在当前代码块中访问和使用，有些类似于函数作用域，但是它又有几点不同的地方。

--------------------------


### let 声明变量，变量不会被提升。

var命令会发生”变量提升“现象，即变量可以在声明之前使用，值为undefined。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。
```
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;
 
// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```
-----------------------


### 不允许重复声明　

let不允许在相同作用域内，重复声明同一个变量。
```
// 报错
function func() {
  let a = 10;
  var a = 1;
}
 
// 报错
function func() {
  let a = 10;
  let a = 1;
}
```
因此，不能在函数内部重新声明参数
```
function func(arg) {
  let arg; // 报错
}
 
function func(arg) {
  {
    let arg; // 不报错
  }
}
```
-------------------


### 为什么需要块级作用域？
ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。

 第一种场景，内层变量可能会覆盖外层变量。
 ```
 var tmp = new Date();
 
function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}
 
f(); // undefined
```
上面代码的原意是，if代码块的外部使用外层的tmp变量，内部使用内层的tmp变量。但是，函数f执行后，输出结果为undefined，原因在于变量提升，导致内层的tmp变量覆盖了外层的tmp变量。

第二种场景，用来计数的循环变量泄露为全局变量。
```
var s = 'hello';
 
for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}
 
console.log(i); // 5
```
上面代码中，变量i只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。

---------------------


## 2、const　
const声明一个只读的常量。一旦声明，常量的值就不能改变。
```
const PI = 3.1415;
PI // 3.1415
 
PI = 3;
// TypeError: Assignment to constant variabl
```
上面代码表明改变常量的值会报错

const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值
```
const foo;
// SyntaxError: Missing initializer in const declaration
```
上面代码表示，对于const来说，只声明不赋值，就会报错

const的作用域与let命令相同：只在声明所在的块级作用域内有效。
```
if (true) {
  const MAX = 5;
}
 
MAX // Uncaught ReferenceError: MAX is not defined
```





More info: [let 和 const 命令](http://es6.ruanyifeng.com/?search=let&x=0&y=0#docs/let)
