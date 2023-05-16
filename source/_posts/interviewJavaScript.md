---
title: JavaScript面试题
# single_column: true
---

## 01. 变量赋值

JavaScript 中的基本数据类型包括字符串、数值、布尔值、undefined、Symbol。

JavaScript 中的引用数据类型包括对象、数组、函数、null。

```javascript
let m = 100;
let n = m;
m = 200;
console.log(n); // ?
```

基本数据类型在进行变量赋值时采用的是复制值的方式，n = m 其实就是将 m 变量的值 100 复制了一份给了变量 n，重新为 m 变量赋值时变量 n 不会受到影响，所以变量 n 的值依然是 100。

```javascript
let p1 = { age: 20 };
let p2 = p1;
p2.age = 21;
console.log(p1.age);
```

```javascript
let m = 10;
let n = 20;

function display(m, n) {
  m = 100;
  n = 200;
}

display(m, n);

console.log(m); // ?
console.log(n); // ?
```

复杂数据类型在进行变量赋值时采用的是复制地址的方式，p2 = p1 其实就是将变量 p1 对应的对象引用地址复制了一份给了变量 p2，此时 p1 变量和 p2 变量同时指向了同一个对象，所以无论是使用 p1 更改 age 还是使用 p2 更改 age，双方都会受到影响。

```javascript
const o1 = {
  x: 100,
  y: 200,
};
const o2 = o1;
let x = o1.x;
o2.x = 101;
x = 102;
console.log(o1.x);
```

```javascript
let p = { name: "张三" };

function person(p) {
  p.name = "李四";
}
person(p);

console.log(p.name); // ?
```

在 JavaScript 中，var、let、const 有什么区别？

`var`、`const`、`let` 是 JavaScript 中用于声明变量的关键字，它们之间的主要区别在于作用域和是否可重新赋值。

1. `var` 声明的变量具有函数作用域（function scope），这意味着它们在整个函数体内都可见。

   `var` 可以在任何时候重新赋值，也可以在声明之前使用（hoisted）。

```javascript
function testVar() {
  console.log(foo); // 输出 "undefined"，因为 var 变量会提升（hoisted）
  var foo = "Hello";
  console.log(foo); // 输出 "Hello"
  foo = "World";
  console.log(foo); // 输出 "World"
}
testVar();
```

2. `let` 声明的变量具有块级作用域（block scope），这意味着它们只在定义它们的代码块内可见。

   `let` 可以在任何时候重新赋值，但不能在声明之前使用。

```javascript
function testLet() {
  // console.log(bar); 在此时使用会报错，因为 let 变量不会提升
  let bar = "Hello";
  console.log(bar); // 输出 "Hello"
  bar = "World";
  console.log(bar); // 输出 "World"

  {
    let bar = "Inside block";
    console.log(bar); // 输出 "Inside block"，因为这是一个新的作用域
  }
  console.log(bar); // 输出 "World"
}
testLet();
```

3. `const` 用于声明一个常量，它具有与 `let` 相同的块级作用域，但在声明时必须初始化，且之后不能重新赋值。

```javascript
function testConst() {
  // console.log(baz); 在此时使用会报错，因为 const 变量不会提升
  const baz = "Hello";
  console.log(baz); // 输出 "Hello"
  // baz = "World"; // 如果尝试重新赋值，将会导致错误

  {
    const baz = "Inside block";
    console.log(baz); // 输出 "Inside block"，因为这是一个新的作用域
  }
  console.log(baz); // 输出 "Hello"
}
testConst();
```

在现代 JavaScript 开发中，一般建议尽量使用 `let` 和 `const`，以避免因 `var` 变量提升和函数作用域可能导致的意外行为。

## 02. typeof 运算符

typeof 运算符可以识别所有的基本数据类型、函数、可以识别是否是引用数据类型。

```javascript
let str = "Hello JavaScript";
let m = 100;
let isMarry = true;
let unique = Symbol("unique");
let n = undefined;
function fn() {}
let arr = ["a", "b"];
let obj = { name: "张三" };
let empty = null;

console.log(typeof str); // "string"
console.log(typeof m); // "number"
console.log(typeof isMarry); // "boolean"
console.log(typeof unique); // "symbol"
console.log(typeof n); // "undefined"
console.log(typeof fn); // "function"
console.log(typeof arr); // "object"
console.log(typeof obj); // "object"
console.log(typeof null); // "object"
```

## 03. 深拷贝

```javascript
function deepClone(target) {
  // 如果要拷贝的数据不是引用数据类型或要拷贝的数据为 null
  // 表示不需要深拷贝, 直接返回该数据即可
  if (typeof target !== "object" || target === null) return target;
  // 创建新的变量用于保存拷贝结果 区分要拷贝的数据是数组还是对象
  let result = target instanceof Array ? [] : {};
  // 遍历要拷贝的数据
  for (let key in target) {
    // 拷贝时排除原型对象中的属性
    if (target.hasOwnProperty(key)) {
      // 递归拷贝
      result[key] = deepClone(target[key]);
    }
  }
  // 返回拷贝结果
  return result;
}
```

```javascript
let obj1 = {
  a: 1,
  p: {
    name: "张三",
  },
};

let obj2 = deepClone(obj1);
console.log(obj1 === obj2);
console.log(obj2);
obj2.p.name = "李四";
console.log(obj1.p.name);
```

编写一个方法用于比较两个对象是否深度相等。

在 JavaScript 中，可以使用递归方法来实现深度比较两个对象是否相等。

```javascript
function isEqual(obj1, obj2) {
  if (obj1 === obj2) {
    return true;
  }

  if (
    typeof obj1 !== "object" ||
    typeof obj2 !== "object" ||
    obj1 === null ||
    obj2 === null
  ) {
    return false;
  }

  const keys1 = Object.keys(obj1);
  const keys2 = Object.keys(obj2);

  if (keys1.length !== keys2.length) {
    return false;
  }

  for (const key of keys1) {
    if (!keys2.includes(key)) {
      return false;
    }

    if (!isEqual(obj1[key], obj2[key])) {
      return false;
    }
  }

  return true;
}
```

这个 `isEqual` 方法首先比较两个对象的引用是否相等，如果相等则返回 true。然后，检查两个对象是否都是对象类型，如果不是，则返回 false。接下来，比较两个对象的键的数量，如果不相等，则返回 false。最后，使用递归方法逐个比较键值，如果存在不相等的键值，则返回 false。如果所有键值都相等，则返回 true。

## 04. 数据类型转换规则

① 其他类型的值转换为字符串

```javascript
String(null); // "null"
String(undefined); // "undefined"
String(true); // "true"
String(false); // "false"
String(10); // "10"
// 数组转为字符串是将数组中的所有元素按照 "," 连接起来
// 相当于调用数组的 Array.prototype.join(",") 方法
// 如 [1, 2, 3] 转为 "1,2,3"
// 空数组 [] 转为空字符串
// 数组中的 null 或 undefined, 会被当做空字符串处理
String([1, 2, 3]); // "1,2,3"
String([]); // ""
String([null]); // ""
String([1, undefined, 3]); // "1,,3"
// 普通对象转为字符串相当于直接使用 Object.prototype.toString() 方法
String({}); // "[object Object]"
```

② 其他类型的值转换为数值

```javascript
Number(null); // 0
Number(undefined); // NaN
Number("10"); // 10
Number("10px"); // NaN
Number(""); // 0
Number(true); // 1
Number(false); // 0
// 数组会先被转换为原始类型, 然后再将转换后的原始类型转换为数值类型
Number([]); // 0
Number(["10"]); // 10
Number(["10", "20"]); // NaN
Number({}); // NaN
```

③ 其他类型转换为布尔值

```javascript
Boolean(null); // false
Boolean(undefined); // false
Boolean(""); // false
Boolean(NaN); // false
Boolean(0); // false
Boolean([]); // true
Boolean({}); // true
Boolean(Infinity); // true
```

④ 对象、数组转换为原始类型

当对象类型需要被转为原始类型时，它会先查找对象的 valueOf 方法，如果 valueOf 方法返回原始类型的值，就使用这个值作为转换结果。

如果 valueOf 方法返回的不是原始类型的值，尝试调用对象的 toString 方法，使用该方法的返回值作为转换结果。

valueOf 方法用于返回对象的原始类型，一般由系统自动调用。

```javascript
// 创建字符串对象
let strObject = new String("Hello");
// 输出字符串对象
strObject; // String { 0: "H", 1: "e", 2: "l", 3: "l", 4: "o" }
// 输出它字符串对象的类型
typeof strObject; // "object"
// 获取字符串对象 strObject 的原始值
strObject.valueOf(); // "Hello"
```

```javascript
let object = {};
let array = [];

object.valueOf(); // {}
console.log(array.valueOf()); // []
```

```javascript
let object = {};
let array = [];

Object.prototype.valueOf = function () {
  return 100;
};

console.log(object.valueOf()); // 100
console.log(array.valueOf()); // 100
```

```javascript
// 将数组转换为数值类型
// 系统会先调用数组下的 valueOf 方法, 但是当前 valueOf 方法返回的就是数组, 所以系统去调用 toString 方法
// toString 方法调用之后返回了一个空字符串, 将空字符串转换为数值会得到 0
Number([]);
```

```javascript
// 在以下运算式中由于 + 号两边都不是数值, 所以要进行字符串连接操作
// 系统会先调用对象下的 valueOf 方法, 但是当前 valueOf 方法返回的就是对象, 所以系统去调用 toString 方法
// toString 方法调用后返回了 '[object Object]', 所以最终连接的结果就是 '[object Object][object Object]'
({}) + {};
```

## 05. 宽松比较中的隐式转换

在 JavaScript 中宽松比较会发生隐式类型转换，严格比较不会发生隐式类型转换。

① 布尔类型和其他类型的相等比较

布尔类型的值在参数比较时值会被转换为数值类型，true 转换为 1，false 转换为 0

```javascript
false == 0; // true
true == 1; // true
true == 2; // false
```

```javascript
const x = 10;
if (x == true) {
  console.log(x); // ? 是否会被输出
}
```

② 数值类型和字符串类型的相等比较

当数值类型和字符串类型做相等比较时，字符串类型会被转换为数值类型。

如果字符串为数值字符串，则将其转换为对应的数值，如果是空字符串转换为 0，其他一律转换为 NaN。

NaN 和任何值比较都不相等，包括它自己。

```javascript
0 == ""; // true
1 == "1"; // true
true == "1"; // true
false == "0"; // true
false == ""; // true
```

③ 对象类型和原始类型的相等比较

当对象类型和原始类型做相等比较时，对象类型要被转换为原始类型。

```javascript
"[object Object]" == {}; // true
"1,2,3" == [1, 2, 3]; // true
```

```javascript
// 在以下比较中, 数组需要被转换为原始类型, 系统先调用 valueOf 方法, 但返回值不是原始值
// 系统继续调用 toString 方法, 得到 "2"
// "2" == 2 比较, 字符串会被转换为数值 2, 所以 2 == 2 比较, 得到的结果是 true
[2] == 2;
```

```javascript
[null] == 0 // true
[undefined] == 0 // true
[] == 0 // true

```

```javascript
// "" == false
// "" == 0
// 0 == 0
[] == ![]; // true
```

```javascript
// "[object Object]" == false
// "[object Object]" == 0
// NaN == 0
{} == !{} // false

```

```javascript
// 当两个操作数都是对象时，JavaScript 会比较其内存中的引用地址
[] == []
{} == {}

```

④ null、undefined 和其他类型的比较

null 和 undefined 宽松相等的结果为 true，null 和 null 相等，undefined 和 undefined 相等。

null 和 undefined 和其他值比较时都不相等。

null 和 undefined 都是假值。

```javascript
null == undefined; // true
null == false; // false
undefined == false; // false
```

总结：在日常工作中进行相等比较时一律使用严格比较避免隐式类型转换产生的不可预知问题。

只有一种情况除外，当我们要判断对象中是否存在某一个属性时，可以使用宽松比较。

如果一个对象中的属性的值是 null 或是 undefined，我们都认为这个属性是不存在的。

```javascript
const object = {
  x: 100,
};
// 因为 x 无论是 null 还是 undefined, 和 null 进行比较时都会相等
if (object.x == null) {
  console.log("不存在");
} else {
  console.log("存在");
}
```

## 06. 运算中的隐式转换

在使用 + - \* / 进行运算时，数据类型会发生隐式类型转换。

\+ 号两边只要有一个运算数是字符串类型，其他运算数都会被转换成字符串类型。

除了 \+ 号以外的其他运算符，比如 - \* / 等都会将运算数转换为数值类型。

\+ 作为正号使用会将运算数转换为数值类型。

```javascript
"11" + 11;
"11" - 11;
11 + +"11";
```

## 07. 原型对象基本使用

在 JavaScript 中每一个构造函数都会配备一个名字叫做 prototype 的对象，我们称它为原型对象。

原型对象的作用是为了在实例对象之间进行属性共享。

```javascript
// Person 构造函数
function Person() {}
// Person 构造函数的原型对象
console.log(Person.prototype);
```

```javascript
// Person 构造函数
function Person(name) {
  this.name = name;
}

// 在 Person 构造函数的原型对象中添加 sayHello 方法
// 所有通过 Person 构造函数创建出来的实例对象都可以调用该方法
Person.prototype.sayHello = function () {
  alert(`Hello, 我是${this.name}`);
};

// 创建 p1 实例
const p1 = new Person("张三");
// 创建 p2 实例
const p2 = new Person("李四");

// 验证 p1 实例是否可以调用 sayHello 方法
p1.sayHello();
// 验证 p2 实例是否可以调用 sayHello 方法
p2.sayHello();
// 验证 p1 和 p2 调用的是否是同一个 sayHello 方法
alert(p1.sayHello === p2.sayHello);
```

<img src="./assets/03.png" align="left" width="50%"/>

实例对象在查找属性时，先在自身进行查找，自身如果找不到再去构造函数的原型对象中查找。

```javascript
p1.sayHello = function () {
  alert(`Hello, 我是${this.name}, 我是实例对象自身身上的 sayHello 方法`);
};

p1.sayHello();
p2.sayHello();
```

