---
title: stylus预处理入门(一)--选择器
date: 2016-11-01 13:50:46
tags: [stylus,预处理器,css,前端]
categories: [css]
photos: ['http://p1.bqimg.com/567571/8390f3bdbc787465.png']
---

![stylus](/img/stylus.png)

---
# 相关网站
- 官网：[stylus](http://stylus-lang.com/)
- 中文文档：[stylus中文版参考文档](http://www.zhangxinxu.com/jq/stylus/) by 张鑫旭
- 理解css预处理器：[css预处理器初识](http://leeluolee.github.io/2013/08/01/mcss-start/) by 栓萝卜的棍子
- 三种css预处理器语言详解：[less|sass|stylus](http://www.oschina.net/question/12_44255) by 开源中国

---
# CSS预处理器语言的个人理解
- 用另外一种可读性高、语法性强的语言来写CSS。stylus可以使用循环、分支、定义变量和函数结构来写css，这样写起来既方便又快捷。
- 通过一种转换机制将这种语法转化为原生css。每种语言的转化机制也有很多种，可以使用各种语言官网的方法转化，例如[less](http://lesscss.org/#using-less)。也可以使用构建工具webpack的[stylus-loader](https://github.com/shama/stylus-loader)来将stylus语言转化为原生css。

---
# stylus的特点
- 官网可以直接将stylus代码放在官网[这一页](http://stylus-lang.com/try.html#?code=body%20%7B%0A%20%20font%3A%2014px%2F1.5%20Helvetica%2C%20arial%2C%20sans-serif%3B%0A%20%20%23logo%20%7B%0A%20%20%20%20border-radius%3A%205px%3B%0A%20%20%7D%0A%7D)测试生成原生css（本人硬加上去的特点）
- 通过缩进来解释语言（喜欢python的geek应该会比较喜欢）。
- 待补充..

```
body
  div
  	color white
```
转化：
```
body div {
	color:#fff;
}
```
---
# stylus的优点
- 类python语法（官网称stylus为pythonic）。
- 语法灵活（可选的括号，冒号，分号等）。

---
# stylus的缺点
- 由于其语法灵活的问题，如果没有团队规范，那么就会带来团队开发混乱，维护起来比较麻烦，各种语法混杂。

---
# stylus语法介绍

## 选择器(selectors)
### **缩进(换行缩进表示语句前进)**

```
body
  color:white;
```
转化：
```
body{
	color:white;
}
```
---
### **同级选择器**
- element,element

```
div
p
	color:white;
```
转化：
```
div,p{
	color:white;
}
```
---
- element element

```
div
	p
    	color:white;
```
转化：
```
div p{
	color:white;
}
```
---
- element >element

```
div
	>p
      color:white;
```
转化：
```
div >p{
	color:white;
}
```

- element +element

```
div
	+p
    	color:white;
```
转化：
```
div +p{
	color:white;
}
```
---
**例外：**
```
foo bar baz
>span
	color:white;
```
>**注：上面代码的foo bar baz编译器解析有可能是"标签 属性 属性"，有可能是"标签 标签 标签"（有可能是自定义的标签）**（编译器只会识别文档结构而不是标签或者属性）

---
建议写成下面这样（在最后的选择器后面加一个逗号[comma]）：
```
foo bar baz,
>span
```
>**注：上面的同级选择器是[这里](http://www.w3school.com.cn/cssref/css_selectors.ASP)的优化，没有提到的其他选择器大多数保留原来的语法，或者与上面的语法类似，当然上面提到的选择器也可以使用原来的语法**

---
### **引用父级选择器**
>**使用&指向父级选择器，有可能是选择器数组，也有可能是单独的一个选择器。**

```
div
p
	color:#FFF;
    &:hover
    	color:#000;
```
转化：
```
div,
p {
  color: #fff;
}
div:hover,
p:hover {
  color: #000;
}
```
---
>**解释：其实"&"可以理解为代替了上一层的选择器，比如上面例子，"&"的上一层是"div,p"，这样说比较好理解。**

下面是引用父级选择器的另一个例子

```
  box-shadow()
    -webkit-box-shadow arguments
    -moz-box-shadow arguments
    box-shadow arguments
    html.ie8 &,
    html.ie7 &,
    html.ie6 &
      border 2px solid arguments[length(arguments) - 1]

  body
    #login
      box-shadow 1px 1px 3px #eee
```
转化：
```
  body #login {
    -webkit-box-shadow: 1px 1px 3px #eee;
    -moz-box-shadow: 1px 1px 3px #eee;
    box-shadow: 1px 1px 3px #eee;
  }
  html.ie8 body #login,
  html.ie7 body #login,
  html.ie6 body #login {
    border: 2px solid #eee;
  }
```
---
>**注：如果想在代码里使用"&"符号而不是stylus的"&"，可以在&字符前加一个反斜杠并加上引号，如下：**

```
.foo[title*='\&']  /*.foo[title*='&']*/
```
---
### **部分引用父级选择器之单层选择器**
>使用^ [N] 引用第N层父级选择器。如果N是正数，这里的第N层指的是最上层上层选择器到第N层选择器，如果N是负数，这里的第N层指的是最上层选择器到倒数第|N|层选择器。

- 有&的情况：

```
.foo
  &__bar
    width: 10px

    ^[0]:hover &
      width: 20px
```
转化：
```
.foo__bar {
  width: 10px;
}
.foo:hover .foo__bar {
  width: 20px;
}
```
---
- 没有&的情况：

```
.foo
  .bar
    width: 10px

    ^[0]:hover &
      width: 20px
```
转化：
```
.foo .bar {
  width: 10px;
}
.foo:hover .foo .bar {
  width: 20px;
}
```
---

- N为0或者正数则从最上层开始到最下层，N为负数则从最下层开始到最上层。其实第N层的选择器是包含了上一层的选择器的，例如下面的例子，第一层选择器是foo,第二层就是foo bar,第三层是foo bar baz，如此类推（官网说的是嵌套）。

```
.foo
  bar
    baz
      width: 10px

      ^[-1]:hover &
        width: 20px
```
转化：
```
.foo bar baz {
  width: 10px;
}
.foo bar:hover .foo bar baz {
  width: 20px;
}
```
---
>**注：如果写在mixins里的话，推荐将N写成负数。因为你并不知道你在调用哪一层（ 有可能还有隐藏的上层选择器）**

---
### **部分引用父级选择器之范围选择器**
>^ [N..M] 引用第N层选择器到第M层选择器组成的选择器。
注：这里有些跟引用单层选择器有点不同，这里的第几层是不包括上层选择器的，具体来看例子感受一下。

```
.foo
  bar
    baz
      test
         width: 10px
         ^[-1]:hover ^[-2..-1]
             width: 20px
```
转化：
```
.foo bar baz test {
  width: 10px;
}
.foo bar baz:hover baz test {
  width: 20px;
}
```
---
### 其他部分引用父级选择器
- 最上层父级选择器(~/)，相当于^ [0]

```
.block
  &__element
    ~/:hover &
      color: red
```
转化：
```
.block:hover .block__element {
  color: #f00;
}
```
---
-  相对父级选择器(../)

```
.foo
  bar
    baz
      test
         width: 10px
         ../:hover ^[-1..-2]
             width: 20px
```
转化：
```
.foo .bar .baz .test {
  width: 10px;
}
.foo .bar .baz:hover .baz .test {
  width: 20px;
}
```
---
-  脱离嵌套的选择器(/)

```
div
p
   span
      color #A7A7A7
      &:hover,
      /.is-hovered
        color #000
```
转化：
```
div span,
p span {
  color: #a7a7a7;
}
div span:hover,
p span:hover,
.is-hovered {
  color: #000;
}
```
>**上面的例子里，转化后的代码.is-hovered已经脱离了嵌套结构了，已经不在任何选择器嵌套里。**

---
### **选择器的值**
>**selector()获取当前嵌套层的值，selectors()获取从最上层到当前层每一层的选择器的list**

```
.foo
  &:hover
       class selector()
       color black
       span
          color white
```
转化：
```
.foo:hover {
  class: '.foo:hover'; /*这个是selector()的值*/
  color: #000;
}
.foo:hover span {
  color: #fff;
}
```
---
```
.a
  .b
    &__c
      content: selectors()
      span
            color white
```
转化：
```
.a .b__c {
  content: '.a', '& .b', '&__c';
}
.a .b__c span {
  color: #fff;
}
```
---

> QUOTE: If you are not moving ahead , you are falling behind.
