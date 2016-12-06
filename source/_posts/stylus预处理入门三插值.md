---
title: stylus预处理入门(三)--插值
date: 2016-11-01 13:56:17
tags: [stylus,预处理器,css,前端]
categories: [css]
photos: ['http://p1.bqimg.com/567571/8390f3bdbc787465.png']
---

![stylus](/img/stylus.png)

# 插值(interporation)
> 插值相当于解析表达式或者变量，让它们的值替换插值的位置。
> **注：** 不能用于属性值的插值（但属性值可以使用变量替换）。

- 在css属性名中使用插值

```
partOfProp = radius
value = 10px
div
    border-{partOfProp} value  /*切记属性值这里不可以使用插值*/
```
转化：
```
div {
  border-radius: 10px;
}
```
---
- 在选择器中使用插值

```
selector = div
partOfProp = radius
value = 10px
{selector}
    border-{partOfProp} value
```
转化：
```
div {
  border-radius: 10px;
}
```
---
```
selectors = '#foo,#bar,.baz'

{selectors}
  background: #000
```
转化：
```
#foo,
#bar,
.baz {
  background: #000;
}
```
---
> 高级使用：与mixins配合使用

```
  vendor(prop, args)
    -webkit-{prop} args
    -moz-{prop} args
    {prop} args

  border-radius()
    vendor('border-radius', arguments)

  box-shadow()
    vendor('box-shadow', arguments)

  button
    border-radius 1px 2px / 3px 4px
```
转化
```
  button {
    -webkit-border-radius: 1px 2px / 3px 4px;
    -moz-border-radius: 1px 2px / 3px 4px;
    border-radius: 1px 2px / 3px 4px;
  }
```
---
> 高级使用：与循环迭代(iteration)配合使用

```
table
  for row in 1 2 3 4 5
    tr:nth-child({row})
      height: 10px * row
```
转化：
```
table tr:nth-child(1) {
  height: 10px;
}
table tr:nth-child(2) {
  height: 20px;
}
table tr:nth-child(3) {
  height: 30px;
}
table tr:nth-child(4) {
  height: 40px;
}
table tr:nth-child(5) {
  height: 50px;
}
```
---

> QUOTE: If you are not moving ahead , you are falling behind.