在实例对象身上有一个属性叫做 \_\_proto\_\_，它指向了实例的构造函数的原型对象，实例对象在查找属性时就是通过它找到的原型对象。

```javascript
alert(p1.__proto__ === Person.prototype);
```

\_\_proto\_\_ 被称之为隐式原型对象，实例会自动通过它去原型对象中查找，开发者不需要显式的去调用它。

```javascript
alert(p2.sayHello === p2.__proto__.sayHello);
```

在每一个原型对象中都会有一个名字叫做 constructor 的属性，该属性指向了构造函数。

```javascript
alert(Person.prototype.constructor === Person);
alert(p1.constructor === Person);
```

## 08. 原型对象进阶

Person.prototype 对象是 Object 构造函数的实例。

```javascript
// 以下代码不需要开发者编写
Person.prototype = new Object();
```

<img src="./assets/04.png" align="left" width="85%"/>

```javascript
// 此处 p1 调用的 hasOwnProperty 方法以及 toString 方法均来自 Object 构造函数的原型对象
alert(p1.hasOwnProperty("name"));
alert(p1.toString());
```

```typescript
function Person() {}
let p1 = new Person();
p1.__proto__ === Person.prototype; // true
Person.prototype.__proto__ === Object.prototype; // true
Object.prototype.__proto__ === null; // true
```

<img src="./assets/05.png" align="left" width="85%"/>

```javascript
function Person() {}
Person.__proto__ === Function.prototype; // true
Function.prototype.__proto__ === Object.prototype; // true
Object.prototype.__proto__ === null; // true
```

## 09. 继承

```javascript
function Person(name) {
  this.name = name;
}

function Student(name, number) {
  this.number = number;
}

const s1 = new Student("张三", 01);
```

```javascript
function Student(name, number) {
  // 继承父类实例属性
  Person.call(this, name);
}
// 继承父类原型属性
Student.prototype.__proto__ = Person.prototype;

const s1 = new Student("张三", 01);
```

```typescript
// ES6 语法糖
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log(`Hello, 我是${this.name}`);
  }
}

class Student extends Person {
  constructor(name, number) {
    super(name);
    this.number = number;
  }
  sayNumber() {
    console.log(`我是${this.name}, 我的学号是${this.number}`);
  }
}

console.log(typeof Person); // "function"
const tom = new Student("Tom", 1);
tom.sayHello();
tom.sayNumber();
```

## 10. instanceof

instanceof 运算符用于检查对象的类型，它返回一个布尔值，如果为真则表示该对象是特定类的实例，如果为假则不是。

```javascript
tom instanceof Student; // true
tom instanceof Person; // true
tom instanceof Object; // true
```

```javascript
[] instanceof Array   // true
[] instanceof Object  // true
{} instanceof Object  // true

```

## 11. 作用域

作用域是指变量和函数的可访问范围。

JavaScript 中有全局作用域、局部作用域和块级作用域。

(1) 全局作用域

在全局作用域中声明的变量和函数可以在代码中的任何位置被访问。

```javascript
// 全局作用域中声明变量
var globalVariable = "global variable";

// 全局作用域中声明函数
function globalFunction() {
  console.log("global function");
}

// 在函数中访问全局变量和函数
function myFunction() {
  console.log(globalVariable);
  globalFunction();
}

// 输出：global variable 和 global function
myFunction();
```

(2) 局部作用域

局部作用域是指在函数内部声明的变量和函数，只能在函数内部访问。

```javascript
function myFunction() {
  // 在函数内部声明局部变量
  var localVariable = "local variable";

  // 在函数内部声明局部函数
  function localFunction() {
    console.log("local function");
  }

  console.log(localVariable);
  localFunction();
}

myFunction(); // 输出：local variable 和 local function
console.log(localVariable); // 报错：localVariable is not defined
```

(3) 块级作用域

块级作用域是指在 if、for、while 等语句中产生的作用域，在其中声明的变量和函数只在该代码块内部有效。

在 ES6 中使用关键字 let 和 const 声明的变量具有块级作用域。

```javascript
// 使用 let 和 const 声明变量具有块级作用域
if (true) {
  let x = 1;
  const y = 2;
}
console.log(x); // 报错：x is not defined
console.log(y); // 报错：y is not defined
```

<img src="./assets/06.png" align="left" width="70%"/>

```javascript
var a = 100;
function print(fn) {
  var a = 200;
  fn();
}
function fn() {
  console.log(a);
}
print(fn);
```

```javascript
for (var i = 1; i <= 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 0);
}
```

```typescript
for (let i = 1; i <= 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 0);
}
```

```javascript
console.log(a);
var a = 12;
function fn() {
  console.log(a);
  var a = 13;
}
fn();
console.log(a);
```

```javascript
console.log(a);
var a = 12;
function fn() {
  console.log(a);
  a = 13;
}
fn();
console.log(a);
```

```javascript
console.log(a);
a = 12;
function fn() {
  console.log(a);
  a = 13;
}
fn();
console.log(a);
```

```javascript
var foo = 1;
function bar() {
  if (!foo) {
    var foo = 10;
  }
  console.log(foo);
}
bar();
```

```javascript
var foo = 1;
function bar() {
  // 不管条件是否成立都会进行变量提升 var foo = undefined;
  // if (!undefined) => 将 undefined 转换为布尔值再取反 !false => true
  if (!foo) {
    var foo = 10;
  }
  console.log(foo);
}
bar();
```

```javascript
var a = 10,
  b = 11,
  c = 13;

function test(a) {
  a = 1;
  var b = 2;
  c = 3;
}

test(10);

console.log(a);
console.log(b);
console.log(c);
```

```javascript
if (!("a" in window)) {
  var a = 10;
}
console.log(a);
```

```javascript
var a = 4;

function b(x, y, a) {
  console.log(a);
  // 在 JS 非严格模式下实参集合与形参变量存在映射关系, 严格模式下该关系比切断了
  arguments[2] = 10;
  console.log(a);
}

a = b(1, 2, 3);
console.log(a);
```

```javascript
var foo = "hello";
(function (foo) {
  console.log(foo);
  var foo = foo || "world";
  console.log(foo);
})(foo);
console.log(foo);
```

## 12. 闭包

闭包就是一个函数，一个引用了上级作用域链中的变量的函数，即使外部函数已不存在，也可以通过作用域链访问到外部函数中声明的变量。

```javascript
function outer() {
  var testVar = "test";
  return function () {
    console.log(testVar);
  };
}

var inner = outer();
inner();
```

使用闭包实现模块化开发

```javascript
var module = (function () {
  var privateVar = "这个变量是私有的";

  function privateFunction() {
    console.log("私有函数");
  }

  return {
    publicVar: "这个变量是公开的",
    publicFunction: function () {
      console.log("公开函数");
    },
  };
})();

console.log(module.publicVar);
module.publicFunction();
```

使用闭包实现计数器功能

```javascript
function counter() {
  var count = 0;
  return function () {
    count++;
    console.log(count);
  };
}

var countFunc = counter();
countFunc(); // 1
countFunc(); // 2
```

使用闭包实现缓存功能

```javascript
function cache() {
  var results = {};
  return function (key, val) {
    if (results[key]) {
      return results[key];
    } else {
      results[key] = val;
      return val;
    }
  };
}

var getResults = cache();
console.log(getResults("a", 1)); // 1
console.log(getResults("a", 2)); // 1
console.log(getResults("b", 2)); // 2
```

使用闭包实现一次性函数

```javascript
function singleUse() {
  var isUsed = false;
  return function () {
    if (!isUsed) {
      console.log("执行一次");
      isUsed = true;
    } else {
      console.log("无法执行！");
    }
  };
}

var useFunc = singleUse();
useFunc(); // 执行一次
useFunc(); // 无法执行！
```

面试题

```javascript
var n = 0;
function a() {
  var n = 10;
  function b() {
    n++;
    console.log(n);
  }
  b();
  return b;
}

var c = a();
c();
console.log(n);
```

## 13. this

JavaScript 中的 this 是一个指向当前执行环境的关键字，用来访问当前执行环境的上下文。

this 具体指向的对象要取决于当前调用方式。

(1) 默认绑定：函数调用时没有明确指定 this 的指向，或者使用的是独立的函数调用方式时，this 会绑定到全局对象上。

```js
function test() {
  console.log(this);
}
test(); // Window (浏览器) / global (Node.js)
```

(2) 隐式绑定：函数作为对象的属性调用时，this 会绑定到该对象。

```js
const obj = {
  name: "Alice",
  sayName() {
    console.log(this.name);
  },
};
obj.sayName(); // Alice
```

(3) 显式绑定：使用 apply、call、bind 等函数显式调用时，可以指定 this 的指向。

```js
function greet() {
  console.log(`Hello, ${this.name}!`);
}
const person1 = { name: "Alice" };
const person2 = { name: "Bob" };
greet.call(person1); // Hello, Alice!
greet.apply(person2); // Hello, Bob!
const boundGreet = greet.bind(person1); // bind 方法返回一个新函数
boundGreet(); // Hello, Alice!
```

(4) new 绑定：使用 new 关键字创建对象时，this 会绑定到新创建的对象上。

```js
function Person(name) {
  this.name = name;
}
const person = new Person("Alice");
console.log(person.name); // Alice
```

(5) 箭头函数：箭头函数没有自己的 this 绑定，会继承上一层作用域中的 this。

```js
const obj = {
  name: "Alice",
  logMyName: () => {
    console.log(this.name);
  },
};
obj.logMyName(); // undefined
```

需要注意的是，箭头函数中的 this 不可以被显式地绑定或修改，因为箭头函数根本没有自己的 this。

## 14. 手写 bind 方法

```javascript
// 创建一个自定义的bind函数
Function.prototype.myBind = function (context) {
  // 保存调用 myBind() 函数的函数对象
  var fn = this;
  // 获取 myBind() 函数调用时传入的参数，不包括第一个参数（即需要绑定的 this 值或上下文对象）
  var args = Array.prototype.slice.call(arguments, 1);
  // 返回一个新函数
  return function () {
    // 获取新函数调用时的参数
    var newArgs = Array.prototype.slice.call(arguments);
    // 将新函数的参数和调用myBind()函数时传入的参数合并
    var allArgs = args.concat(newArgs);
    // 返回调用原函数时的结果
    return fn.apply(context, allArgs);
  };
};

/*
  上述代码首先在 Function.prototype 上添加了一个 myBind 方法，以实现自定义的 bind 方法。
  当我们使用 myBind() 函数的时候，需要传入一个上下文对象（即需要绑定的 this 值），
  以及可以传入一系列参数，稍后会与新函数的参数合并。在myBind()函数中，先将调用myBind()
  函数的原函数对象保存下来，并获取传入myBind()函数的参数数组，去除第一个参数（即上下文对象）。然后返回一个新函数，
  新函数的作用是将调用新函数的参数与新函数之前已经传入myBind()函数中的参数合并起来，再通过 apply() 方法来调用原函数，
  同时将上下文对象和所有参数传递给apply()方法，确保原函数能够正确的使用上下文和参数。
*/
```

## 15. 获取元素索引

```html
<ul id="ul">
  <li>a</li>
  <li>b</li>
  <li>c</li>
</ul>
<script>
  // 获取 ul
  var ul = document.getElementById("ul");
  // 获取 ul 的所有子元素
  var lis = ul.children;
  // 获取 ul 子元素的个数
  var length = lis.length;
  // 遍历所有 li
  for (var i = 0; i < length; i++) {
    // 为 li 添加点击事件
    lis[i].onclick = function () {
      // 弹出 i 的值
      alert(i);
    };
  }
</script>
```

```javascript
// 解决办法一: 使用 let 关键字声明变量 i, 使其产生块级作用域
for (let i = 0; i < length; i++) {}
```

```javascript
// 解决办法二: 使用闭包使局部变量 i 不销毁
for (var i = 0; i < length; i++) {
  (function (i) {
    // 为 li 添加点击事件
    lis[i].onclick = function () {
      // 弹出 i 的值
      alert(i);
    };
  })(i);
}
```

## 16. 同步和异步

在 JavaScript 中，同步和异步是指代码的执行方式。

同步代码会按照代码的书写顺序一行一行地执行，每行代码必须等待前一行代码执行完成后才能执行。

在同步代码中如果某个操作需要执行的时间较长，代码的执行会被阻塞，直到操作完成才能继续执行下一行代码。

异步代码则是在某些代码执行完成前(ajax 请求、定时器等) ，允许继续执行其他的代码。

异步代码的执行结果不会立即得到，异步操作完成后再通过回调函数获取执行结果。

```javascript
// 同步代码
console.log("Step 1");
console.log("Step 2");
console.log("Step 3");
```

```
Step 1
Step 2
Step 3

```

```javascript
// 异步代码
console.log("Step 1");
setTimeout(function () {
  console.log("Step 2");
}, 1000);
console.log("Step 3");
```

```
Step 1
Step 3
Step 2

```

JavaScript 是单线的，这意味着它只能在一个时间点执行一个任务，无法同时执行多个任务。

如果某一个任务的执行需要耗费比较长的时间，那么程序将会被卡住，所以异步应运而生。

```javascript
console.log(1);
setTimeout(() => {
  console.log(2);
}, 1000);
console.log(3);
setTimeout(() => {
  console.log(4);
}, 0);
console.log(5);
```

## 17. JavaScript 运行原理

(1) JavaScript 为单线程语言

JavaScript 是单线程语言，这意味着 JavaScript 在同一个时间点只能执行一项任务，该项任务未完成之前其他任务需要等待。

```javascript
console.log("开始任务");

// 同步任务
function sleep(ms) {
  const start = new Date().getTime();
  while (new Date().getTime() < start + ms) {}
}

sleep(2000);
console.log("同步任务完成");

console.log("结束任务");
```

(2) JavaScript 是如何实现异步的?

既然 JavaScript 是单线程的，那么它是怎样实现非阻塞(异步)的呢?

上面我们所说的 JavaScript 指的是 EcmaScript，所有 EcmaScript 代码都在同一个线程中执行，这个单线程通常我们称它为主线程。

