---
title: 布局
date: 2018-01-24 14:18:00
tags: 
- 定位
- 流
- 浮动
- flex
- grid
categories:
- 前端
- CSS
---
最常用的布局是定位布局、流布局、浮动布局，flex布局和grid是后面新奇的，flex相对来说兼容性比较好，主要是解决一些一维上的问题，而grid兼容性还不是很好，它主要就是解决二维上的问题<!--more-->
### BFC/IFC/GFC/FFC
CSS2.1只有BFC/IFC,CSS3.0加了GFC/FFC(flex和grid)
### BFC定义
BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。
**BFC规则**
* 内部的Box会在垂直方向，一个接一个地放置。
* Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
```
<body>
<div></div>
<div></div>
</body>
//如果这两个元素同时设置margin，那么会出现重叠的现象，因为他们这两个元素处于同一个BFC里面，如果要解决问题可以将他们分别放到不同的BFC中
<div></div>
<div>
  <div></div>
</div>
```
* 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
* BFC的区域不会与float box重叠。
* BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
* 计算BFC的高度时，浮动元素也参与计算
**哪些元素会生成BFC**
* 根元素
* float属性不为none
* position为absolute或fixed
* display为inline-block, table-cell, table-caption, flex, inline-flex
* overflow不为visible

### IFC
IFC就是属于"内联格式化上下文"，跟BFC差不多

