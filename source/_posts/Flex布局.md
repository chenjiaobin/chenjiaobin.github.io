---
title: Flex布局
date: 2018-01-17 16:04:00
tags: 
- Flex
- 布局
categories:
- 前端
- CSS
---
在网页布局没有进入 CSS 的时代，排版几乎是通过 table 元素实现的，在 table 的单元格里可以方便的使用 align、valign 来实现水平和垂直方向的对齐，随着 Web 语义化的流行，这些写法逐渐淡出了视野，CSS 标准为我们提供了 3 种布局方式：标准文档流、浮动布局和定位布局。<!--more-->这几种布局能够解决常见的布局问题，然而，这些写法都存在一些缺陷：缺少语义并且不够灵活。我们需要的是通过 1 个属性就能优雅的实现子元素居中或均匀分布，甚至可以随着窗口缩放自动适应。在这样的需求下，CSS 的第 4 种布局方式诞生了，这就是我们今天要重点介绍的 flex 布局。
### Flex容器属性
* flex-direction
* flex-wrap
* flex-flow
* justify-content
* align-items
* align-content
### Flex容器属性详解
1. 大容器设置
```
.container{
  display:flex;
  display:-webkit-flex;
}
```
1. flex-direction
```
//项目排列方向
flex-direction:row|column|row-reverse|column-reverse;
```
2. flex-wrap
```
//设置如果一条轴线排不下，如何换行
flex-wrap:wrap|nowrap|wrap-reverse;
```
3. flex-flow
```
//这个是flex-direction和flex-wrap的结合
flex-flow:<flex-direction> || <flex-wrap>;
flex-flow:row nowrap
```
4. justify-content
```
//设置主轴的对齐方式
justify:flex-start|flex-end|center|space-around|space-between
```
5. align-items
```
//设置交叉轴的对齐方式
align-items:flex-start | flex-end | center | baseline | stretch;
```
6. align-content
```
//该属性设置多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用
align-content: flex-start | flex-end | center | space-between | space-around | stretch;
```
### Flex项目属性
* order
* flex-grow
* flex-shrink
* flex-basis
* flex
* align-self
### Flex项目属性详解
1. order
```
//order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0
order:1;
```
2. flex-grow
```
//flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍
flex-grow:1
```
3. flex-shrink
```
//flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。负值对该属性无效
flex-shrink:1
```
4. flex-basis
```
//flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。
flex-basis: <length> | auto; //auto 是默认值
```
5. flex
```
//这个属性是flex-grow和flex-shrink和flex-basis的简写
flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
//该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。
```
6. align-self
```
//align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。
 align-self: auto | flex-start | flex-end | center | baseline | stretch;
```
### 例子
1. 圣杯布局
```
//html代码
<div id="box">
  <header></header>
  <div class="content">
    <main class="main"></main>
    <nav class="nav"></nav>
    <aside class="side"></aside>
  </div>
  <footer></footer>
</div>
//css代码
.box{
  display:flex;
  display:-webkit-flex;
  flex-flow:column row;
}
.content{
  flex:1;
  display:flex;
  display:-webkit-flex;
}
header,footer{
  flex:1;
}
.main{
  flex:1;
}
.nav,.side{
  flex:0 0 12em;
}
.nav{
  order:-1;
 }
```
2. 主栏的左侧或右侧，需要添加一个图片栏。(左边是图片，右边是文字)
```
<div class="Media">
  <img class="Media-figure" src="" alt="">
  <p class="Media-body">...</p>
</div>

.Media {
  display: flex;
  align-items: flex-start;
}
.Media-figure {
  margin-right: 1em;
}
.Media-body {
  flex: 1;
}
```
3. 固定的底栏
情景：有时，页面内容太少，无法占满一屏的高度，底栏就会抬高到页面的中间。这时可以采用Flex布局，让底栏总是出现在页面的底部。
```
<body class="Site">
  <header>...</header>
  <main class="Site-content">...</main>
  <footer>...</footer>
</body>

html,body {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
}

.Site-content {
  flex: 1;
}
```
### 参考文章
[文章1](http://www.runoob.com/w3cnote/flex-grammar.html) | [文章2](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html) | [文章3](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)