但是当 JavaScript 在浏览器中运行时，它不仅包含了 EcmaScript 还包含了浏览器提供的 web API，比如 window、document、setInterval、fetch 等，浏览器提供的 web API 不能在主线程中执行，浏览器提供了另外的线程供它们执行。

所以更加准确的说，JavaScript 本身可以只在一个线程中执行，但是运行在浏览器中的 JavaScript 由多个线程共同的执行。

<img src="./assets/07.png" align="left" width="65%"/>

```javascript
console.log("Learning");
console.log("About");
console.log("The Event Loop");
```

```javascript
const recursion = () => {
  recursion();
};
recursion();
```

<img src="./assets/08.png" align="left" width="65%"/>

```javascript
console.log("🐹");
setTimeout(() => console.log("🐹🐹"), 0);
console.log("🐹🐹🐹");
```

```typescript
// 如果调用堆栈为空并且任务队列中有待执行任务
// 依次将任务函数移动到调用堆栈中执行
if (callStack.isEmpty && taskQueue.length) {
  eventLoop();
}
```

(3) 宏任务与微任务

宏任务包括 script（整体代码），setTimeout, setInterval, setImmediate, requestAnimationFrame 等。

微任务包括 Promise.then/catch/finally、process.nextTick (NodeJS)、MutationObserver 等。

微任务执行的优先级高于宏任务，因为微任务都是 ECMAScript，宏任务都是 WebAPI。

```javascript
console.log("开始执行");

setTimeout(() => {
  console.log("setTimeout");
}, 0);

Promise.resolve()
  .then(() => {
    console.log("Promise 1");
  })
  .then(() => {
    console.log("Promise 2");
  });

console.log("结束执行");
```

## 18. Promise

JavaScript 的异步编程中最常见的问题之一就是回调地狱。

即一个异步操作完成后，需要执行另一个异步操作，而这个异步操作完成后又需要执行另一个异步操作，如此往复嵌套下去，代码变得十分冗长和难以维护。

```javascript
getData(function (data1) {
  getMoreData(data1, function (data2) {
    getMoreData(data2, function (data3) {
      getMoreData(data3, function (data4) {
        // 继续嵌套下去...
      });
    });
  });
});
```

为了避免回调地狱的问题，我们可以使用 Promise 来简化异步调用链。

```javascript
getData()
  .then((data1) => getData(data1))
  .then((data2) => getData(data2))
  .then((data3) => getData(data3))
  .then((data4) => getData(data4))
  .catch((error) => {});
```

Promise 解决回调地狱问题，它是一种用于异步编程的语法结构，可以使异步操作变得更加简单、易读和可维护。

```javascript
function getData(url) {
  return new Promise((resolve, reject) => {
    let xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.onload = function () {
      if (xhr.status === 200) {
        resolve(xhr.response);
      } else {
        reject(Error("请求失败，错误码：" + xhr.statusText));
      }
    };
    xhr.onerror = () => reject(Error("网络请求错误"));
    xhr.send();
  });
}

getData("https://jsonplaceholder.typicode.com/todos/1")
  .then((response) => console.log("成功获取数据：", response))
  .catch((error) => console.error("获取数据失败：", error));
```

封装 loadImage 方法使用 Promise 加载图片。

```javascript
function loadImage(url) {
  return new Promise((resolve, reject) => {
    const img = new Image();
    img.onload = () => resolve(img);
    img.onerror = (err) => reject(err);
    img.src = url;
  });
}

// https://img95.699pic.com/photo/50046/5562.jpg_wh300.jpg
// https://img95.699pic.com/photo/50136/1351.jpg_wh300.jpg

// 使用方法
loadImage("https://img95.699pic.com/photo/50136/1351.jpg_wh300.jpg")
  .then((img) => {
    // 加载成功
    console.log("Image loaded");
  })
  .catch((err) => {
    // 加载失败
    console.error(err);
  });
```

题一：Promise 的基本用法是什么？如何使用 Promise 处理异步操作？

答：Promise 是用于异步编程的一种解决方案，它可以更加优雅和可读地处理异步操作。Promise 有三种状态：Pending（等待态）、Resolved（成功态）和 Rejected（失败态）。在 Promise 构造函数中，可以传入一个 executor（执行器）函数，该函数接受两个参数，resolve 和 reject，代表 Promise 的成功和失败。在 executor 函数中，我们可以执行异步操作，并在操作成功或失败时调用相应的回调函数，例如：

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    if (Math.random() > 0.5) {
      resolve("success");
    } else {
      reject("failure");
    }
  }, 1000);
});

promise
  .then((result) => {
    console.log("Promise resolved: " + result);
  })
  .catch((error) => {
    console.log("Promise rejected: " + error);
  });
```

题二：如何使用 Promise.all 处理多个异步操作？

答：Promise.all 方法接受一个 Promise 对象数组作为参数，将返回一个新的 Promise 对象。只有当数组中所有 Promise 对象都成功时，该 Promise 对象才会被 resolved；如果数组中任一个 Promise 对象失败，该 Promise 对象立即被 rejected，并返回失败的原因。

```javascript
const promises = [
  new Promise((resolve) => setTimeout(() => resolve("a"), 1000)),
  new Promise((resolve) => setTimeout(() => resolve("b"), 2000)),
  new Promise((resolve) => setTimeout(() => resolve("c"), 3000)),
];

Promise.all(promises)
  .then((results) => {
    console.log("Promise.all resolved: " + results);
  })
  .catch((error) => {
    console.log("Promise.all rejected: " + error);
  });
```

题三：如何使用 Promise.race 处理多个异步操作？

答：Promise.race 方法接受一个 Promise 对象数组作为参数，将返回一个新的 Promise 对象。只要有一个 Promise 对象率先改变状态，该 Promise 对象就会采用第一个率先改变状态的 Promise 对象的状态。

```javascript
const promises = [
  new Promise((resolve) => setTimeout(() => resolve("a"), 1000)),
  new Promise((resolve) => setTimeout(() => resolve("b"), 2000)),
  new Promise((resolve) => setTimeout(() => resolve("c"), 3000)),
];

Promise.race(promises)
  .then((result) => {
    console.log("Promise.race resolved: " + result);
  })
  .catch((error) => {
    console.log("Promise.race rejected: " + error);
  });
```

题四：说出输出结果。

```javascript
Promise.resolve()
  .then(() => {
    console.log(1);
  })
  .catch(() => {
    console.log(2);
  })
  .then(() => {
    console.log(3);
  });
```

```javascript
Promise.resolve()
  .then(() => {
    console.log(1);
    throw new Error();
  })
  .catch(() => {
    console.log(2);
  })
  .then(() => {
    console.log(3);
  });
```

```javascript
Promise.resolve()
  .then(() => {
    console.log(1);
    throw new Error();
  })
  .catch(() => {
    console.log(2);
  })
  .catch(() => {
    console.log(3);
  });
```

```javascript
const promise = new Promise((resolve, reject) => {
  console.log(1);
  console.log(2);
});
promise.then(() => {
  console.log(3);
});
console.log(4);
```

```javascript
const promise = new Promise((resolve, reject) => {
  console.log(1);
  resolve("success");
  console.log(2);
});
promise.then(() => {
  console.log(3);
});
console.log(4);
```

```javascript
const promise1 = new Promise((resolve, reject) => {
  console.log(0);
  resolve(3);
});
const promise2 = promise1.then((res) => {
  console.log(res);
});
console.log(1);
console.log(2);
```

```javascript
const fn = () =>
  new Promise((resolve, reject) => {
    console.log(1);
    resolve("success");
  });
fn().then((res) => {
  console.log(res);
});
console.log("start");
```

```javascript
console.log("start");
setTimeout(() => {
  console.log("time");
});
Promise.resolve().then(() => {
  console.log("resolve");
});
console.log("end");
```

```javascript
const promise = new Promise((resolve, reject) => {
  console.log(1);
  setTimeout(() => {
    console.log("timerStart");
    resolve("success");
    console.log("timerEnd");
  }, 0);
  console.log(2);
});
promise.then((res) => {
  console.log(res);
});
console.log(4);
```

```javascript
setTimeout(() => {
  console.log("timer1");
  setTimeout(() => {
    console.log("timer3");
  }, 0);
}, 0);
setTimeout(() => {
  console.log("timer2");
}, 0);
console.log("start");
```

```javascript
setTimeout(() => {
  console.log("timer1");
  Promise.resolve().then(() => {
    console.log("promise");
  });
}, 0);
setTimeout(() => {
  console.log("timer2");
}, 0);
console.log("start");
```

```javascript
Promise.resolve().then(() => {
  console.log("promise1");
  const timer2 = setTimeout(() => {
    console.log("timer2");
  }, 0);
});
const timer1 = setTimeout(() => {
  console.log("timer1");
  Promise.resolve().then(() => {
    console.log("promise2");
  });
}, 0);
console.log("start");
```

```javascript
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("success");
  }, 1000);
});
const promise2 = promise1.then(() => {
  throw new Error("error!!!");
});
console.log("promise1");
console.log("promise2");
setTimeout(() => {
  console.log("promise1");
  console.log("promise2");
}, 2000);
```

```javascript
const promise = new Promise((resolve, reject) => {
  resolve("success1");
  reject("error");
  resolve("success2");
});
promise
  .then((res) => {
    console.log("then: ", res);
  })
  .catch((err) => {
    console.log("catch: ", err);
  });
```

```javascript
const promise = new Promise((resolve, reject) => {
  reject("error");
  resolve("success2");
});
promise
  .then((res) => {
    console.log("then1: ", res);
  })
  .then((res) => {
    console.log("then2: ", res);
  })
  .catch((err) => {
    console.log("catch: ", err);
  })
  .then((res) => {
    console.log("then3: ", res);
  });
```

```javascript
// 透传
Promise.resolve(1).then(2).then(Promise.resolve(3)).then(console.log);
```

## 19. async await

(1) async/await 概述

async/await 是 ECMAScript 2017 中引入的新的异步编程特性。它是建立在 Promise 基础之上的，主要的不同是它提供了更为简单直接的语法机制来处理异步操作，并且可以让异步代码看起来像同步代码，这样可以提高代码的可读性和可维护性。

对于一个函数，只要在它的声明前面使用 async 关键字来修饰，那么它就可以使用 await 来等待异步操作的结果。以下是一个简单的例子：

```js
async function fetchRemoteData() {
  const response = await fetch("https://api.example.com/data");
  const data = await response.json();
  return data;
}
```

上述代码中，fetchRemoteData 函数声明了 async，这意味着它可以通过 await 等待异步函数 fetch 的返回值。fetchRemoteData 函数等待 fetch 函数的响应，然后再等待它的 JSON 内容，最后返回包含 JSON 消息的数据对象。

(2) async/await 原理

async 函数是 Promise 对象的语法糖，async 函数本身返回的是一个 Promise 对象，因此可以与其他 Promise 对象进行链式调用。

```js
async function asyncFunc1() {
  return "hello world";
}

async function asyncFunc2() {
  const result = await asyncFunc1();
  return result.toUpperCase();
}

asyncFunc2().then((res) => {
  console.log(res); // -> 'HELLO WORLD'
});
```

在 async 函数中使用 await 表达式时，它的作用就是将异步调用的结果分割开来，使得后面的代码可以在异步调用返回结果之后继续执行。

```js
async function demo () {
  console.log('step 1')
  const result = await fetch('https://api.example.com/data')
  console.log('step 2')
  return result
}

demo().then(...)

```

在以上例子中，当我们调用 demo() 函数时，第一行代码会打印一个“step 1”的消息，但注意此时代码并不会阻塞。随后，代码中的 await 表达式会发起一个异步调用，该调用需要一些时间来完成，我们称为“await point”。

(3) 异常处理

await 操作符也可以捕获异步操作产生的异常，就像 Promise.catch() 方法一样。因此，建议在 async 函数中使用 try-catch 块来处理可能出现的异常。以下是一个例子：

```js
async function divide (x, y) {
  try {
    const result = await Promise.resolve(x / y)
    return result
  } catch (err) {
    console.error('Error:', err)
  }
}

divide(10, 0).then(...)

```

(4) 异步函数代码执行顺序说明

```javascript
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}
async function async2() {
  console.log("async2");
}
async1();
console.log("start");
```

```javascript
function double(m) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(m * 2);
    }, 1000);
  });
}

const arr = [1, 2, 3];

(async function () {
  for (var i = 0; i < arr.length; i++) {
    let r = await double(arr[i]);
    console.log(r);
  }
  console.log(i);
})();

console.log("start");
```

## 20. 优化 DOM 操作性能

(1) 优化操作 DOM 的频率

操作 DOM 的次数越少，页面反应就越快。

比如，我们使用一个 for 循环向一个 ul 列表里添加 1000 个 li 元素，这会导致 1000 次 DOM 操作，对页面性能的影响很大。

```javascript
const list = document.querySelector("ul");

for (let i = 0; i < 1000; i++) {
  const li = document.createElement("li");
  li.textContent = `Item ${i}`;
  list.appendChild(li);
}
```

可以使用 DocumentFragment 来高效地添加大量的 DOM 元素。

DocumentFragment 是一个虚拟的节点，可以将多个节点添加到 DocumentFragment 中，再将它一次性添加到 DOM 中，减少 DOM 操作的次数。

```javascript
const list = document.querySelector("ul");
const fragment = document.createDocumentFragment();

for (let i = 0; i < 1000; i++) {
  const li = document.createElement("li");
  li.textContent = `Item ${i}`;
  fragment.appendChild(li);
}

list.appendChild(fragment);
```

(2) 缓存 DOM 元素的引用

每次获取 DOM 对象都会造成一定的性能浪费，所以我们可以使用变量来缓存 DOM 对象的引用。

```javascript
// 未优化
document.querySelector("#myButton").addEventListener("click", function () {
  document.querySelector("#myDiv").innerHTML = "Button clicked";
});

// 优化后
const myButton = document.querySelector("#myButton");
const myDiv = document.querySelector("#myDiv");

