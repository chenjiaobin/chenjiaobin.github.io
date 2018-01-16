---
title: Grid布局
date: 2017-01-16 22:19:00
tags: 
- Grid
- 布局
categories:
- 前端
- CSS
---
Grid是一款二维网格布局系统。CSS在处理网页布局方面一直做的不是很好。一开始我们用的是table（表格）布局，然后用float(浮动)，position（定位）和inline-block（行内块）布局，但是这些方法本质上是hack，遗漏了很多功能，例如垂直居中。后来出了flexbox(盒子布局)，解决了很多布局问题，但是它仅仅是一维布局，而不是复杂的二维布局，实际上它们（flexbox与grid）能很好的配合使用。Grid布局是第一个专门为解决布局问题而创建的CSS模块。<!--more-->
### 兼容性
Chrom--Firefox--opera--Android--Ios的高版本基本都能兼容，但是还是有部分低版本的浏览器不能识别愈语法，使用需要慎用
### Grid容器属性
* display:grid|inline-grid|subgrid
* grid-template-columns
* grid-template-rows
* grid-row-gap
* grid-column-gap
* grid-gap
* grid-template-areas(配合grid-area使用)
* justify-items
* align-items
* justify-content
* align-content
* grid-auto-columns
* grid-auto-rows
* grid-flow
* grid
### Grid容器属性使用详解
1. display
```
//display使用详解
grid: 生成块级网格
inline-grid: 生成行内网格
subgrid: 如果网格容器本身是网格项（嵌套网格容器），此属性用来继承其父网格容器的列、行大小
```
2. grid-template-columns和grid-template-rows
```
//grid-template-columns用来设置列的个数和列的宽度
grid-template-columns:10px 10px;//表示两列每列的宽度为10px
grid-template-columns:1fr 20px 2fr;//容器宽度先减掉20px然后再将剩余的宽度分成3分，第一个项目占1/3,第三个占2/3
grid-template-columns:repeat(3 20px) 1fr;//repeat主要是用来写重复值用的，这里相当于grid-template-columns:20px 20px 20px 1fr;
grid-template-columns:minmax(100px,300px) 200px;//这个minmax作用是设置项目的最小宽度为100px,最大值为300px,当然也可以是auto
grid-template-columns:repeat(auto-fit,minmax(100px,1fr))//这个相当于设置了media-query，根据设备宽度调整项目排序，主要起作用的是auto-fit
//grid-template-rows也是跟上面这个是一样的，只是变成行，即从上到下排列
```
3. grid-row-gap和grid-column-gap和grid-gap
```
//grid-row-gap和grid-column-gap分别是设置行间距和列间距
//grid-gap是其他两个的结合
//第一个值是列间距，第二个是行间距
grid-gap:10px 20px;
```
4. grid-template-areas
```
//这个是通过别名来设置网格模版的,结合grid-area使用
.contain{
  display:grid;
  grid-template-areas:"header header header header"
                      "main main . sidebar"
                       "footer footer footer footer";
}
.item-a{
  grid-area:header;
}
.item-b{
  grid-area:main;
}
.item-c{
  grid-area:sidebar;
}
.item-d{
  grid-area:footer;
}
```
5. justify-items和align-items
```
//这个是设置列方向的对齐方式
justify-items:start|end|center|stretch(填满)
//这个是设置行方向的对齐方式
align-items:start|end|center|stretch
```















