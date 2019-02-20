---
title: ES6常用特性
date: 2018-12-05 17:31:13
tags: ES6
categories:  javascript
cover: /images/cover/ECMAScript6.jpg

---

ES6东西还不少，先记下这么多常用的

<!-- more -->

# 1、Default Parameters（默认参数） in ES6

---
```
//ES5我们这样定义默认参数
var link = function (height, color, url) {
    var height = height || 50;
    var color = color || 'red';
    var url = url || 'http://azat.co';
    ...
}
```
这样做是有一点小问题的，当默认参数为0时就暴露问题了，因为在JavaScript中，0表示false。
```
// ES6可以默认值写在函数声明里
var link = function(height = 50, color = 'red', url = 'http://azat.co') {
  ...
}
```

# 3.字符串的扩展
### 1、模板字符串
模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。
```
// 普通字符串
`In JavaScript '\n' is a line-feed.`

// 多行字符串
`In JavaScript this is
 not legal.`

console.log(`string text line 1
string text line 2`);

// 字符串中嵌入变量
let name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`
```
如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中。
可以使用trim方法消除它。
```
$('#list').html(`
<ul>
  <li>first</li>
  <li>second</li>
</ul>
`.trim());
```
大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性。
模板字符串之中还能调用函数。
```
let x = 1;
let y = 2;

`${x} + ${y} = ${x + y}`
// "1 + 2 = 3"

`${x} + ${y * 2} = ${x + y * 2}`
// "1 + 4 = 5"

function fn() {
  return "Hello World";
}

`foo ${fn()} bar`
// foo Hello World bar
```

# 4.Destructuring Assignment （解构赋值）

### 1、数组的解构赋值
ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。
以前，为变量赋值，只能直接指定值。
```
let a = 1;
let b = 2;
let c = 3;
```
ES6 允许写成下面这样。
```
let [a, b, c] = [1, 2, 3];
```
上面代码表示，可以从数组中提取值，按照对应位置，对变量赋值。
还可以这样
```
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```
如果等号的右边不是数组（或者严格地说，不是可遍历的结构，参见《Iterator》一章），那么将会报错。
```
// 报错
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};
```

---
解构赋值允许指定默认值。
```
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```

### 2、对象的解构赋值
数组的元素是按次序排列的，而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
```
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
```
如果变量名与属性名不一致，必须写成下面这样。
```
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'

let { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };
```
对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。
let { foo: baz } = { foo: "aaa", bar: "bbb" };
baz // "aaa"
foo // error: foo is not defined

## 未完待续...




More info:
 [vue-router 文档](https://router.vuejs.org/zh/guide/#html)