myButton.addEventListener("click", function () {
  myDiv.innerHTML = "Button clicked";
});
```

(3) 使用事件委托

添加事件监听器也会影响性能，我们可以使用事件委托的方式来减少事件监听器的数量。

事件委托就是利用事件冒泡，将事件监听器添加到父元素上，然后根据事件冒泡的原理，在父元素上捕获事件，从而减少事件监听器的数量。

```javascript
// 未优化
const listItems = document.querySelectorAll("li");

for (let i = 0; i < listItems.length; i++) {
  listItems[i].addEventListener("click", function () {
    console.log(`Item ${i} clicked`);
  });
}

// 优化后
const list = document.querySelector("ul");

list.addEventListener("click", function (e) {
  if (e.target.tagName === "LI") {
    console.log(`Item ${e.target.textContent} clicked`);
  }
});
```

(4) 使用 classList 代替 className

className 常用于添加、删除、替换元素的 CSS 类，但是每次操作都会重新渲染元素，影响性能。

所以我们可以使用 classList 来代替 className 属性。

```javascript
// 未优化
const myDiv = document.querySelector("#myDiv");
myDiv.className = "foo";

// 优化后
const myDiv = document.querySelector("#myDiv");
myDiv.classList.add("foo");
```

(5) 避免多次修改样式

在 JavaScript 代码中频繁修改样式也会影响性能，我们可以将多个样式合并成一个字符串，然后一次性修改。

```javascript
// 未优化
const myDiv = document.querySelector("#myDiv");

myDiv.style.backgroundColor = "red";
myDiv.style.color = "white";
myDiv.style.fontSize = "20px";

// 优化后
const myDiv = document.querySelector("#myDiv");
myDiv.style.cssText = "background-color: red; color: white; font-size: 20px;";
```

## 21. property 与 attribute

在 HTML 中，attribute 是指在 HTML 标签属性，property 是指 DOM 对象属性。

HTML 中的 attribute：

```html
<div id="myDiv" class="box" data-value="123">This is a box</div>
```

可以使用 JavaScript 获取 attribute 的值：

```javascript
const myDiv = document.getElementById("myDiv");
console.log(myDiv.getAttribute("data-value")); // "123"
```

也可以使用 JavaScript 动态设置 attribute 的值：

```javascript
myDiv.setAttribute("data-value", "456");
```

DOM 对象属性通过 DOM 对象点上属性的方式进行访问：

```javascript
const myDiv = document.getElementById("myDiv");
console.log(myDiv.id); // "myDiv"
console.log(myDiv.className); // "box"
console.log(myDiv.innerHTML); // "This is a box"
```

也可以通过赋值语句来修改它们的值：

```javascript
myDiv.className = "new-box";
myDiv.innerHTML = "This is a new box";
```

---

HTML 标签属性和 DOM 对象属性都是为 HTML 元素提供附加信息的方式。它们之间有一定的联系，但在使用和目的方面有所不同。

1. HTML 标签属性：

HTML 标签属性主要用于在 HTML 文档中为元素提附加信息。

例如，`id` 属性用于为一个 HTML 元素分配唯一的标识符，`class` 属性可以分配一个或多个类名，以便使用 CSS 进行样式设置。例如：

```html
<input
  type="text"
  id="username"
  class="form-control"
  placeholder="请输入用户名"
/>
```

2. DOM 对象属性：

DOM 即文档对象模型（Document Object Model），是一个编程接口，允许我们使用 JavaScript 操作 HTML 文档的结构、样式和内容。

浏览器在读取 HTML 标签时，会生成标签对应的 DOM 对象，标准的 HTML 标签属性会成为 DOM 对象属性。

DOM 对象属性是我们在 JavaScript 中与 HTML 元素互动时所使用的属性，它不仅包括 HTML 属性，还包括其他与元素相关的属性。例如：

```javascript
const element = document.getElementById("username");
console.log(element.tagName); // 输出元素的标签名称
element.disabled = true; // 将输入框设置为禁用状态
```

3. 不同点：

- HTML 标签属性仅仅存在于 HTML 文档中，而 DOM 对象属性是我们 在 JavaScript 中使用的属性。
- DOM 对象属性包含更多信息，除了标准的 HTML 标签属性以外还包括元素事件处理、元素宽高等。
- 有时，DOM 对象属性和 HTML 标签属性可能有区别。
  - `checked` 和 `selected` 在 HTML 中是布尔属性，但在 DOM 中是布尔值。
  - `value` 属性，HTML 标签的 value 属性始终存储的是设置属性时赋的默认值，而 DOM 对象中的 value 属性保存的是用户输入的内容。
  - `class` 属性用于在 HTML 标签中为元素设置类名，`className` 属性用于在 DOM 对象中设置元素类名。
  -
- 一些 HTML 标签属性没有对应的 DOM 对象属性, 比如 aria-\*, colspan 等
- 一些 DOM 对象属性没有对应的 HTML 标签属性, 比如 textContent 等

在实际项目中，你应该根据需要进行选择：

- 在 HTML 标签中，使用 HTML 属性定义元素的特征和基础数据。
- 在 JavaScript 代码中，通过操作 DOM 对象属性实现动态交互和修改元素状态。

代码示例：

```html
<!-- 设置HTML标签属性 -->
<button id="toggleBtn" class="btn btn-primary">切换禁用状态</button>
<input
  type="text"
  id="username"
  class="form-control"
  placeholder="请输入用户名"
/>

<script>
  // JavaScript访问和操作DOM对象属性
  const toggleBtn = document.getElementById("toggleBtn");
  const inputElement = document.getElementById("username");

  toggleBtn.addEventListener("click", () => {
    inputElement.disabled = !inputElement.disabled;
  });
</script>
```

上面的代码中，我们在 HTML 中设置了基本的属性，然后在 JavaScript 中操作 DOM 对象属性实现点击按钮时切换输入框的禁用状态。

## 22. 通用的绑定事件的方法

```javascript
function addEvent(el, eventType, callback) {
  if (el.addEventListener) {
    el.addEventListener(eventType, callback, false);
  } else if (el.attachEvent) {
    el.attachEvent("on" + eventType, callback);
  } else {
    el["on" + eventType] = callback;
  }
}
```

el 是要绑定事件的元素，eventType 是要绑定的事件类型，callback 是事件处理程序。

这个方法支持现代浏览器、IE8+及更低版本的 IE 浏览器，以及不支持 addEventListener 的其他浏览器。

说出 onLoad (window) 事件与 DOMContentLoaded (document) 事件的区别？

## 23. 事件冒泡

事件冒泡是指当一个元素上的事件被触发时，该事件会向父元素传递，如果父元素也绑定了该事件，则会接着触发父元素上的事件，以此类推直至到达最外层的祖先元素为止。

```html
<div id="outer">
  <div id="middle">
    <button id="inner">Click me!</button>
  </div>
</div>
```

```javascript
const outer = document.getElementById("outer");
outer.addEventListener("click", function (event) {
  console.log("Outer div clicked!");
});

const middle = document.getElementById("middle");
middle.addEventListener("click", function (event) {
  console.log("Middle div clicked!");
});

const inner = document.getElementById("inner");
inner.addEventListener("click", function (event) {
  console.log("Button clicked!");
});
```

当点击按钮时，控制台会打印以下内容：

```
Button clicked!
Middle div clicked!
Outer div clicked!

```

## 24. 事件委托

事件委托是指将事件处理程序绑定在某一元素的祖先元素上，通过祖先元素处理该事件，这能够优化性能并减少事件处理程序的数量。

下面是一个示例，当单击一个 ul 元素的下拉菜单项时，会执行相应的操作：

HTML 代码：

```html
<ul class="menu">
  <li>菜单项 1</li>
  <li>菜单项 2</li>
  <li>菜单项 3</li>
</ul>
```

JavaScript 代码：

```javascript
var menu = document.querySelector(".menu");
menu.addEventListener("click", function (event) {
  if (event.target.tagName === "LI") {
    console.log("你点击了菜单项：", event.target.textContent);
  }
});
```

## 25. Ajax

在原生 JavaScript 中，我们可以使用 XMLHttpRequest 对象发送 Ajax 请求。

一、概述

Ajax 是 Asynchronous JavaScript and XML 的缩写，它是一种使用 JavaScript 在不刷新页面的情况下与服务器交换数据的技术。

二、创建 XMLHttpRequest 对象

要使用 XMLHttpRequest，首先需要创建一个 XMLHttpRequest 对象的实例。

```javascript
var xhr = new XMLHttpRequest();
```

三、设置请求

当实例创建完毕后，可以使用 XMLHttpRequest 对象的 open() 方法来设置请求，open() 方法接受三个参数：

- 请求的类型（GET 或 POST）
- 请求的 URL
- 请求是否异步（true 表示异步，false 表示同步）

```javascript
xhr.open("GET", "https://api.example.com/data", true);
```

四、发送请求

设置好请求之后，可以使用 XMLHttpRequest 对象的 send() 方法来发送请求。

如果是 GET 请求，send() 方法不需要参数；

如果是 POST 请求，send() 方法需要传递一个字符串或 FormData 对象。

```javascript
xhr.send();
```

五、处理响应

要处理服务器返回的响应，可以使用 XMLHttpRequest 对象的 onreadystatechange 事件处理程序。

当请求的状态（readyState）发生变化时，onreadystatechange 事件处理程序将被触发。

readyState 的可能值包括：

- 0：请求未初始化（已创建 XMLHttpRequest 实例，但尚未调用 open() 方法）
- 1：请求已经设置（已调用 open() 方法，但尚未调用 send() 方法）
- 2：请求已发送（已调用 send() 方法，但尚未收到响应）
- 3：请求处理中（已收到部分响应数据）
- 4：请求已完成（已收到所有的响应数据）

当 readyState 等于 4 且 HTTP 状态码为 200 时，表示请求成功，此时可以获取并处理服务器返回的数据。

```javascript
var xhr = new XMLHttpRequest();

xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    console.log(JSON.parse(xhr.responseText));
  }
};

xhr.open("GET", "https://api.example.com/data", true);
xhr.send();
```

实际项目中的代码示例：

1. 获取 GitHub 用户信息：

```javascript
var xhr = new XMLHttpRequest();

xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    var user = JSON.parse(xhr.responseText);
    console.log("GitHub 用户名：" + user.login);
  }
};

xhr.open("GET", "https://api.github.com/users/octocat", true);
xhr.send();
```

2. 从 API 中获取一条随机笑话：

```javascript
var xhr = new XMLHttpRequest();

xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    var joke = JSON.parse(xhr.responseText);
    console.log("随机笑话：" + joke.value.joke);
  }
};

xhr.open("GET", "https://api.icndb.com/jokes/random", true);
xhr.send();
```

## 26. 跨域

(1) 跨域概述

跨域是指跨越原始访问控制(domains)的访问。

在 Web 应用中，浏览器出于安全考虑一般情况下是只允许同源请求。

所谓同源就是协议、域名以及端口号相同，如果请求的资源来自于不同的源（域名、端口、协议），则会出现跨域。

这种安全策略被称为 "同源策略"，同源策略的主要目的是为了保证用户信息的安全，阻止恶意攻击。

如果没有同源策略，攻击者可以通过编写脚本，从一个网站发送请求绕过用户认证，获取用户的隐私数据。

(2) 如何解决跨域问题

① JSONP

JSONP (JSON with Padding) 是一种通过 script 标签跨域访问数据的方式，因为 script 标签本身就允许跨域。

```javascript
// 客户端
function jsonp(url, callbackName) {
  const script = document.createElement("script");
  script.src = `${url}?callback=${callbackName}`;
  document.body.appendChild(script);
}

function handleData(data) {
  console.log(data);
}

jsonp("https://example.com/jsonp", "handleData");
```

```javascript
// 服务端
const http = require("http");
const url = require("url");

http
  .createServer(function (req, res) {
    const query = url.parse(req.url, true).query;
    const callback = query.callback;
    const data = {
      name: "Jack",
      age: 20,
    };

    res.writeHead(200, { "Content-Type": "application/javascript" });
    res.end(`${callback}(${JSON.stringify(data)})`);
  })
  .listen(3000);
```

需要注意的是，JSONP 总是发起一个 GET 请求，所以仅仅适用于请求数据，而不能发送数据。

② CORS (跨域资源共享)

CORS 是一种更为现代化的跨域方案，它允许浏览器与服务器之间进行跨域通信。

服务端需要在响应头中添加 CORS 相关配置，例如 Access-Control-Allow-Origin，这样客户端就可以跨域访问该资源。

```javascript
// 客户端
const xhr = new XMLHttpRequest();
xhr.open("GET", "https://example.com/api/data");
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    console.log(JSON.parse(xhr.responseText));
  }
};
xhr.send();
```

```javascript
// 服务端
const http = require("http");

http
  .createServer(function (req, res) {
    res.writeHead(200, {
      "Content-Type": "application/json",
      "Access-Control-Allow-Origin": "*", // "*" 表示允许任何域名跨域访问，也可以设置指定域名
    });

    const data = {
      name: "Lily",
      age: 22,
    };

    res.end(JSON.stringify(data));
  })
  .listen(3000);
```

除了以上两种方法，还有其他一些常用的跨域解决方案，如使用代理服务器、postMessage 通信等。

## 27. 本地存储

cookie 是浏览器保存用户数据的一种方式。

localStorage 和 sessionStorage 是 HTML5 引入的 Web Storage，同样是用于在浏览器中保存数据。

```javascript
// 设置cookie
document.cookie =
  "name=John Doe; expires=Thu, 18 Dec 2022 12:00:00 UTC; path=/";

// 获取cookie
let name = document.cookie;
console.log(name);
```

```javascript
// 存储数据
localStorage.setItem("name", "John Doe");
localStorage.setItem("age", "30");

// 获取数据
let name = localStorage.getItem("name");
let age = localStorage.getItem("age");

console.log(name, age);
```

```javascript
// 存储数据
sessionStorage.setItem("name", "John Doe");
sessionStorage.setItem("age", "30");

// 获取数据
let name = sessionStorage.getItem("name");
let age = sessionStorage.getItem("age");

