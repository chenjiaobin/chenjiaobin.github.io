---
title: display为table的应用
date: 2017-09-11 23:21:00
tags: 
- display
- table
- cell
- vertical-align
categories:
- 前端
- CSS
---
`display:table`和`display:table-cell`属性指让标签元素以表格和表格单元格的形式呈现，类似于table和td标签。目前IE8+以及其他现代浏览器都是支持此属性的，
但是IE6/7就不行了，需要写对应的hack来解决兼容性的问题<!--more-->
### display:table和table标签的比较
display属性|table标签
-|-
display:table|table
table-row|tr
table-cell|td
table-header-group|thead
table-footer-group|tfoot
table-row-group|tbody
table-column|col
table-caption|caption
### 前言
我们都知道，单元格有一些比较特别的属性，例如元素的*垂直居中对齐*，*关联伸缩*等，所以`display:table-cell`还是有不少潜在的使用价值的，虽说IE6/7不支持此属性，但是幸运的是，IE6/7一些乱糟糟的属性与渲染，我们可以其他方法实现同样或是类似的效果。
与其他一些display属性类似，table-cell同样会被其他一些CSS属性破坏，例如**float, position:absolute**，所以，在使用display:table-cell与*float:left*或是*position:absolute*属性尽量不用同用。设置了display:table-cell的元素对**宽度高度敏感**，对**margin值无反应，响应padding属性**，基本上就是活脱脱的一个td标签元素了。
### 实现两栏适应布局
场景：左边一张图片，右边一行或多行文字
```
1.HTML
<div class="box">
  <a href="#" class="pic"><img src="#"/></a>
  <div class="text">
    <p></p>
  </div>
</div>
2.CSS
.pic{float:left}
.text{display:table-row;*display:inline-block}   //inline-block是为了解决ie8以下浏览器不兼容的问题
```
### 实现等高布局
table表格中的单元格最大的特点之一就是同一行列表元素都等高。所以，很多时候，我们需要等高布局的时候，就可以借助`display:table-cell`属性。说到table-cell的布局，不得不说一下“匿名表格元素创建规则”：
> 匿名表格元素创建规则:比如在一个子元素上设置`display:table-cell`，没有在父元素上设置`display:table-row`，那么浏览器会默认创建出一个行，也就是相当于帮你加上了display:table-row

场景：实现三列等高布局
```
1.HTML
<div class="box">
  <div class="cell"></div>
  <div class="cell"></div>
  <div class="cell"></div>
</div>
2.CSS
.box{display:table-row}
.cell{display:table-cell;width:30%;margin-bottom:-100px;  *padding-bottom:110px; *float:left} //加星号的是解决兼容性问题
```
对于不支持display:table-cell属性的IE6/7浏览器，又当如何解决呢？
我们可以使用**补差等高法**，就是一个一个很大的`margin-bottom`负值配上一个同样大小的`padding-bottom`值
### 大小不固定的图片和多行文字的垂直水平居中
单行文字居中可以通过`line-height`来实现
如果是多行文字垂直居中的话就需要用到`table-cell`了
**实现多行文字垂直居中**
场景：多行文字垂直居中
```
1.HTML
<div class="box">
  <span class="text">这里是多行文字内容</span>
</div>
2.CSS
.box{diplay:table-cell;width:300px;height:500px;vertical-align:middle}
.text{display:inline-block;vertical-align:middle}
```
**实现图片垂直居中**
场景：实现宽高不等的图片垂直居中排列
```
1.HTML
<ul class="box">
    <li>
        <div><img src="#" /></div>
    </li>
</ul>
2.CSS
.box li{float:left; margin-right:13px;}
.box li div{display:table-cell; width:144px; height:144px; border:1px solid #beceeb; font-size:118px; text-align:center; vertical-align:middle;}
.box li div img{vertical-align:middle;}
```
[参考文章](http://www.zhangxinxu.com/wordpress/2010/10/%E6%88%91%E6%89%80%E7%9F%A5%E9%81%93%E7%9A%84%E5%87%A0%E7%A7%8Ddisplaytable-cell%E7%9A%84%E5%BA%94%E7%94%A8/)

作者：陈焦滨#kevin


