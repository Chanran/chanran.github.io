---
title: ES6(二)--变量的解构赋值
date: 2017-01-11 18:38:09
tags: [ES6]
catogory: [ES6]
---

# 定义

> ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

# 数组的解构赋值

> 基本要领：结构一致，左边变量等于“=”右边的值

## 完全匹配

```
let [a,b,c] = [1,2,3];
console.log(a); //1
console.log(b); //2
console.log(c); //3
```

```
let [a,[b],c] = [1,[2],3];
console.log(a); //1
console.log(b); //2
console.log(c); //3
```

## 部分匹配

** 不匹配的变量默认赋值undefined **

### 变量比“=”右边的值多

```
let [a,b,c] = [,2,3];
console.log(a); //undefined
console.log(b); //2
console.log(c); //3
```
### 变量比“=”右边的值少

```
let [a,b,c] = [1,2,3,4];
console.log(a); //1
console.log(b); //2
console.log(c); //3
```

```
let [a,[b],c] = [1,[2,3],4];
console.log(a); //1
console.log(b); //2
console.log(c); //4
```

## 不定参数匹配

** ...variable是[不定参数](http://www.infoq.com/cn/articles/es6-in-depth-rest-parameters-and-defaults) **

```
let [a,...b] = [1,2,3,4];
console.log(a); //1
console.log(b); //[2,3,4]
```
```
let [a,...b] = [1,2,3,4,5];
console.log(a); //1
console.log(b); //[2,3,4,5]
```

```
let [a,b,...c] = [1];
console.log(a); //1
console.log(b); //undefined
console.log(c); //[],空数组
```

## 支持默认值

```
let [a = 1] = [];
console.log(a); //1
```
特殊地:
```
let [a = 1] = [undefined]; //undefined不能赋值给a
console.log(a); //1
```
```
let [a = 1] = [NaN]; //NaN可以赋值给a
console.log(a); //NaN
```
```
let [a = 1] = [null]; //null可以赋值给a
console.log(a); //null
```

## var,let,const都支持解构赋值

```
var [a,b] = [1,2];
console.log(a); //1
console.log(b); //2
```
```
const [a,b] = [1,2];
console.log(a); //1
console.log(b); //2
```

## 支持变量解构的数据结构--可以迭代(iterator)的结构

科普：[可迭代协议](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)

# 对象解构

## 变量必须与属性同名

```
let {a,b} = {a:1,b:2};
console.log(a); //1
console.log(b); //2
```
否则变量默认赋值undefined
```
let {c} = {a:1,b:2};
console.log(c); //undefined
```

## 次序无关

```
let {b,a} = {a:1,b:2};
console.log(a); //1
console.log(b); //2
```

## 自定义变量名称

```
let {a:newA,b:newB} = {a:1,b:2};
console.log(newA); //1
console.log(newB); //2
```
** 要注意的是: **
```
let {a:newA,b:newB} = {a:1,b:2};
console.log(a); //ReferenceError: a is not defined
console.log(b); //ReferenceError: a is not defined
```
因为
```
let {a,b} = {a:1,b:2};
console.log(a); //1
console.log(b); //2
```
等价于
```
let {a:a,b:b} = {a:1,b:2};
console.log(a); //1，但这里的a其实是第二个a
console.log(a); //2，但这里的b其实是第二个b
```

## 声明与赋值

```
let a;
let {a} = {a:1}; //Identifier 'newA' has already been declared
```

应为:

```
let a;
({a} = {a:1});
```

## 默认值

```
let {a = 0,b = 0} = {c:3,d:4};
console.log(a); //0
console.log(b); //0
```

## 嵌套对象

```
let {a,b:{c}} = {a:1,b:{c:3}}
console.log(a); //1
console.log(c); //2
console.log(b); //ReferenceError: b is not defined
```
## 应用

```
import {Component} from 'react'; //导入
let {log,sin,cos} = Math;
```

# 字符串的解构

## 字符串的数组解构

```
let [a,b,c] = '123';
console.log(a); //"1"
console.log(b); //"2"
console.log(c); //"3"
```

## 字符串的对象解构

```
let {length:a} = '123'; //因为字符串是特殊的对象，且拥有length这一属性
console.log(a); //3
```

# 函数解构

## 完全匹配

### 传入数组

```
function max([a,b]){
    return a>b?a:b;
}
max([1,2]); //结果为2
```

### 传入对象

```
function max({a,b}){
    return a>b?a:b;
}
max({a:1,b:2}); //结果为2
```

## 部分匹配

### 少值
```
function max([a,b]){
    return a>b?a:b;
}
max([1]); //结果为undefined
```

```
function max({a,b}){
    return a>b?a:b;
}
max({a:1}); //结果为undefined
```

### 多值
```
function max([a,b]){
    return a>b?a:b;
}
max([1,2,3]); //结果为2
```

```
function max({a,b}){
    return a>b?a:b;
}
max({a:1,b:2,c:3}); //结果为2
```

## 默认值

### 数组

```
function array2Object([a = 0,b = 0] = []){ //一种是没有传入数组时给一个默认空数组，一种是传入了数组，但是没有包含a或b或a和b，默认给a和b一个0值
    return {a,b};
}
array2Object([1,2]); //{a:1,b:2}
array2Object([1]); //{a:1,b:0}
array2Object([]); //{a:0,b:0}
array2Object();   //{a:0,b:0}
```

```
function array2Object([a,b] = [0,0]){ //一种是没有传入数组时给一个默认有值的数组，一种是传入了数组，但是没有包含a或b或a和b，js引擎给a和b一个undefined值
    return {a,b};
}
array2Object([1,2]); //{a:1,b:2}
array2Object([1]);  //{a:1,b:undefined}
array2Object([]);   //{a:undefined,b:undefined}
array2Object();     //{0,0}
```

### 对象

```
function object2Array({a = 0, b = 0} = {}) { //一种是没有传入对象时给一个默认空对象，一种是传入了对象，但是没有包含a或b或a和b，默认给a和b一个0值
  return [a, b];
}

object2Array({a: 1, b: 2}); // [1, 2]
object2Array({a: 1}); // [1, 0]
object2Array({}); // [0, 0]
object2Array(); // [0, 0]
```

```
function object2Array({a, b} = { a: 0, b: 0 }) { //一种是没有传入对象时给一个默认有值的对象，一种是传入了对象，但是没有包含a或b或a和b，js引擎给a和b一个undefined值
  return [a, b];
}

object2Array({a: 3, b: 8}); // [3, 8]
object2Array({a: 3}); // [3, undefined]
object2Array({}); // [undefined, undefined]
object2Array(); // [0, 0]

```
# 用途

## 交换变量的值

### ES5:

```
var x = 1,y = 2,tmp;
/*三行代码*/
tmp = x;
x = y;
y = tmp;
console.log(x); //2
console.log(y); //1
```

### ES6

```
let x = 1,y = 2;
/*一行代码解决*/
[y,x] = [x,y];
console.log(x); //2
console.log(y); //1
```

## 从函数返回多个值

```
// 返回一个数组

function example() {
  return [1, 2, 3];
}
var [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
var { foo, bar } = example();
```

## 函数参数定义

```
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

## 提取json数据

```
var jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
```

## 函数参数的默认值

```
jQuery.ajax = function (url, {
  async = true,
  beforeSend = function () {},
  cache = true,
  complete = function () {},
  crossDomain = false,
  global = true,
  // ... more config
}) {
  // ... do stuff
};
```

## 遍历Map结构

```
var map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
```

## 输入模块的指定方法

```
const { SourceMapConsumer, SourceNode } = require("source-map");
```