console.log(name, age);
```

cookie 保存数据的容量较小，只有 4KB 左右，而 localStorage 和 sessionStorage 可以存储更多数据。

cookie 可以设置过期时间，不过需要手动管理，而 localStorage 和 sessionStorage 可以无限期保存数据。

cookie 可以跨域传递数据，而 localStorage 和 sessionStorage 只能在同源页面之间共享数据。

cookie 保存在浏览器的 cookie 文件夹中，localStorage 和 sessionStorage 保存在浏览器中的特定位置，更安全。

如果需要跨页面或跨域传递数据，或者需要在用户不主动清除浏览器缓存的情况下保存数据，建议使用 cookie。

如果需要在同源页面中保存少量数据，建议使用 localStorage。

如果需要在同源页面中保存临时数据，建议使用 sessionStorage。

## 28. HTTP 协议

(1) HTTP 协议概述

HTTP (Hypertext Transfer Protocol) 是一种在 Web 应用中进行数据通信的协议。

在客户端和服务器之间交换的所有数据（例如 HTML 文件、图像文件、查询结果等）都必须遵循 HTTP 协议中规定的格式。

HTTP 协议基于客户端服务器模型，客户端发送请求，服务器发送响应。

通常，Web 浏览器是作为客户端出现的。当您在浏览器中输入一个 URL 时，这实际上向服务器发出了一个请求，以获取网页。

服务器将响应包含在一个 HTTP 格式的消息中，并将其发送回给浏览器。浏览器将消息解析并显示响应的文本、图像等内容。

HTTP 格式的消息分为两类：请求和响应。

(2) HTTP 请求

HTTP 请求由三个部分组成：

1. 请求方法：指定对服务器执行的操作类型。常用方法包括 GET、POST、PUT、DELETE 等。
2. 请求 URI：指定要操作的资源的 URI。
3. HTTP 版本：该消息所使用的 HTTP 版本。

例如，以下是一个 HTTP GET 请求的示例：

```
GET /index.html HTTP/1.1
Host: www.example.com

```

这个请求由 GET 方法（请求获取资源）组成，请求的 URI 是 `/index.html`，HTTP 版本是 1.1。

请求消息可以包含其他信息，如下所示：

```
GET /search?q=example HTTP/1.1
Host: www.google.com
Accept-Encoding: gzip, deflate, br
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3

```

其中，Accept-Encoding 和 User-Agent 是请求首部。

(3) HTTP 响应

HTTP 响应也由三个部分组成：

1. 状态码：指定执行该请求后服务器的状态。常见的状态码包括 200 OK（请求成功）、404 Not Found（未找到请求的资源）等。
2. 响应首部：包含与响应相关的元数据，如服务器类型、响应时间等。
3. 实体：包含响应的实际内容，例如网页、图像等。

以下是一个 HTTP 响应的示例：

```
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 138
Date: Tue, 06 Jun 2017 09:45:32 GMT

<html>
<head>
<title>Welcome to Example.com</title>
</head>
<body>
<h1>Hello World!</h1>
</body>
</html>

```

状态码为 200（请求成功），并且响应实体中包含一个 HTML 页面。

(4) HTTP 方法

HTTP 协议支持多种请求方法。以下是一些常见的方法：

1. GET：请求获取资源。应该只用于获取数据，不应更改数据。
2. POST：请求向服务器提交数据。该请求可能导致服务器更改状态或执行其他操作。
3. PUT：请求更新服务器上的资源。
4. DELETE：请求删除服务器上的资源。

(5) HTTP 状态码

HTTP 响应状态码指示 HTTP 请求是否已成功完成。以下是一些常见的状态码及其含义：

1. 200 OK：请求已成功完成。
2. 201 Created：服务器已成功创建新资源。
3. 400 Bad Request：请求无效，例如缺少一些必需的参数。
4. 401 Unauthorized：未授权，需要身份验证。
5. 404 Not Found：请求的资源不存在。
6. 500 Internal Server Error：服务器遇到了错误，无法完成请求。

## 29. restful API

RESTful（Representational State Transfer）是一种软件架构风格，主要用于网络上的 web 服务。

RESTful API 按照资源进行设计，使用标准的 HTTP 方法，如 GET、POST、PUT 和 DELETE，实现资源的创建、读取、更新和删除操作（CRUD 操作）。

资源：RESTful API 中的资源可以是一篇文章、用户信息或者商品等，这些资源可以通过 URI（统一资源标识符）进行标识，例如：/users、/articles 等。

方法：RESTful API 中的方法是 HTTP 请求动词，例如：GET、POST、PUT、DELETE 等。不同的动词表示对资源的不同操作。

状态：资源的状态表示请求操作的结果，使用 HTTP 状态码来传达，如 200 表示操作成功，404 表示资源未找到，500 表示服务器内部错误等。

---

以下是一个使用 Node.js 的 Express 框架创建的简单 RESTful 风格的 API 的代码示例。

```javascript
// 导入模块
const express = require("express");
const app = express();

// 使用中间件
app.use(express.json());

// 资源：用户
const users = [];

// 方法：
// 1. 获取所有用户：GET /users
app.get("/users", (req, res) => {
  res.status(200).json(users);
});

// 2. 创建用户：POST /users
app.post("/users", (req, res) => {
  const newUser = {
    id: users.length + 1,
    name: req.body.name,
    age: req.body.age,
  };
  users.push(newUser);
  res.status(201).json(newUser);
});

// 3. 获取单个用户：GET /users/:id
app.get("/users/:id", (req, res) => {
  const user = users.find((u) => u.id === parseInt(req.params.id));
  if (!user) res.status(404).send("用户未找到。");
  res.status(200).json(user);
});

// 4. 修改用户信息：PUT /users/:id
app.put("/users/:id", (req, res) => {
  const user = users.find((u) => u.id === parseInt(req.params.id));
  if (!user) res.status(404).send("用户未找到。");

  user.name = req.body.name;
  user.age = req.body.age;
  res.status(200).json(user);
});

// 5. 删除用户：DELETE /users/:id
app.delete("/users/:id", (req, res) => {
  const user = users.find((u) => u.id === parseInt(req.params.id));
  if (!user) res.status(404).send("用户未找到。");

  const index = users.indexOf(user);
  users.splice(index, 1);

  res.status(204).send();
});

const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`Listening on port ${port}...`));
```

## 30. 数组 splice、slice

在 JavaScript 中，数组的 splice 和 slice 是两个用于操作数组的方法，它们的用法和区别如下：

splice 方法用于在数组内插入、删除或替换元素。语法如下：

```javascript
array.splice(startIndex, deleteCount, item1, ..., itemN)

```

startIndex：开始变更的索引（包括该位置的元素）

deleteCount：要删除的元素个数，可选。如果省略该参数，那么 startIndex 之后的所有元素都会被删除。

item1, ..., itemN：要插入到数组中的元素，可选。

splice 方法的返回值是一个包含被删除元素的数组。如果没有元素被删除，则返回一个空数组。

```javascript
const arr = [1, 2, 3, 4, 5];
const removed = arr.splice(1, 2, 7, 8);
console.log(arr); // [1, 7, 8, 4, 5]
console.log(removed); // [2, 3]
```

slice 方法用于截取数组的子数组。它不会修改原数组，而是返回一个新的数组。

```javascript
array.slice(startIndex, endIndex);
```

startIndex：起始索引（包含），可选。默认值为 0。

endIndex：结束索引（不包含），可选。默认值为数组的长度。

```javascript
const arr = [1, 2, 3, 4, 5];
const newArr = arr.slice(1, 4);
console.log(arr); // [1, 2, 3, 4, 5] （原数组不变）
console.log(newArr); // [2, 3, 4]
```

splice 和 slice 区别：

1. splice 方法会修改原数组，而 slice 方法不会修改原数组，而是返回一个新数组。
2. splice 方法可以插入、删除、替换元素，而 slice 方法用于截取子数组。
3. endIndex 参数在 slice 中是不包含在返回数组中的，而 splice 的 deleteCount 则是包含在被删除的元素个数中。

## 31. map 与 parseInt

请说出下列代码的返回值是什么

```javascript
[10, 20, 30].map(parseInt);
```

以上代码是想将数组 [10, 20, 30] 中的每个元素用 parseInt 函数解析成整数，然而实际执行结果却与预期不符：执行后的结果是 [10, NaN, NaN]。

为了理解这个现象，我们需要深入了解 map 和 parseInt 函数的工作原理。

map 是数组的一个方法，它接收一个函数作为参数。map 会遍历数组的每一个元素，并对每个元素执行传入的函数。

传入的函数有三个参数：当前元素、当前元素的索引和整个数组。

map 函数返回一个新的数组，这个新数组中的元素是原数组每个元素经过传入函数处理后的结果。

parseInt 是一个全局函数，用于将字符串转换为整数。

它接受两个参数：要解析的字符串和一个可选的基数（例如 2 表示二进制，10 表示十进制等）。

当提供基数时，parseInt 会按照给定的进制把字符串转换为对应的整数。

当我们将 parseInt 传入 map 函数时，map 函数会依次为数组中每个元素调用 parseInt 函数，并把当前元素值、索引和整个数组分别传给 parseInt 的第一个、第二个和第三个参数。然而此时 parseInt 函数的第二个参数并非作为解析的基数，而是把数组索引作为基数了。这就导致了实际执行结果与预期不符。

当处理第一个元素（10）时，`parseInt` 接收到的参数是 `parseInt(10, 0)`（0 是数组索引）。基数为 0 时，`parseInt` 会默认按照十进制进行解析，因此结果是 10。

当处理第二个元素（20）时，`parseInt` 接收到的参数是 `parseInt(20, 1)`（1 是数组索引）。此时基数为 1，但 1 是一个无效的基数，因此 `parseInt` 函数会返回 `NaN`。

当处理第三个元素（30）时，`parseInt` 接收到的参数是 `parseInt(30, 2)`（2 是数组索引）。此时基数为 2，即二进制，而 30 并不符合二进制表示，所以 `parseInt` 返回 `NaN`。

因此，`[10, 20, 30].map(parseInt)` 的执行结果是 `[10, NaN, NaN]`。

如果你想用 map 函数将 [10, 20, 30] 中的每个元素转换为整数，可以这样做：

```javascript
[10, 20, 30].map((num) => parseInt(num));
```

这样就可以得到预期的结果 `[10, 20, 30]`。

## 32. 函数声明与函数表达式

函数声明（Function Declaration）和函数表达式（Function Expression）都是在 JavaScript 中定义和创建函数的两种方法。

**1. 函数声明（Function Declaration）**

函数声明是使用 `function` 关键字后跟函数名称和函数体来定义函数的方法。函数声明会在代码执行前被初始化，因此可以在声明之前调用。这种行为被称为函数提升（hoisting）。

```javascript
// 函数声明
function greeting(name) {
  return "Hello, " + name;
}

// 调用函数
console.log(greeting("John"));
```

**2. 函数表达式（Function Expression）**

函数表达式是将一个函数赋值给一个变量。函数表达式在执行到其所在行时，通过变量名进行调用。

```javascript
// 函数表达式
const greeting = function (name) {
  return "Hello, " + name;
};

// 调用函数
console.log(greeting("John"));
```

**函数声明与函数表达式的区别：**

1. 提升：函数声明在执行前会被提升，这意味着你可以在声明之前调用它。而函数表达式需要等到其所在行被执行后，才可被调用。
2. 语法：函数声明需要提供一个函数名，而函数表达式可以是命名的也可以是匿名的。

```javascript
// 函数声明提升示例
console.log(square(5)); // 输出：25
function square(num) {
  return num * num;
}

// 函数表达式提升示例
console.log(square(5)); // 会抛出 TypeError: square is not a function
const square = function (num) {
  return num * num;
};
```

## 33. 手写 trim 方法

在 JavaScript 中，字符串对象自带了 trim() 方法，可以用来去除字符串两端的空格。

但是如果需要自己手动实现一个 trim() 方法，可以使用正则表达式来去除字符串两端的空格。

```javascript
String.prototype.trim = function () {
  return this.replace(/^\s+|\s+$/g, "");
};

// 使用自定义的 trim() 方法
let str = "   Hello, World!   ";
console.log(str.trim()); // 输出：'Hello, World!'
```

我们扩展了 String.prototype 对象，添加了一个名为 trim() 的方法。该方法使用正则表达式来去除字符串两端的空格，并返回处理后的字符串。

正则表达式 `/^\s+|\s+$/g` 匹配字符串两端的空格。其中：

- `^` 表示匹配字符串开头；
- `\s+` 表示匹配一个或多个空格；
- `|` 表示或者；
- `$` 表示匹配字符串结尾；
- `g` 表示全局匹配。

因此，`/^\s+|\s+$/g` 可以匹配字符串开头和结尾的空格，并将其替换为空字符串。

## 34. 手写 max 方法

> 目标：编写 myMax 方法模拟 Math.max 方法的功能

可以使用 apply() 方法来模拟 Math.max 方法的功能。

apply() 方法可以接受一个数组作为参数，并将其展开为一系列参数，传递给函数。

```javascript
function myMax() {
  let max = arguments[0];
  for (let i = 1; i < arguments.length; i++) {
    if (arguments[i] > max) {
      max = arguments[i];
    }
  }
  return max;
}

