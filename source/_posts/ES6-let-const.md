---
title: ES6(一)--let和const
date: 2017-01-11 11:44:13
tags: [ES6]
catogory: [ES6]
---

# 使用let，不会造成变量提升

## ES5

```
console.log(test); //结果是undefined
var test = 'test';
```
可以理解为:
```
var test;
console.log(test); //结果是undefined
test = 'test';
```

## ES6

```
console.log(test); //结果是ReferenceError:test is not defined
let test = 'test';
```
因为不存在变量提升，可以理解为:

```
console.log(test);
let test; //上不去哩，所以报错了。
test = 'test';
```
# 作用在块级作用域

首先要科普一下块级作用域：
> 在ES6之前javascript是用函数function来划分作用域，于是定义的变量作用在各个函数里，单独或者嵌套，也造成了函数作用域链。

>现在ES6定义了块级作用域，用{}(if、for等使用了{}才具备块级作用域的条件)划分块级作用域，let在块级作用域定义的变量在离开块级作用域的时候会被销毁，所以在块级作用域内定义的变量在块级作用域外访问的话会报引用错误(ReferenceError)

## ES5

```
{
    var a = 1;
}
console.log(a); //结果为1
```

## ES6

```
{
    let a = 1;
}
console.log(a) //结果是ReferenceError: a is not defined
```

# 不允许重复声明变量

## ES5

```
{
    var a = 1;
    var a = 2; //我照样过得好好的～
}
```
## ES6
```
{
    let a = 1;
    let a = 2; //出错啦～,SyntaxError: Identifier 'a' has already been declared
}
```

# 暂时性死区(temporal dead zone，简称 TDZ)

> ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

## 没有let的区块(无论是es5还是es6)

```
{
  tmp = 'abc';
  console.log(tmp); //结果为abc
}
```

```
{
  tmp = 'abc';
  console.log(tmp); //结果为abc
  var tmp; //这里定义的tmp将会变量提升，相当于移动这个语句在tmp = 'abc'上面。
}
```

## 有let的区块

```
{
  // TDZ开始，因为下面出现了let tmp，所以let以上的语句出现暂时性死区
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束，let tmp以下的语句没有暂时性死区，因为这里let tmp没有赋值，所以下面报undefined的错
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
```

## typeof

typeof也会报错

```
{
  typeof test; //ReferenceError: test is not defined
  let test = 2;
}
```

## 隐蔽的暂时性死区

```
function bar(x = y,y = 2){
    return [x,y]
}
bar(); //ReferenceError: y is not defined
```

# 在块级作用域里声明函数

## ES5严格模式下(非严格模式下不会报错)

```
'use strict';
if (true){
    function test(){console.log('test')}
}
test();  //ReferenceError:test is not defined
```

## ES6允许在块级作用域声明函数

### 非严格模式

```
if (true) {
  function test() {console.log('test')}
}
test(); //结果为test
```

### 严格模式

不用var、let、const定义:
```
'use strict';
if (true) {
  function test() {console.log('test')} //由于function没有加var、let、const定义，所以此函数的作用域是全局的
}
test() //结果为test
```

var:
```
'use strict';
if (true){
    var test = function(){console.log('test')} //由于function是用var定义的，所以此函数是当前function作用域，此处为global
}
test() //结果为test
```

let或者const：
```
'use strict';
if (true){
    let test = function(){console.log('test')} //由于function是用let定义的，所以此函数是当前块级作用域，即是{}包括的区域，离开之后变量被销毁
}
test() //结果为ReferenceError: test is not defined
```

# const定义常量

## 不可以重新赋值

```
const test = 1;
test = 0; //TypeError: Assignment to constant variable.
```

## 一旦定义就必须初始化

```
const test; //SyntaxError: Missing initializer in const declaration
```

## const定义的地址不能改变，但是内容可以改变

```
const test = [];
test.push('test');
console.log(test); //['test']
```

## 不存在变量提升

## 有暂时性死区

## 作用在块级作用域
