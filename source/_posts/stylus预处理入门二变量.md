---
title: stylus预处理入门(二)--变量
date: 2016-11-01 13:54:34
tags: [stylus,预处理器,css,前端]
categories: [css]
photos: ['http://p1.bqimg.com/567571/8390f3bdbc787465.png']
---

![stylus](/img/stylus.png)

# 变量(variables)
>**变量的标识符可以由$、下划线、字母、数字组成，其中数字不能作为变量的开头。其实这里的变量只是理解为单纯的字符替换**

**外部定义变量：**
```
_font-size = 14px
font = _font-size "Lucida Grande", Arial

body
  font font, sans-serif
```
转化：
```
body {
  font: 14px "Lucida Grande", Arial, sans-serif;
}
```
---
 **下面是变量的另外一种用法，不在外部定义变量:**
- 内部重新定义当前变量

```
 #logo
   width: w = 150px
   height: h = 80px
   margin-left: (w / 2)
   margin-top: (h / 2) /*注意括号一定要加上*/
```
转化：
```
#logo {
  width: 150px;
  height: 80px;
  margin-left: 75px;
  margin-top: 40px;
}
```
---
- 使用内部变量

```
 #logo
   width: w = 150px
   height: h = 80px
   margin-left: (@width / 2)
   margin-top: (@height / 2) /*注意括号一定要加上*/
```
转化：
```
#logo {
  width: 150px;
  height: 80px;
  margin-left: 75px;
  margin-top: 40px;
}
```
---
> **变量的深入用法：写在mixins里，与分支结构配合定义初始属性值等。**

```
  position()
    position: arguments
    z-index: 1 unless @z-index /*这里是mixins，如果不懂可以先忽略。*/

  #logo
    z-index: 20
    position: absolute

  #logo2
    position: absolute
```
转化：
```
#logo {
  z-index: 20;
  position: absolute;
}
#logo2 {
  position: absolute;
  z-index: 1;
}
```
---

> **变量冒泡查询**

先看一个栗子：
```
   body
    color: red
    ul
      li
        color: blue
        p
            color:black
            a
                background-color: @color
```
转化：
```
body {
  color: #f00;
}
body ul li {
  color: #00f;
}
body ul li p {
  color: #000;
}
body ul li p a {
  background-color: #000;
}
```
> 注：从当前层开始，一直往上层查询，直到找到为止，找不到则返回null。上面例子中选择器"body ul li p"就定义了color属性了，而且选择器a被"body ul li p"嵌套的。所以查询到了上层的color属性，停止查询。

---

> QUOTE: If you are not moving ahead , you are falling behind.