let nums = [1, 5, 3, 9, 2];
let maxNum = myMax.apply(null, nums);
console.log(maxNum); // 输出：9
```

## 35. 捕获异常

在 JavaScript 中，可以使用 try-catch 语句来捕获异常。

try-catch 语句包含两个关键字：try 和 catch。try 代码块中包含可能会引发异常的代码，而 catch 代码块用于处理异常情况。

```javascript
try {
  // 可能会引发异常的代码
  let x = y + 1;
} catch (e) {
  // 处理异常情况
  console.log("发生了异常：" + e.message);
}
```

onerror 事件：用于捕获全局范围内发生的异常。可以使用 window.onerror 事件来捕获全局范围内的异常。例如：

```javascript
// 该事件会在发生异常时自动触发，并将异常信息作为参数传递给事件处理函数。
window.onerror = function (message, url, line, column, error) {
  console.log("发生了异常：" + message);
};
```

## 36. JSON 介绍

JSON 是一种轻量级的数据交换格式，它的全称是 JavaScript Object Notation。

JSON 最初是由 Douglas Crockford 在 2001 年提出的，它是一种基于文本的数据格式，具有易于阅读和编写的特点。

JSON 格式通常用于通过网络传输数据，因为它可以被多种编程语言解析和生成。

JSON 与 JavaScript 密切相关，因为 JSON 的语法是 JavaScript 对象的子集。

这意味着在 JavaScrip t 中，我们可以轻松地将 JSON 格式的数据转换为 JavaScript 对象，反之亦然。

JavaScript 提供了两个内置方法来解析和生成 JSON 数据：JSON.parse() 和 JSON.stringify()。

序列化：将对象转换为 JSON 字符串

反序列化：将 JSON 字符串转换为 JSON 对象。

## 37. 获取查询参数

编写一个方法用于获取 url 中的查询参数并返回其对象格式。

```javascript
function getQueryParams() {
  var queryParams = {};
  var queryString = window.location.search.substring(1);
  var pairs = queryString.split("&");

  for (var i = 0; i < pairs.length; i++) {
    var pair = pairs[i].split("=");
    var key = decodeURIComponent(pair[0]);
    var value = decodeURIComponent(pair[1]);

    if (typeof queryParams[key] === "undefined") {
      queryParams[key] = value;
    } else if (Array.isArray(queryParams[key])) {
      queryParams[key].push(value);
    } else {
      queryParams[key] = [queryParams[key], value];
    }
  }

  return queryParams;
}
```

当用户需要从 URL 中获取查询参数时，可以使用以下 JavaScript 函数来实现：

```javascript
function getUrlParams(url) {
  const params = {};
  const regex = /([^&?+]+)=([^&?+]+)/g;
  url.replace(regex, function () {
    params[arguments[1]] = arguments[2];
  });
  return params;
}
```

这个函数使用正则表达式来匹配 URL 中的查询参数，然后将它们存储在一个对象中并返回。

```
https://example.com/search?q=javascript&lang=en&page=2

```

我们可以调用 getUrlParams 函数来获取查询参数：

```javascript
const urlParams = getUrlParams(
  "https://example.com/search?q=javascript&lang=en&page=2"
);
console.log(urlParams);
```

这将输出以下内容：

```javascript
{
  q: "javascript",
  lang: "en",
  page: "2"
}

```

## 38. 拍平多维数组

例如，调用 flatten([1, [2, 3], [4, [5, 6]]]) 将返回 [1, 2, 3, 4, 5, 6]。

```javascript
function flatten(arr) {
  let result = [];
  for (let i = 0; i < arr.length; i++) {
    if (Array.isArray(arr[i])) {
      result = result.concat(flatten(arr[i]));
    } else {
      result.push(arr[i]);
    }
  }
  return result;
}
```

```javascript
function flatten(arr) {
  return arr.reduce(function (flat, toFlatten) {
    return flat.concat(
      Array.isArray(toFlatten) ? flatten(toFlatten) : toFlatten
    );
  }, []);
}
```

## 39. 数组去重

以下是使用 JavaScript 编写的一个函数，可以对数组进行去重，包括三种不同的去重方法：

```javascript
function uniqueArray(arr) {
  // 方法一：使用 Set 数据结构
  const set = new Set(arr);
  const uniqueArr1 = [...set];

  // 方法二：使用 indexOf() 方法
  const uniqueArr2 = [];
  for (let i = 0; i < arr.length; i++) {
    if (uniqueArr2.indexOf(arr[i]) === -1) {
      uniqueArr2.push(arr[i]);
    }
  }

  // 方法三：使用 filter() 方法
  const uniqueArr3 = arr.filter(function (item, index, array) {
    return array.indexOf(item) === index;
  });

  return { uniqueArr1, uniqueArr2, uniqueArr3 };
}
```

这个函数接收一个数组作为参数，并返回一个包含三种不同去重方法结果的对象。其中：

- 方法一使用了 ES6 中的 Set 数据结构，将数组转换为 Set，然后再将 Set 转换为数组。
- 方法二使用了 for 循环和 indexOf() 方法来遍历数组，如果元素在新数组中不存在，就将其添加到新数组中。
- 方法三使用了 filter() 方法来遍历数组，只保留第一次出现的元素。

## 40. vue2 声明周期函数

在 Vue2 中，生命周期函数是 Vue 组件会经历的一系列依次触发的函数。它们在特定时候执行，并允许开发者执行响应逻辑。

这些生命周期函数主要包括以下几种:

1. beforeCreate：在 Vue 实例初始化之后，数据观测和事件配置之前被调用。

   表示含义：Vue 实例已经创建，但数据观测和事件还未设置。

   执行时机：页面渲染前。

   应用：这个阶段不能访问到 data、methods 等，通常用得较少。

   ```javascript
   new Vue({
     beforeCreate() {
       console.log("beforeCreate");
     },
   });
   ```

2. created：在 Vue 实例创建完成后被立即调用，此时已完成数据观测，方法和计算属性的运算。

   表示含义：Vue 实例创建完成，数据观测以及 data、methods 等已设置。

   执行时机：页面渲染前。

   应用：可以用于转换数据格式、初始化、扩展 methods 方法等。

   ```javascript
   new Vue({
     data: {
       message: "Hello Vue!",
     },
     created() {
       console.log("created");
       console.log(this.message);
     },
   });
   ```

3. beforeMount：在挂载开始之前被调用，相关的 render 函数首次被调用。

   表示含义：模板编译完成，但 DOM 还没有挂载。

   执行时机：页面渲染前。

   应用：一般不会在这个阶段操作，因为没有渲染出 DOM。

   ```javascript
   new Vue({
     el: "#app",
     beforeMount() {
       console.log("beforeMount");
     },
   });
   ```

4. mounted：在 Vue 实例挂载完成时调用。此时已完成模板到 DOM 的挂载。

   表示含义：完成 DOM 挂载，可以操作 DOM。

   执行时机：页面渲染后。

   应用：你可以在这里操作 DOM，比如插入插件或者调用接口。

   ```javascript
   new Vue({
     el: "#app",
     mounted() {
       console.log("mounted");
     },
   });
   ```

5. beforeUpdate：数据发生变化时，且发生在虚拟 DOM 重新渲染和打补丁之前调用。

   表示含义：数据已更新，但 DOM 还没有更新。

   执行时机：页面更新前。

   应用：可以在更新之前进行某些操作，比如离开当前页面前做个提示。

   ```javascript
   new Vue({
     data: {
       message: "Hello Vue!",
     },
     beforeUpdate() {
       console.log("beforeUpdate");
     },
   });
   ```

6. updated：在虚拟 DOM 重新渲染和打补丁后调用，表示 DOM 已完成更新。

   表示含义：DOM 更新完成。

   执行时机：页面更新后。

   应用：可以执行依赖于 DOM 的操作，但要注意避免无限循环更新。

   ```javascript
   new Vue({
     data: {
       message: "Hello Vue!",
     },
     updated() {
       console.log("updated");
     },
   });
   ```

7. beforeDestroy：在 Vue 实例销毁之前调用。此时实例仍然可以完全正常使用。

   表示含义：实例即将销毁，但仍可正常使用。

   执行时机：销毁前。

   应用：可以用来解绑事件或者清除定时器等。

   ```javascript
   new Vue({
     beforeDestroy() {
       console.log("beforeDestroy");
     },
   });
   ```

8. destroyed：在 Vue 实例销毁完成后调用。此时所有的绑定、实例方法等均已解除。
   表示含义：实例已完全销毁。
   执行时机：销毁后。
   应用：这个阶段实例已被销毁，很少会用到。

   ```javascript
   new Vue({
     destroyed() {
       console.log("destroyed");
     },
   });
   ```

以上就是 Vue2 中的生命周期函数及其作用和应用。在实际项目中，我们可以根据不同需求选择合适的生命周期函数来执行相应操作。

## 41. vue2 父子组件生命周期调用顺序

在 Vue2 中，父子组件的生命周期调用顺序如下：

1. 挂载阶段（Mounting）
   - 父组件：beforeCreate
   - 父组件：created
   - 父组件：beforeMount
   - 子组件：beforeCreate
   - 子组件：created
   - 子组件：beforeMount
   - 子组件：mounted
   - 父组件：mounted
2. 更新阶段（Updating）
   - 父组件：beforeUpdate
   - 子组件：beforeUpdate
   - 子组件：updated
   - 父组件：updated
3. 卸载阶段（Unmounting）
   - 父组件：beforeDestroy
   - 子组件：beforeDestroy
   - 子组件：destroyed
   - 父组件：destroyed

## 42. vue2 keep-alive 组件

在 Vue2 中，keep-alive 是一个抽象组件，它的作用是缓存非活动的组件实例，以避免反复重渲染，提高性能。

keep-alive 自身实际上不会渲染成一个 DOM 元素。

(1) 应用场景

1. 列表切换渲染时：当有多个列表页面时，用户在不同的列表页面之间切换，需要保持每个列表页面的滚动位置、数据状态等。
2. 页面级别的缓存：SPA 应用中，用户在不同页面之间切换时，需要保持一些页面的缓存，以提升性能并减少页面加载时间。
3. 菜单切换渲染：当有多个菜单需要切换渲染时，为了保持之前菜单的状态，可以使用 keep-alive 来缓存这些菜单组件。

(2) 特有生命周期函数

activated：当被包裹的组件激活时，执行此钩子函数。这个钩子函数表示组件被缓存后，再次被渲染到页面时调用。

deactivated：当被包裹的组件被缓存时，执行此钩子函数。这个钩子函数表示组件被缓存时调用。

(3) 实现原理

keep-alive 的实现原理主要基于 Vue 的虚拟 DOM 和组件的生命周期函数。

1. 当一个组件被包裹在 keep-alive 中时，keep-alive 会监听它的 activated 和 deactivated 钩子函数，缓存/激活/子组件。

2. keep-alive 使用了 Vue 的虚拟 DOM 实现，通过创建一个 \<keep-alive\> 的虚拟节点将对应组件的虚拟节点放入属性对应的缓存，为被包裹的组件额外添加 activated 和 deactivated 的生命周期处理，以达到激活和停用组件的目的。

3. 当组件被激活时，keep-alive 会使用缓存中的组件 VNode 进行渲染，而不是重新创建一个新的 VNode。

   这样，缓存的组件实例仍然保持之前的状态，不需要进行重新渲染。

4. 当组件被停用时，keep-alive 会将其放入缓存，组件实例并未销毁，可用来再次激活组件。

5. 可以使用 include 和 exclude 属性来指定哪些组件需要被缓存或排除。

(4) 示例代码

假设我们有两个组件： Home.vue 和 About.vue。当导航从 Home 组件切换到 About 组件时，我们希望保留 Home 组件的状态。

1. 首先，创建两个组件文件：`Home.vue` 和 `About.vue`。

```vue
<!-- Home.vue -->
<template>
  <div>
    <h2>Home Page</h2>
  </div>
</template>

<script>
export default {
  name: "Home",
};
</script>
```

```vue
<!-- About.vue -->
<template>
  <div>
    <h2>About Page</h2>
  </div>
</template>

<script>
export default {
  name: "About",
};
</script>
```

2. 接下来，在`App.vue`文件中设置我们的路由视图以及`keep-alive`组件。

```vue
<!-- App.vue -->
<template>
  <div>
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>
    </nav>
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </div>
</template>
```

3. 然后，在`router.js`文件里配置我们的路由。

```js
// router.js
import Vue from "vue";
import Router from "vue-router";
import Home from "./components/Home.vue";
import About from "./components/About.vue";

Vue.use(Router);

export default new Router({
  mode: "history",
  routes: [
    {
      path: "/",
      name: "home",
      component: Home,
    },
    {
      path: "/about",
      name: "about",
      component: About,
    },
  ],
});
```

4. 最后，在项目入口文件`main.js`中导入路由配置。

```js
// main.js
import Vue from "vue";
import App from "./App.vue";
import router from "./router";

Vue.config.productionTip = false;

