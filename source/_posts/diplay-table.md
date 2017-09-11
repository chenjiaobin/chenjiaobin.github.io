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
- display-table
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
我们都知道，单元格有一些比较特别的属性，例如元素的*垂直居中对齐*，*关联伸缩*等，所以`display:table-cell`还是有不少潜在的使用价值的，虽说IE6/7不支持此属性，
但是幸运的是，IE6/7一些乱糟糟的属性与渲染，我们可以其他方法实现同样或是类似的效果。
与其他一些display属性类似，table-cell同样会被其他一些CSS属性破坏，例如**float, position:absolute**，所以，在使用display:table-cell与float:left或是
position:absolute属性尽量不用同用。设置了display:table-cell的元素对宽度高度敏感，对margin值无反应，响应padding属性，基本上就是活脱脱的一个td标签
元素了。
