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
* grid-auto-flow
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
6. justify-content和align-content
```
//如果用像px非弹性单位定义的话，总网格区域大小有可能小于网格容器，这时候你可以设置网格的对齐方式，一般如果想不出现这种情况我们可以通过fr和百分比等方式来避免
.container{
display:grid;
justify-content:start|end|center|space-between|space-around|stretch|space-evenly(网格项间隙相等)
}
//下面是行对齐，上面是列对齐
.container{
display:grid;
align-content:start|end|center|space-between|space-around|stretch|space-evenly(网格项间隙相等)
}
```
7. grid-auto-columns和grid-auto-rows
自动生成隐式网格
```
//生成一个2x2的表格
.container{
  display:grid;
  grid-template-columns:60px 60px;
  grid-template-rows:50px 50px;
}
//item-a定义的表格位置是存在的
.item-a{
  grid-column:1/2;
  grid-row:2/3;
}
//item-b定义的表格区域是不存在的
.item-b{
  grid-column:5/6;//这个网格是不存在的，所以达不到我们的效果，那
  grid-row:2/3;
}
//解决方法：我们可以在.container容器里面添加grid-auto-column:60px;这样就会帮我们隐式创建出这个表格了
```
8. grid-auto-flow
```
//这个是用来试着网格项下排列方式的
.cotainer{
  display:grid;
  grid-auto-flow:row|column|dense(按先后顺序排列)
}
```
9. grid
```
//这个是其他几个容器属性的集合
.cotainer{
  display:grid;
  grid:none | <grid-template-rows> / <grid-template-columns> | <grid-auto-flow> [<grid-auto-rows> [ / <grid-auto-columns>] ]
}
```
接着就是介绍一下项目属性了
### Grid项目属性
* grid-column-start
* grid-column-end
* grid-row-start
* grid-row-end
* grid-column
* grid-row
* grid-area
* justify-self
* align-self
### Grid项目属性详解
1. grid-column-start和grid-column-end和grid-row-start和grid-row-end和grid-column和grid-row
```
//则几个都是以网格线来定位的
.item-a{
  grid-column-start:1;
  grid-column-end:2;
  grid-row-start:1;
  grid-row-end:2;
}
//相当于
.item-a{
  grid-column:1/2;
  grid-row:1/2;
}
//span引用
.item-c{
  grid-column:1 / span 2; //合并1,2,3个网格
  grid-row:1 / 2;
}
```
2. grid-ara
```
//可以定义区域的名字，结合grid-template-areas使用
.item-a{
  grid-area:header;
}
//也可以通过他获取一个区域
// grid-area:<row-start> / <column-start> / <row-end> / <column-end> ;
.item-b{
  grid-area:1/2/1/2 ;
}
```
3. align-self和justify-self
```
//设置网格项的对齐方式
start: 网格区域左对齐。
end: 网格区域右对齐。
center: 网格区域居中。
stretch: 网格区域填满
```
### [参考博文](https://www.jianshu.com/p/d183265a8dad)











