---
title: '[转载]译文《容器组件和展示组件》原作者：Dan Abramov'
date: 2017-02-08 18:20:05
tags: [react]
catogory: [react]
---

转载自:[译文《容器组件和展示组件》原作者：Dan Abramov](http://www.jianshu.com/p/6fa2b21f5df3)

# 前言

最初学习react时，只是认识了它的api与十分好用的jsx语法。现在尝试着用react和meteor写实际webapp时，就不知不觉地习惯了一些react方法。今晚偶然逛到redux作者————**Dan Abramov** 的medium主页，发现他的[《Presentational and Container Components》](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)文章中总结了一种模式。看后，获益匪浅，便将其翻译了，分享给大家。

# 译文

## 容器组件和展示组件

> 容器组件和展示组件名词都来自于redux中文文档。

我在用react写程序时，发现了一种简单好用的模式。如果你也熟悉react，或许它早就被你发现了。[有一篇文章](https://medium.com/@learnreact/container-components-c0e67432e005)讲得很好，但是，我想补充几点。

如果你将组件分成两类，你会发现它们容易更被重用和理解。这两类我称之为容器组件和展示组件。我也听过其他说法，比如"臃肿的"和"苗条的"，"智能的"和"单调的"，"多状态变量"和"单纯的"，"封装物"和"元件"等等。概念不完全一致，却有一样的中心思想。

### 我所说的展示组件：

- 只关心它们的样子。
- 可能同时包含子级容器组件和展示组件，一般含DOM标签和自定的样式。
- 通常用`this.props.children`来包含其他组件
- 不依赖app其它组件，比如flux的actions和stores
- 不会定义数据如何读取，如何改变
- 只通过`this.props`接受数据和回调函数
- 很少有自己的状态变量，即使有，也是UI的状态变量，比如
` toggleMenuOpen`,`InputFocus`
- 一般是函数级组件，除非它们需要状态，lifecycle hooks，优化处理。

> lifecycle hook这个词语很形象，但我不知道怎么翻译得贴切,大概指那些监控组件生命周期里一些关键时刻的函数，比如，我需要在这组件初始化的时候调用某函数，那么我实现一个onInit()接口。react中的componentWillMount之类的函数也许也是lifecycle hook?

- 例子有Page,Sidebar,Story,UserInfo,List。

### 我所说的容器组件：
- 只关心它们的运作方式。
- 可能同时包含子级容器组件和展示组件，但大都不含DOM标签，而含他们自己所用的wrapping div，从不用自己的样式。
- 为展示组件或其他组件提供数据和方法。
- 调用Flux的actions，并且将其作为展示组件的回调函数。
- 维持许多状态变量，通常充当一个数据源。
- 通常由[高阶组件](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750)生成，比如Redux里的connect()，Relay里的createContainer()，Flux Utils里的Container.create()，而非手工写出（译者：可能在meteor中数据是例外吧）
- 例子有UserPage, FollowersSidebar, StoryContainer, FollowedUserList。
我把他们放在不同的文件夹中，以示区别。

## 这种方法的好处

- 分离关注，你可以更好的理解app和UI。
- 更易复用，同样的展示组件可以在不同的状态源、数据源中使用。也可以封装成容器组件，在未来重用它们。
- 展示组件是app的调色板。你可以把它们放到单独的页面，并让设计师来调整它们的样式和结构，而不用改变app的逻辑。单独的页面有静态性，你可以在上面进行[screenshot regression](https://github.com/Huddle/PhantomCSS)测试。
- 这种方法会强迫你去解析布局相关的组件，比如Sidebar, Page, ContextMenu，强迫你去使用this.props.children，而非在不同容器中不断复制jsx那块地方。
记住，react的组件不一定要生成DOM，它们只需要考虑如何设计UI之间的分界与组合关系。利用好这一点。

## 什么时候引入容器？

我的建议是，你最好先做展示组件。当你意识到，有一些中间组件传递了过多的props，有一些组件并不使用它们继承的props而只是将这些props传递给他们的子级，而且每次子级组件需要更多数据时，你都需要重新调整或编写这些中间组件，那么，这时候你可以考虑引入容器组件了。这样做，你可以传递props和方法给末端的子级组件，而不必麻烦一些不相关的中间组件。

这是一个边写边改的重构，所以不必一步到位。你尝试着这种模式，慢慢地会培养起一种对何时引入容器的直觉，就像你知道何时该增加函数一样。我的[redux教程](https://egghead.io/series/getting-started-with-redux)也许会帮助你哦。

## 其他二分法

容器组件和展示组件的分别并不被严格定义，理解这一点很重要。为了对比，我再列举一些相关（但是不同的）的二分法。

### 多状态变量和少状态变量

有些组件用`setState()`，有些不用。容器组件通常多状态变量，而展示组件却不，这不是铁规律。展示组件也可以多状态变量，而容器组件也可以少状态变量。

### 类与函数

自从[React0.14](https://facebook.github.io/react/blog/2015/10/07/react-v0.14.html#stateless-functional-components)，组件可以被声明为类，也可以被声明为函数。定义函数方便，却少了类独有的特性。有些限制或许会在未来消失，但是它们至少是存在的。因为函数容易理解，所以我推荐使用函数，除非你需要那些，现在只有类才有的状态管理，lifecycle hooks，性能优化等特性。

### 纯的不纯的

人们说一个组件纯，是指给予它一样props和state，它保证能输出一样的结果。纯函数可以是类，可以是函数，可以多状态，可以无状态。Another important aspect of pure components is that they don’t rely on deep mutations in props or state, so their rendering performance can be optimized by a shallow comparison in their [shouldComponentUpdate() hook](https://facebook.github.io/react/docs/pure-render-mixin.html)。目前只有类可以定义`shouldComponentUpdate()`，或许将来有改变吧。

展示组件和容器组件都有上述的二分特性。在我看来，展示组件倾向于少状态、纯的函数，容器组件倾向于多状态，纯的类。当然啦，这只是个人观察，而非规则，我也见过完全相反的情况。

不要将展示组件和容器组件当作教条。有些时候，不必划出清晰的线条，也不用觉得划出区分会很困难。如果你分不清某个组件是展示组件还是容器组件，也许是为时尚早。要知道，心急吃不了热豆腐

## 例子

[This gist](https://gist.github.com/chantastic/fc9e3853464dffdb1e3c) by [Michael Chan](https://twitter.com/chantastic) 讨论了这个

# 延伸阅读

[Getting Started with Redux](https://egghead.io/series/getting-started-with-redux)

[Mixins are Dead, Long Live Composition](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750)

[Container Components](https://medium.com/@learnreact/container-components-c0e67432e005)

[Atomic Web Design](http://bradfrost.com/blog/post/atomic-web-design/)

[Building the Facebook News Feed with Relay](http://facebook.github.io/react/blog/2015/03/19/building-the-facebook-news-feed-with-relay.html)
