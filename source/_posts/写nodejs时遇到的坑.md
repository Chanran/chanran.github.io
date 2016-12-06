---
title: 写nodejs时遇到的坑
date: 2016-11-29 19:16:45
tags: [commonjs,nodejs]
categories: [nodejs]
photos: ['http://p1.bqimg.com/567571/aada741cacad2114.jpg']
---

![nodejs](/img/nodejs.jpg)

# 坑一：模块require

## 在html引入的js文件里require的时候是基于该html文件的。


`目录结构`

```
src/
   index.html
   js/
      index.js
      test.js

```

`src/index.html`

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
    <script src="./js/index.js"></script>
</body>
</html>
```

`src/js/index.js`

```
const test = require('./test');
test();
```

`src/js/test.js`

```
module.exports = function(){
  console.log('you are require src/js/test.js');
}
```

用**nodejs**运行上面的代码，打开**index.html**，会报找不到**./test**模块的错误，将**index.js**的代码改成下面的就行了。

`src/js/index.js`

```
const test = require('./js/test'); //基于index.html所在目录下。
test();
```

## nodejs循环依赖（每种情况不同解决方法）。

> 定义： 当有require()模块循环时，模块返回的时候还没有执行完。
即： a模块 require b模块，b模块也 require a模块。

> 官网例子:

1. 当**main.js先require a.js**时，此时程序进入a.js执行require b.js，然后程序又进入b.js之后b.js尝试require a.js，此时就发现有问题,a.js返回了没有执行完的模块给b.js,即a.js执行到`exports.done =false`就返回给b.js了，所以在b.js里的a.js是没有执行完的。而如果是另一种情况，a.js是用module.exports出来的，那么b.js得到的是一个空对象object{}。（这里有点绕，有空画一张图出来比较好理解）

`a.js`

```
console.log('a starting');
exports.done = false;
const b = require('./b.js');
console.log('in a, b.done = %j', b.done);
exports.done = true;
console.log('a done');
```

`b.js`

```
console.log('b starting');
exports.done = false;
const a = require('./a.js');
console.log('in b, a.done = %j', a.done);
exports.done = true;
console.log('b done');
```

`main.js`

```
console.log('main starting');
const a = require('./a.js');
const b = require('./b.js');
console.log('in main, a.done=%j, b.done=%j', a.done, b.done);
```

`执行结果`

```
main starting
a starting
b starting
in b, a.done = false
b done
in a, b.done = true
a done
in main, a.done=true, b.done=true
```

2. 基本同上，但是main.js有所改动。这里main.js先调用b.js。对比上面main.js先调用a.js。程序入口不一样，程序的执行顺序也不一样，结果也反过来了。

`main.js`

```
console.log('main starting');
const b = require('./b.js');     //对比之前的main.js，这里先调用b.js
const a = require('./a.js');
console.log('in main, a.done=%j, b.done=%j', a.done, b.done);
```

`执行结果`

```
main starting
b starting
a starting
in a, b.done = false
a done
in b, a.done = true
b done
in main, a.done=true, b.done=true
```

### 解决方案1