new Vue({
  router,
  render: (h) => h(App),
}).$mount("#app");
```

现在，当你在 `Home` 和 `About` 组件之间切换时，组件的状态应该被保留在 `keep-alive` 组件中。

## 43. vue2 生命周期与异步请求

在 Vue2 中，可以执行异步请求的生命周期函数有以下几个：

1. created：组件实例被创建后，在渲染 DOM 之前执行。
2. mounted：组件实例被挂载到 DOM 元素上之后执行。
3. updated：组件数据更新导致的虚拟 DOM 重新渲染后执行。
4. beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave：在路由导航守卫中执行。

在这些生命周期里，通常推荐在 created 生命周期进行异步请求。理由如下：

1. created 生命周期会在渲染 DOM 之前执行。这意味着，在数据请求返回前，可以进行其他的逻辑处理，例如展示加载状态等。

2. 如果在 mounted 生命周期发起异步请求，可能会让用户在等待数据返回时看到一个空的页面，影响用户体验。

3. 在 updated 生命周期发起异步请求会增加不必要的更新，因为每次数据更新都会触发这个生命周期。

   通常情况下，我们需要根据具体需求调用和更新数据。

4. 在路由导航守卫中进行异步请求适用于那些依赖特定路由参数的场景，如需要在页面跳转前获取数据。

综上，对于普通的异步请求推荐使用 created 生命周期。当然，在特定情况下，请根据具体需求选择合适的生命周期。

## 44. Vue2 v-if vs v-show

在 Vue2 中，v-show 和 v-if 是两个用于条件渲染的指令。

下面分别介绍这两个指令的相同点和不同点，以及哪个指令的性能更好。

相同点：

1. 都是用于条件渲染。v-show 和 v-if 这两个指令都可以根据条件去渲染或者隐藏某个 HTML 元素。

不同点：

1. v-if 是"真正"的条件渲染指令，它会根据表达式的值在 DOM 树中插入或者删除对应的元素。

   v-show 只是简单地切换元素的 CSS 属性 display，控制元素的显示和隐藏。

2. 当表达式的值为 false 时，v-if 不会渲染元素到 DOM 树中

   v-show 无论表达式的值为何始终都会渲染元素，在元素的 display 属性中设置为 none 来控制隐藏。

3. v-if 由于需要插入和删除 DOM 元素，当条件切换频繁时，对性能消耗较大。

   v-show 只是简单地修改 CSS 属性，性能消耗较小。

如果需要频繁切换显示隐藏的场景，建议使用 v-show，因为它只需要修改 CSS 属性，性能开销较小。

如果元素不需要频繁切换，或者切换时需要重新获取数据、处理逻辑等，建议使用 v-if，因为它不会多次渲染不需要的元素，从而减少性能消耗。

## 45. v-for key

问题：v-for 为什么要配合 key 一起使用？

在 Vue2 中，v-for 和 key 被一起使用是出于性能优化的考虑，有助于提高列表渲染的性能。

v-for 指令是 Vue.js 中用于循环渲染列表元素的一个重要特性。在数据发生变化时，Vue 将更新 DOM 来匹配新的数据。

然而，有时候我们只是对列表进行简单的添加或删除操作，如果每次都重新计算整个列表并重新渲染，则会造成浪费，降低性能。

为解决此问题，Vue 实现了一种名为“就地复用”的策略。

当列表发生变化时，它会尽可能地减少重新创建和销毁 DOM 元素的次数。如果一个元素没有 key，Vue 将尝试使用 tag 复用现有元素。

然而，这种复用策略可能会导致一些问题。例如当列表元素的顺序发生变化时，它可能会导致不必要的更新，从而导致性能问题。

这时我们需要使用 key 属性来指定一个唯一标识符，使得 Vue 可以准确地识别每一个列表项。

key 属性应该为每一个列表项分配一个唯一的值，便于 Vue 知道如何对应新旧元素。

这样，当数据发生变化时，Vue 可以通过 key 属性来识别哪个元素被添加、修改或者删除，从而实现更有效的局部更新，提高列表渲染性能。

## 46. Vue2 组件通讯

在 Vue2 中，我们可以使用以下方式实现组件之间的通信：

(1) 父组件向子组件传递数据：通过 props

```html
<!-- 父组件 -->
<template>
  <div>
    <child-component :parent-data="data"></child-component>
  </div>
</template>

<script>
  import ChildComponent from "./ChildComponent.vue";

  export default {
    components: {
      ChildComponent,
    },
    data() {
      return {
        data: "父组件数据",
      };
    },
  };
</script>
```

```html
<!-- 子组件 -->
<template>
  <div>{{ parentData }}</div>
</template>

<script>
  export default {
    props: {
      parentData: {
        type: String,
        required: true,
      },
    },
  };
</script>
```

(2) 子组件向父组件传递数据，通过自定义事件和 $emit

```html
<!-- 父组件 -->
<template>
  <div>
    <child-component @child-event="handleChildEvent"></child-component>
  </div>
</template>

<script>
  import ChildComponent from "./ChildComponent.vue";

  export default {
    components: {
      ChildComponent,
    },
    methods: {
      handleChildEvent(payload) {
        console.log("子组件传递的数据：", payload);
      },
    },
  };
</script>
```

```html
<!-- 子组件 -->
<template>
  <button @click="sendDataToParent">发送数据至父组件</button>
</template>

<script>
  export default {
    methods: {
      sendDataToParent() {
        this.$emit("child-event", "来自子组件的数据");
      },
    },
  };
</script>
```

(3) 兄弟组件之间通信需要通过共同的父组件，或者使用事件总线（Event Bus）

首先创建一个新的事件总线实例：

```javascript
// event-bus.js
import Vue from "vue";
export const EventBus = new Vue();
```

组件 A 发送数据至组件 B：

```html
<template>
  <button @click="sendDataToSibling">发送数据至兄弟组件</button>
</template>

<script>
  import { EventBus } from "@/event-bus.js";

  export default {
    methods: {
      sendDataToSibling() {
        EventBus.$emit("sibling-event", "来自组件 A 的数据");
      },
    },
  };
</script>
```

组件 B 接收数据：

```html
<template>
  <div>接收到的数据：{{ receivedData }}</div>
</template>

<script>
  import { EventBus } from "@/event-bus.js";

  export default {
    data() {
      return {
        receivedData: null,
      };
    },
    created() {
      EventBus.$on("sibling-event", this.handleSiblingData);
    },
    beforeDestroy() {
      EventBus.$off("sibling-event", this.handleSiblingData);
    },
    methods: {
      handleSiblingData(payload) {
        this.receivedData = payload;
      },
    },
  };
</script>
```

注意：使用事件总线时，不要忘记在组件销毁时移除对应的事件监听，避免内存泄漏。

## 47. v-model 的实现原理

Vue.js 中的 v-model 是实现表单控件与数据双向绑定的指令。

其实现原理主要基于两个部分：数据到视图的绑定（数据驱动视图更新）和视图到数据的绑定（视图驱动数据更新）。

数据到视图的绑定：当数据发生变化时，通过数据劫持（Vue.js 2.x 使用 Object.defineProperty，Vue.js 3.x 使用 Proxy）触发数据变化的侦听函数，然后通过订阅者模式通知相应的指令更新，最后视图更新。

视图到数据的绑定：当用户与视图交互（如输入文本、选择选项等）时，通过监听视图的 input 事件，获取视图的最新值，然后将其赋值给相应的数据，实现视图到数据的绑定。

下面是一个 Vue.js 的代码示例：

```html
<template>
  <div>
    <!-- 使用 v-model 进行双向数据绑定 -->
    <input type="text" v-model="message" />
    <p>{{ message }}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        message: "",
      };
    },
  };
</script>
```

在这个示例中，我们使用 v-model 指令将输入框与 data 中的 message 属性进行双向绑定。当用户输入文本时，视图到数据的绑定将用户的输入赋值给 message 属性；同时，数据到视图的绑定会自动将 message 的最新值更新到视图上，展示在 `<p>` 标签中。

在原生 JavaScript 中，你可以使用事件监听器和数据代理来实现一个简单的双向数据绑定。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <input type="text" id="input" />
    <p id="output"></p>
    <script>
      // 1. 获取 DOM 元素
      const input = document.getElementById("input");
      const output = document.getElementById("output");

      // 2. 初始化一个数据对象
      let data = {
        value: "",
      };

      // 3. 代理数据对象，监听数据变化
      const proxy = new Proxy(data, {
        set(target, key, value) {
          target[key] = value;
          // 更新 DOM
          output.innerText = value;
          return true;
        },
      });

      // 4. 监听 input 事件，并更新数据对象
      input.addEventListener("input", (e) => {
        proxy.value = e.target.value;
      });

      // 初始更新
      proxy.value = "";
    </script>
  </body>
</html>
```

这个示例中，我们创建了一个简单的数据代理，使用 Proxy 对象来代理一个初始数据对象并在数据对象的值被设置时更新 DOM。然后，我们使用事件监听器监听 input 元素的输入事件，并在输入值发生变化时更新数据代理对象的值。通过这样的方式，我们实现了一个简单的双向数据绑定。

## 48. 如何理解 MVVM

MVVM（Model-View-ViewModel）模式是一种软件架构设计模式，主要用于分离应用程序的界面表示层（View）和业务逻辑层（Model）。它的核心思想是基于数据驱动的视图更新。ViewModel 作为 View 和 Model 之间的连接器，负责将 Model 中的数据转换成 View 可以显示的数据，并处理从 View 接收到的用户交互事件。MVVM 让应用程序的不同部分更容易分离、测试和维护。

MVVM 的优点如下：

1. 解耦：MVVM 模式可以将 UI（用户界面）和业务逻辑分离，在一定程度上简化了代码编写和程序设计。
2. 双向数据绑定：ViewModel 和 View 之间的数据同步是自动完成的，这意味着当数据发生变化时，不需要手动更新视图，从而减少了很多重复和繁琐的工作。
3. 可维护性：由于 UI 和业务逻辑分离，维护成本较低。当需要更新视图或业务逻辑时，可以只关注对应的部分，而不会对其他部分产生影响。
4. 可测试性：可以对 ViewModel 进行单元测试，提高应用程序的可靠性和健壮性。
5. 可重用性：可以重用 ViewModel 中的代码，提高开发效率。

MVVM 的缺点如下：

1. 过度抽象：当应用程序的功能或逻辑不是很复杂时，MVVM 可能会导致过度抽象，增加了代码的复杂性和学习成本。
2. 内存占用：由于持续监听数据变化和双向绑定，可能导致更高的内存占用和性能损失。
3. 依赖框架：为实现 MVVM，通常需要依赖于特定的框架（如：Vue.js、Angular、React 等），在某种程度上限制了技术选型。

总结一下，MVVM 为前端应用带来了更好的可维护性、可测试性和解耦。它尤其适用于复杂数字、逻辑复杂的大型应用程序。但对于简单的应用程序来说，可能会导致过度抽象和依赖于特定框架。

<img src="./assets/09.png" align="left" width="70%"/>

在 JavaScript 中，Object.create() 是一个用于创建新对象的方法。它是基于现有对象创建新对象的一种方式，新对象可以继承现有对象的属性和方法。

使用 Object.create() 方法时，需要传入一个原型对象作为参数。该方法将返回一个新对象，该对象的原型是传入的原型对象。这个新对象可以通过访问原型对象来获取属性和方法。

以下是一个简单的示例，说明如何使用 Object.create() 方法：

```javascript
// 定义一个原型对象
let person = {
  name: "John",
  age: 30,
  greeting: function () {
    console.log(
      "Hello, my name is " + this.name + " and I am " + this.age + " years old."
    );
  },
};

// 使用 Object.create() 方法创建一个新对象
let student = Object.create(person);

// 设置新对象的属性
student.name = "Jane";
student.age = 20;

// 调用新对象的方法
student.greeting(); // 输出：Hello, my name is Jane and I am 20 years old.
```

在这个示例中，我们首先定义了一个名为 `person` 的原型对象，它有三个属性：`name`、`age` 和 `greeting`。然后，我们使用 `Object.create()` 方法创建了一个名为 `student` 的新对象，并将 `person` 对象作为其原型。最后，我们设置了 `student` 对象的 `name` 和 `age` 属性，并调用了其 `greeting()` 方法。

需要注意的是，使用 Object.create() 方法创建的新对象并不具有自己的属性和方法，它们都是从原型对象继承而来的。如果需要添加新的属性或方法，可以直接在新对象上定义。

## 49 computed 计算属性

Vue 中的计算属性（Computed properties）是一种特殊类型的属性，它依赖其他属性值的变化进行自动计算和更新。

计算属性的主要作用是将一些复杂的逻辑和计算放入独立的属性中，使 Vue 模板更简洁、可读性更强。

计算属性具有以下特性：

1. 响应式依赖：计算属性依赖其他响应式属性值，当依赖属性值发生变化时，计算属性自动更新。
2. 缓存优化：计算属性具有缓存机制，只有当依赖的属性值发生变化时，计算属性才重新计算值。否则，直接使用缓存的值。
3. 可读性：通过计算属性可以将复杂数值计算和转换逻辑移到独立的属性中，使 Vue 模板更简洁、便于理解和维护。
4. 支持 getter 和 setter：计算属性默认只有 getter 方法，但也可以指定一个 setter 方法，实现对计算属性的赋值操作。

实际项目中，计算属性的应用主要包括：

- 动态计算属性值：当一个属性值依赖于其他属性值时，可以使用计算属性。
- 格式化显示数据：当需要对数据进行格式化或转换时，可以使用计算属性。
- 过滤数组或对象：当需要根据一定条件筛选出数据列表或对象的子集时，可以使用计算属性。

以下是一个代码示例：

```html
<template>
  <div>
    <input v-model="firstName" placeholder="First Name" />
    <input v-model="lastName" placeholder="Last Name" />
    <p>Full name: {{ fullName }}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        firstName: "John",
        lastName: "Doe",
      };
    },
    computed: {
      // 计算属性 fullName 依赖于 firstName 和 lastName
      fullName() {
        return this.firstName + " " + this.lastName;
      },
    },
  };
</script>
```

在这个示例中，我们有两个输入框，用于填写名和姓。我们定义了一个计算属性 fullName，它的值依赖于 firstName 和 lastName。当我们在输入框中更改名字或姓氏时，fullName 自动更新，显示完整的名字。这使我们的模板非常简洁，便于理解。

## 50. vue data 函数

为什么 vue 中的 data 配置项必须是一个函数?

在 Vue 中，data 配置项必须是一个函数，主要原因是为了避免组件实例之间共享数据。

若不是一个函数，那么所有组件实例将引用相同的数据对象，这样当其中一个组件实例改变数据时，其他组件实例中的数据也将受到影响。

假设我们有一个简单的组件，用于显示一个计数器：

```javascript
Vue.component("counter", {
  template: `<div>{{ count }}</div>`,
  data: function () {
    return {
      count: 0,
    };
  },
});
```

我们将 `data` 配置项定义为一个函数，它返回一个对象，包含该组件实例的初始数据。

在这个例子中，我们为每个 `counter` 组件实例定义了一个 `count` 属性，并将其初始值设置为 0。

当我们在应用程序中多次使用 `counter` 组件时：

```html
<div id="app">
  <counter></counter>
  <counter></counter>
  <counter></counter>
</div>
```

每个组件实例都会拥有自己独立的 `count` 数据。可以想象，如果我们将 `data` 直接定义为一个对象，如下所示：

```javascript
Vue.component("counter", {
  template: `<div>{{ count }}</div>`,
  data: {
    count: 0,
  },
});
```

那么所有的 `counter` 组件实例将共享同一个 `count` 数据，并且当一个实例的 `count` 值发生变化时，其他实例的 `count` 值也会受到影响。

因此，将 `data` 定义为一个函数，确保了每个组件实例都能拥有独立的数据副本，从而避免了潜在的问题。

## 51. vue2 抽取逻辑

如何将多个组件中的公共逻辑抽取出来？

