---
title: css3动画笔记
date: 2016-11-01 13:34:41
tags: [css3,动画,animation]
categories: [css]
photos: ['http://p1.bpimg.com/567571/8cce04f88da90eb5.jpg']
---

![css](/img/css.jpg)

# some sites
- [css3动画文档](http://www.w3school.com.cn/css3/css3_animation.asp) by W3CSchool
- [css3动画简介](http://www.ruanyifeng.com/blog/2014/02/css_transition_and_animation.html) by阮一峰
- [css3动画手册](http://isux.tencent.com/css3/) by 腾讯 isux
- [css3动画之硬件加速](http://www.w3cplus.com/css3/introduction-to-hardware-acceleration-css-animations.html) by w3cplus
- [调试css3 动画 keyframe](http://www.w3ctech.com/topic/1472) by w3ctech
- [css动画的性能优化](http://zencode.in/14.CSS%E5%8A%A8%E7%94%BB%E7%9A%84%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96.html) by zencode.in
- [浏览器是如何渲染页面的](http://code.leozhang2018.me/2016/03/07/%E9%9D%A2%E5%90%91%E5%88%9D%E5%AD%A6%E8%80%85%E7%9A%84%E5%89%8D%E7%AB%AF%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E9%9B%86%E9%94%A6/) by code.leozhang2018.me (读完了之后会更加懂得如何提升css动画性能)
- [前端开发体系建设日记](https://github.com/fouber/blog/issues/2) by 张云龙 （这个链接是作为拓展的）

# transition
- grammar: tansition: property duration timing-function delay;
- detail
  - transition-property：要过渡的css属性
  - transition-duration：要过渡持续多少秒或者毫秒
  - transition-timing-function：速度效果的速度曲线
  - transition-delay：延迟多少秒执行过渡
- notice
  - 默认值：transition:all 0 ease 0
  - 如果transition-duration属性没有被设置，则默认为0，即不会产生过渡效果
  - tanstion-timing-function: linear/*匀速*/ | ease-in /*加速*/ | ease-out /*减速*/ | ease /*逐渐放慢*/ | cubic-bezier /*函数，自定义速度模式，可以使用 [工具网站](http://cubic-bezier.com/) 这个网站制作*/
- compatibility
  - 目前，各大浏览器（包括IE 10）都已经支持无前缀的transition，所以transition已经可以很安全地不加浏览器前缀。
  - 不是所有的CSS属性都支持transition，完整的列表查看[这里](http://oli.jp/2010/css-animatable-properties/)，以及具体的[效果](http://leaverou.github.io/animatable/)。
  - transition需要明确知道，开始状态和结束状态的具体数值，才能计算出中间状态。比如，height从0px变化到100px，transition可以算出中间状态。但是，transition没法算出0px到auto的中间状态，也就是说，如果开始或结束的设置是height: auto，那么就不会产生动画效果。类似的情况还有，display: none到block，background: url(foo.jpg)到url(bar.jpg)等等。
- advantage : 简单易用
- disavantage
  - transition需要事件触发，所以没法在网页加载时自动发生。
  - transition是一次性的，不能重复发生，除非一再触发。
  - transition只能定义开始状态和结束状态，不能定义中间状态，也就是说只有两个状态。
  - 一条transition规则，只能定义一个属性的变化，不能涉及多个属性。

# animation
- grammar: name duration timing-function delay iteration-count direction
- detail
  - @keyframes：规定动画
  - animation-name：绑定选择器的keyframe名称
  - animation-duration：动画的持续时间，以秒或者毫秒计
  - animation-timing-function：动画的速度曲线
  - animation-delay：延迟多少秒执行动画
  - animation-iteration-count：动画播放次数
  - animation-diretion：是否应该轮流反向播放动画
  - animation-play-state(通常用在js控制是否播放,object.style.animationPlayState)：（paused[停止] | running[播放]）规定动画正在播放还是暂停
  - animation-fill-mode(通常用在js控制动画效果是否可见,object.style.animationFillMode)：（none[不改变默认行为] | forwards[当动画完成后，保持最后一个属性值] | backwards[在animation-delay 所制定的一段时间内，在动画显示之前，应该开始属性值] | both[向前和向后填充模式都被应用]）规定动画在播放之前或之后，其动画效果是否可见。
- compatibility
  - Internet Explorer 10、Firefox 以及 Opera 支持 @keyframes 规则和 animation 属性。Chrome 和 Safari 需要前缀 -webkit-
  - Internet Explorer 9，以及更早的版本，不支持 @keyframe 规则或 animation 属性
- notice
  - 默认值：animation:none 0 ease 0 1 normal
  - 尽量少在@keyframe里使用除了transform,opacity,filter以外的元素，因为会触发浏览器的重绘(repaint)[详情](http://www.w3cplus.com/css3/introduction-to-hardware-acceleration-css-animations.html)
  - @keyframe
    - 定义：以百分比来规定改变发生的时间，或者通过关键词"from" 和 "to"，等价于0%和100%，0%是动画的开始时间，100%是结束时间
    - grammar:@keyframes animationname{ keyframes-selector { css-styles;} }
    - detail
      - animationname：（必需）定义动画名称
      - keyframes-selector：（必需）动画时长的百分比，合法的值：0%-100%（可以使用 from[0%] 和 to[100%]）
      - css-styles：（必需）一个或多个合法的css样式属性
    - example

---
              @keyframes mymove
              {
                0%   {top:0px;}
                25%  {top:200px;}
                50%  {top:100px;}
                75%  {top:200px;}
                100% {top:0px;}
              }
              @-moz-keyframes mymove /* Firefox */
              {
                0%   {top:0px;}
                25%  {top:200px;}
                50%  {top:100px;}
                75%  {top:200px;}
                100% {top:0px;}
              }

              @-webkit-keyframes mymove /* Safari 和 Chrome */
              {
                0%   {top:0px;}
                25%  {top:200px;}
                50%  {top:100px;}
                75%  {top:200px;}
                100% {top:0px;}
              }

              @-o-keyframes mymove /* Opera */
              {
                0%   {top:0px;}
                25%  {top:200px;}
                50%  {top:100px;}
                75%  {top:200px;}
                100% {top:0px;}
              }
---

  - 如果animation-duration属性没有设置，则时长为0，即动画不会被播放
  - animation-iteration-count默认值为1，可设置为infinite（无限次播放）
  - animation-direction:normal /*正常播放*/ | alternate /*轮流反向播放*/
- advantage ：解决了transition过渡效果不能循环播放的弊端
- disvantage：目前，IE 10和Firefox（>= 16）支持没有前缀的animation，而chrome不支持，所以必须使用webkit前缀。代码必须写成下面这样。

---
          div:hover {
            -webkit-animation: 1s rainbow;
            animation: 1s rainbow;  
          }

          @-webkit-keyframes rainbow {
            0% { background: #c00; }
            50% { background: orange; }
            100% { background: yellowgreen; }
          }

          @keyframes rainbow {
            0% { background: #c00; }
            50% { background: orange; }
            100% { background: yellowgreen; }
          }
---

# transform
- grammar:transform:none | transform-functions
- detail:none and transform-functions
  - none：不定义转换
  - matrix(n,n,n,n,n,n)：定义2D转换，使用六个值的矩阵
  - matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n)：定义3D转换，使用16个值的4X4矩阵
  - translate(x,y)：定义2D转换（参数：x轴移动量，y轴移动量。+即向右移动，-即向左移动。单独一个量时表示x轴移动量与y轴移动量相等）
  - translate3d(x,y,z)：定义3D转换（参数基于translate(x,y)扩展）
  - translateX(x)：定义转换，只用于X轴的值
  - translateY(y)：定义转换，只用于Y轴的值
  - translateZ(z)：定义转换，只用于Z轴的值
  - scale(x,y)：定义2D缩放转换（参数x,y是倍数）
  - scale(x,y,z)：定义3D缩放转换（参数基于scale(x,y)扩展）
  - scaleX(x)：设置X轴的值来定义缩放转换
  - scaleY(y)：设置Y轴的值来定义缩放转换
  - scaleZ(z)：设置Z轴的值来定义缩放转换
  - rotate(angle)：定义2D旋转。在参数中规定角度（正是顺时针，负是逆时针）
  - rotate3d(x,y,z,angle)：定义3D旋转
  - rotateX(angle)：定义沿着X轴的3D旋转
  - rotateY(angle)：定义沿着Y轴的3D旋转
  - rotateZ(angle)：定义沿着Z轴的3D旋转
  - skew(x-angle,y-angle)：定义沿着X和Y轴的2D倾斜转换
  - skew(angle)：定义沿着X轴的2D倾斜转换
  - skew(angle)：定义沿着Y轴的2D倾斜转换
  - perspective(n)：为3D转换元素定义透视视图
- compatibility
  - Internet Explorer 10、Firefox、Opera 支持 transform 属性
  - Internet Explorer 9 支持替代的 -ms-transform 属性（仅适用于 2D 转换）
  - Safari 和 Chrome 支持替代的 -webkit-transform 属性（3D 和 2D 转换）
  - Opera 只支持 2D 转换
  - Internet Explorer 10 和 Firefox 支持 3D 转换
  - Chrome 和 Safari 需要前缀 -webkit-
  - Opera 仍然不支持 3D 转换（它只支持 2D 转换）
- notice
  - transform-origin可以定义改变被转换元素的位置
    - grammar：transform-origin:x-axis,y-axis,z-zxis
    - 默认值：transform-origin:50% 50% 0
    - detail
      - x-axis | y-axis：定义视图被置于X轴的何处。可能的值：left | center | right | length | %
      - z-axis：定义视图被置于Z轴的何处。可能的值：length
    - compatibility
      - Internet Explorer 10、Firefox、Opera 支持 transform-origin 属性
      - Internet Explorer 9 支持替代的 -ms-transform-origin 属性（仅适用于 2D 转换）
      - Safari 和 Chrome 支持替代的 -webkit-transform-origin 属性（3D 和 2D 转换）
      - Opera 只支持 2D 转换
  - transform-style规定如何在3D空间中呈现被嵌套的元素
    - grammar:transform-style:flat | preserve-3d
    - tranform-style默认值：transform-style:flat
    - compatibility
      - Firefox 支持 transform-style 属性
      - Chrome、Safari 和 Opera 支持替代的 -webkit-transform-style 属性
  - perspective定义3D元素距视图的距离，以像素计。
    - grammar:perspective number | none
    - perspective默认值：perspective:none
    - detail
      - 当为元素定义 perspective 属性时，其子元素会获得透视效果，而不是元素本身
      - perspective 属性只影响 3D 转换元素
      - 通常和perspective-origin属性配合改变3D元素的底部位置
    - compatibility
      - 目前浏览器都不支持 perspective 属性
      - Chrome 和 Safari 支持替代的 -webkit-perspective 属性
  - perspective-origin定义 3D 元素所基于的 X 轴和 Y 轴。该属性允许您改变 3D 元素的底部位置
    - grammar:perspective-origin:x-axis,y-axis
    - perspective-origin默认值：perspective-origin:50% 50%
    - detail
      - 当为元素定义 perspective-origin 属性时，其子元素会获得透视效果，而不是元素本身
      - 该属性必须与 perspective 属性一同使用，而且只影响 3D 转换元素
    - compatibility
      - 目前浏览器都不支持 perspective-origin 属性
      - Chrome 和 Safari 支持替代的 -webkit-perspecitve-origin 属性
  - backface-visibility定义当元素不面向屏幕时是否可见
    - grammar:backface-visibility: visible | hidden
    - backface-visibility默认值：backface-visibility:visible
    - detail
      - 如果在旋转元素不希望看到其背面时，该属性很有用
    - compatility
      - 只有 Internet Explorer 10+ 和 Firefox 支持 backface-visibility 属性
      - Opera 15+、Safari 和 Chrome 支持替代的 -webkit-backface-visibility 属性

------
> QUOTE: If you are not moving ahead , you are falling behind.