在 Vue2 中，有两种主要的方式可以将多个组件中的相同逻辑抽取出来： Mixins（混入） 和高阶组件（Higher-Order Components，简称 HOC）。

1. Mixins（混入）

Mixins 是一种在多个组件之间共享可复用的功能的方法。一个 mixin 的方法可以被其他组件混入，这样这些组件就可以获得 mixin 中定义的数据、组件、指令、生命周期方法等。

假设我们有以下相同逻辑需要在多个组件中使用：

```javascript
// commonLogic.js
export default {
  data() {
    return {
      message: "Hello from mixin!",
    };
  },
  created() {
    console.log(this.message);
  },
  methods: {
    showAlert() {
      alert(this.message);
    },
  },
};
```

在其他组件中使用 Mixin：

```javascript
// ComponentA.vue
import commonLogic from "./commonLogic.js";

export default {
  mixins: [commonLogic],
  // ...
};
```

```javascript
// ComponentB.vue
import commonLogic from "./commonLogic.js";

export default {
  mixins: [commonLogic],
  // ...
};
```

2. 高阶组件（Higher-Order Components）

高阶组件（HOC）是一个接收组件作为参数并返回一个新组件的函数。该新组件包含了原组件的所有功能，并且可以向原组件注入新的逻辑。

```javascript
// withCommonLogic.js
export default function withCommonLogic(WrappedComponent) {
  return {
    name: `withCommonLogic(${WrappedComponent.name})`,
    data() {
      return {
        message: "Hello from HOC!",
      };
    },
    created() {
      console.log(this.message);
    },
    methods: {
      showAlert() {
        alert(this.message);
      },
    },
    render(h) {
      return h(WrappedComponent, {
        props: this.$props,
        on: this.$listeners,
        scopedSlots: this.$scopedSlots,
      });
    },
  };
}
```

在其他组件中使用 HOC：

```javascript
// ComponentA.vue
import withCommonLogic from "./withCommonLogic.js";

export default withCommonLogic({
  name: "ComponentA",
  // ...
});
```

```javascript
// ComponentB.vue
import withCommonLogic from "./withCommonLogic.js";

export default withCommonLogic({
  name: "ComponentB",
  // ...
});
```

以上两种方法都可以在 Vue2 中将多个组件中的相同逻辑抽取出来。你可以根据具体的需求和项目场景决定使用哪种方式。

## 52. vue2 异步组件

异步组件在 Vue.js 中可以帮助我们实现代码分割和按需加载，优化性能和加载速度。这对于大型应用程序是非常有用的。以下是 4 个场景的例子：

(1) 路由懒加载

可以把某个路由对应的组件编写成异步组件，这样在初始加载时，只需要加载首页的组件，其他页面的组件可以在实际访问时按需加载。

```javascript
import Vue from "vue";
import Router from "vue-router";

Vue.use(Router);

const Foo = () => import("@/components/Foo.vue");
const Bar = () => import("@/components/Bar.vue");

export default new Router({
  routes: [
    { path: "/foo", component: Foo },
    { path: "/bar", component: Bar },
  ],
});
```

(2) 按需加载弹窗组件

某些场景下，我们需要在用户点击某个按钮后展示一个弹窗组件。这种组件在页面加载时可能并不需要，可以使用异步组件按需加载。

```html
<template>
  <div>
    <button @click="openModal">打开弹窗</button>
    <Modal v-if="showModal" />
  </div>
</template>

<script>
  const Modal = () => import("@/components/Modal.vue");

  export default {
    data() {
      return {
        showModal: false,
      };
    },
    components: { Modal },
    methods: {
      openModal() {
        this.showModal = true;
      },
    },
  };
</script>
```

(3) 大型选项卡式应用

在一个包含多个选项卡的应用中，可以将每个选项卡对应的组件编写为异步组件，这样可以在切换选项卡时按需加载相应的组件。

```html
<template>
  <div>
    <tabs>
      <tab name="A" :component="A"></tab>
      <tab name="B" :component="B"></tab>
      <tab name="C" :component="C"></tab>
    </tabs>
  </div>
</template>

<script>
  const A = () => import("@/components/A.vue");
  const B = () => import("@/components/B.vue");
  const C = () => import("@/components/C.vue");

  export default {
    components: {
      A,
      B,
      C,
    },
  };
</script>
```

(4) 根据不同设备加载不同组件

当我们需要根据不同设备展示不同的组件时，可以在运行时动态加载相应的异步组件。

```html
<template>
  <div>
    <component :is="currentComponent" />
  </div>
</template>

<script>
  const DesktopComponent = () => import("@/components/DesktopComponent.vue");
  const MobileComponent = () => import("@/components/MobileComponent.vue");

  export default {
    data() {
      return {
        currentComponent: null,
      };
    },
    created() {
      if (window.innerWidth < 768) {
        this.currentComponent = MobileComponent;
      } else {
        this.currentComponent = DesktopComponent;
      }
    },
  };
</script>
```

## 53. vue 作用域插槽

Vue 中的作用域插槽（Scoped Slots）是一种特殊类型的插槽，它允许父组件向子组件传递数据，同时还允许子组件在其插槽中使用该数据。这使得子组件可以更灵活地渲染父组件传递的数据，而不仅仅是简单地显示它们。

```html
<!-- 父组件 -->
<template>
  <div>
    <child-component>
      <template v-slot:default="slotProps">
        {{ slotProps.message }} World!
      </template>
    </child-component>
  </div>
</template>

<script>
  import ChildComponent from "./ChildComponent.vue";

  export default {
    components: {
      ChildComponent,
    },
  };
</script>
```

```html
<!-- 子组件 -->
<template>
  <div>
    <slot :message="message"></slot>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        message: "Hello",
      };
    },
  };
</script>
```

在这个示例中，父组件包含一个名为 child-component 的子组件。父组件使用 v-slot 指令来定义一个默认插槽，并将其绑定到一个名为 slotProps 的变量上。该变量是一个对象，其中包含子组件需要使用的数据。在这种情况下，子组件需要使用名为 message 的数据。

子组件包含一个名为 message 的数据属性，并将其传递给插槽。父组件中的模板使用 slotProps.message 来访问该数据，并将其与字符串 World!连接起来。

## 54. vuex action mutation

在 Vuex 中，mutation 和 action 都是用于管理应用程序状态的重要概念。

它们的作用是不同的，mutation 用于修改状态，而 action 用于处理异步逻辑。

mutation 是 Vuex 中用于修改状态的方法。它们必须是同步的，这意味着它们不能包含异步逻辑。mutation 只能通过提交(commit)来调用。

下面是一个简单的示例：

```javascript
const store = new Vuex.Store({
  state: {
    count: 0,
  },
  mutations: {
    increment(state) {
      state.count++;
    },
  },
});

store.commit("increment");
```

action 是 Vuex 中用于处理异步逻辑的方法。它们可以包含任何异步代码，例如 API 调用或 setTimeout()函数。action 可以通过 dispatch 来调用。

```javascript
const store = new Vuex.Store({
  state: {
    count: 0,
  },
  mutations: {
    increment(state) {
      state.count++;
    },
  },
  actions: {
    incrementAsync(context) {
      setTimeout(() => {
        context.commit("increment");
      }, 1000);
    },
  },
});

store.dispatch("incrementAsync");
```

## 55. vue router 路由模式

Vue Router 提供了三种路由模式：hash 模式、history 模式和 abstract 模式。

(1) Hash 模式

Hash 模式使用 URL 的 hash 部分（即#号后面的内容）来管理路由。在这种模式下，当 URL 的 hash 部分发生变化时，Vue Router 会自动更新视图。Hash 模式不需要服务器配置，因此它非常适合在静态文件服务器上部署单页应用程序。

```javascript
const router = new VueRouter({
  mode: 'hash',
  routes: [...]
})

```

(2) History 模式

History 模式使用 HTML5 History API 来管理路由。在这种模式下，URL 的路径部分被用来管理路由。当 URL 的路径部分发生变化时，Vue Router 会自动更新视图。History 模式需要服务器配置，以便在用户直接访问页面时正确地响应请求。

```javascript
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})

```

(3) Abstract 模式

Abstract 模式不会改变浏览器的 URL，而是将路由信息保存在内存中。这种模式常用于服务器端渲染或单元测试中。

```javascript
const router = new VueRouter({
  mode: 'abstract',
  routes: [...]
})

```

在选择路由模式时，需要考虑以下几个因素：

1. 服务器配置：如果你的应用程序需要在服务器上运行，则需要选择支持 HTML5 History API 的路由模式。
2. SEO 优化：如果你的应用程序需要进行 SEO 优化，则需要选择支持 HTML5 History API 的路由模式。
3. 兼容性：如果你的应用程序需要在旧版浏览器上运行，则需要选择支持 Hash 模式的路由模式。

综上所述，选择哪种路由模式取决于你的应用程序的需求和服务器配置。

在大多数情况下，我们建议使用 History 模式，因为它可以提供更好的用户体验和 SEO 优化。

## 55. JavaScript 模拟前端路由

前端路由可以通过监听 URL 的变化，并根据 URL 的不同渲染不同的视图来实现。下面是使用 JavaScript 模拟前端路由的示例代码：

```javascript
const routes = {
  "/": home,
  "/about": about,
  "/contact": contact,
};

const content = document.querySelector(".content");

function render(path) {
  content.innerHTML = routes[path];
}

function router() {
  const path = window.location.pathname;
  render(path);
}

window.addEventListener("popstate", router);

document.addEventListener("DOMContentLoaded", () => {
  document.body.addEventListener("click", (e) => {
    if (e.target.tagName === "A") {
      e.preventDefault();
      const href = e.target.getAttribute("href");
      window.history.pushState(null, null, href);
      router();
    }
  });

  router();
});
```

在这个示例中，我们首先定义了一个 routes 对象，其中包含了不同路径对应的视图。

然后，我们定义了一个 render 函数，用于根据 URL 渲染不同的视图。

接下来，我们定义了一个 router 函数，用于监听 popstate 事件并根据 URL 的变化渲染不同的视图。我们还在 DOMContentLoaded 事件中添加了一个事件监听器，用于处理用户点击链接时的行为。当用户点击链接时，我们使用 pushState 方法将新的 URL 添加到浏览器历史记录中，并调用 router 函数来渲染新的视图。

最后，在 DOMContentLoaded 事件中调用 router 函数来初始化路由，并开始监听 popstate 事件的触发。

需要注意的是，这只是一个简单的示例代码，实际的前端路由实现可能需要更复杂的逻辑和处理。

在 JavaScript 中，执行顺序是由事件循环（Event Loop）来实现的。事件循环中主要有两种任务：宏任务（Macro-task）和微任务（Micro-task）。DOM 渲染通常在宏任务和微任务之间执行。

执行顺序如下：

1. 首先处理宏任务（例如整个 script 代码执行完毕）
2. 完成 DOM 渲染。
3. 处理微任务队列中的所有微任务
4. 进行下一轮的宏任务（例如 setTimeout）

依赖注入（Dependency Injection，简称 DI）是一种软件设计模式，它通过将对象的依赖关系从对象内部解耦，使得对象实例在运行时可以更加灵活地接收依赖项。依赖注入主要有以下好处：

1. 提高了代码的可维护性，使得组件之间的耦合度降低。
2. 提高了代码的可测试性，因为依赖关系可以根据测试需求动态注入。

下面举一个简单的 TypeScript 代码例子说明依赖注入的工作方式。

假设我们需要为一个电子商务系统设计一个购物车功能。购物字功能内部需要调用一系列子服务，例如库存服务、付款服务和邮寄服务。

```typescript
// 定义子服务
interface IStockService {
  checkStock(product: string): boolean;
}

interface IPayService {
  pay(amount: number): boolean;
}

interface IShippingService {
  ship(products: string[], address: string): boolean;
}

// 创建依赖于子服务的购物车类
class ShoppingCart {
  private stockService: IStockService;
  private payService: IPayService;
  private shippingService: IShippingService;

  constructor(
    stockService: IStockService,
    payService: IPayService,
    shippingService: IShippingService
  ) {
    this.stockService = stockService;
    this.payService = payService;
    this.shippingService = shippingService;
  }

  purchase(products: string[], address: string): boolean {
    for (const product of products) {
      if (!this.stockService.checkStock(product)) {
        console.log(`库存不足: ${product}`);
        return false;
      }
    }

    const amount = products.length * 10;
    if (!this.payService.pay(amount)) {
      console.log("付款失败");
      return false;
    }

    if (!this.shippingService.ship(products, address)) {
      console.log("邮寄失败");
      return false;
    }

    console.log("购物成功");
    return true;
  }
}
```

在这个例子中，购物车类（ShoppingCart）的实例需要接收三个子服务实例作为创建实例的输入。这样做的好处是：

- 查看购物车类，可以很明显地看到它依赖于哪些子服务，便于理解和维护。
- 在运行时，可以根据需要灵活地为购物车类注入不同版本的子服务，例如在测试环境中使用模拟数据的子服务。

为了完成依赖注入，你需要创建子服务的实现类并将它们传递给购物车类。例如，你可以这样使用购物车类：

```typescript
// 定义子服务的实现类
class StockService implements IStockService {
  checkStock(product: string): boolean {
    return true; // 在实际项目中，此处应从数据库或其他来源检查库存
  }
}

class PayService implements IPayService {
  pay(amount: number): boolean {
    return true; // 在实际项目中，此处应调用支付接口完成付款
  }
}

class ShippingService implements IShippingService {
  ship(products: string[], address: string): boolean {
    return true; // 在实际项目中，此处应调用物流接口完成邮寄
  }
}

// 实例化子服务
const stockService = new StockService();
const payService = new PayService();
const shippingService = new ShippingService();

// 依赖注入：实例化购物车类，并将子服务的实例注入
const shoppingCart = new ShoppingCart(
  stockService,
  payService,
  shippingService
);
shoppingCart.purchase(["product-1", "product-2"], "北京市海淀区");
```

在这个例子中，我们首先实现了子服务的实现类，接着创建了子服务的实例，最后将他们注入到购物车类中。这样就实现了购物车类对子服务的灵活依赖。
