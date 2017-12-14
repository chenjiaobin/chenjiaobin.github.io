---
title: shape-outside元素实现iphone刘海效果
date: 2017-09-25 13:24:01
tags: 
- shape-outside
- polygon
categories:
- 前端
- CSS
---
iphoneX的诞生，让很多人对它的刘海感到非常大的好奇心，那对于我们前端开发的人来说，关注点就是在他的布局上面了，怎么实现字体环绕他进行滚动？让文字不会被刘海挡住。<!--more-->效果图如下：

![image](https://raw.githubusercontent.com/chenjiaobin/chenjiaobin.github.io/Source/themes/raytaylorism/source/css/images/shape.gif)

CSS3里面针对这种特定形状环绕的效果已经支持很久了，CSS3 Shapes和CSS3 Regions都是可以实现的，接下来我们就用shapes来实现这一效果。
shape-outside属性要想生效，本身需要是浮动float元素。
本文demo效果实现使用的是shape-outside:polygon()，通过点坐标勾勒出和齐刘海形状相似的多边形形状，CSS代码如下
```
.shape{
			float: left;
			width: 30px;
			height: 340px;
			shape-outside: polygon(0 0, 0 150px, 16px 154px, 30px 166px, 30px 314px, 16px 326px, 0 330px, 0 0);
    	transition: shape-outside .15s;
		}
```
此时，后面没有设置BFC（块状格式化上下文）的列表元素就会自动环绕这个形状排列，也就是自动避开了齐刘海区域。
接着就是把刘海的图片覆盖在这个刘海区域上面，这样，刘海就做成了。
接着就是实现文字环绕刘海滚动的效果：
由于shape-outside所在的元素是浮动元素，因此，必定会跟着容器一起滚动，我们需要的效果是我们所绘制的这个刘海区域需要是固定的，那么我们就需要监听整个容器的滚动事件，将容器滚动的高度`scrollTop`加载每个刘海的纵坐标上面，具体代码如下：
```
<script type="text/javascript">
	var box=document.getElementById("box"); //这个是外层容器
	var shape=document.getElementById("shape"); //这个是刘海容器
	shape.style.height=box.scrollHeight+"px";  //这个的作用是将刘海的高度始终保持位置固定
	var fn=function(){
		var scrollTop=box.scrollTop; //获取容器滚动的高度
		var shapeOutside = 'polygon(0 0, 0 '+ (150 + scrollTop) +'px, 16px '+ (154 + scrollTop) +'px, 30px '+ (166 + scrollTop) +'px, 30px '+ (314 + scrollTop) +'px, 16px '+ (326 + scrollTop) +'px, 0 '+ (330 + scrollTop) +'px, 0 0)'; //将纵坐标加上scrollTop就实现了刘海固定的效果
		shape.style.webkitShapeOutside = shapeOutside;//兼容性
		shape.style.shapeOutside=shapeOutside;//将更新后的shapeOutside始终保持更新
	};
  //滚动监听
	box.onscroll=function(){
		fn(); 
	}
	fn();
</script>
```
# 兼容性
CSS Shapes的兼容性为Chrome浏览器和Safari浏览器（包括iOS）都是支持的，也就意味着我们是可以在iPhone上使用的，完美。只是需要注意的是在iOS10.2及其之前的版本，CSS Shapes的使用还是需要加webkit私有前缀的，但据说iPhone X至少默认iOS 11，而刘海头交互效果就是针对iPhone X处理的，因此webkit私有前缀不加也没关系。
# demo
```
<!DOCTYPE html>
<html>
<head>
	<title>刘海</title>
	<style type="text/css">
		.box{
			width: 500px;
			height: 450px;
			margin: auto;
			overflow: auto;
			border:2px solid #000;
		}
		.shape{
			float: left;
			width: 30px;
			height: 340px;
			shape-outside: polygon(0 0, 0 150px, 16px 154px, 30px 166px, 30px 314px, 16px 326px, 0 330px, 0 0);
    		transition: shape-outside .15s;
		}
		.icon{
			width: 24px;
			height: 180px;
			background: url(http://www.zhangxinxu.com/study/201709/liu.png) no-repeat left center;
			position: absolute;
			margin-top: 150px;
		}
		.content ul{
			list-style: none;
			margin: 0;
			padding: 0;
		}
		.content li{
			border-bottom: 1px solid #eee;
			padding: 10px;
		}
	</style>
</head>
<body>
<div class="box" id="box">
	<span class="shape" id="shape"></span>
	<span class="icon"></span>
	<div class="content">
		<ul>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
			<li>box1</li>
		</ul>
	</div>
</div>
<script type="text/javascript">
	var box=document.getElementById("box");
	var shape=document.getElementById("shape");
	shape.style.height=box.scrollHeight+"px";
	var fn=function(){
		var scrollTop=box.scrollTop;
		var shapeOutside = 'polygon(0 0, 0 '+ (150 + scrollTop) +'px, 16px '+ (154 + scrollTop) +'px, 30px '+ (166 + scrollTop) +'px, 30px '+ (314 + scrollTop) +'px, 16px '+ (326 + scrollTop) +'px, 0 '+ (330 + scrollTop) +'px, 0 0)';
		shape.style.webkitShapeOutside = shapeOutside;
		shape.style.shapeOutside=shapeOutside;
	};
	box.onscroll=function(){
		fn();
	}
	fn();
</script>
</body>
</html>
```
这样就实现了文字环绕刘海的效果

参考网站：[地址1](http://www.zhangxinxu.com/wordpress/2017/09/css-shapes-outside-iphone-x-head/)|[地址2](http://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=2651226995&idx=1&sn=502119ff834717266d738fcbda8f6af7&chksm=bd495af78a3ed3e127c9a186afe3bf9587a8c4b1dc4c1f11a936f31280ea5771c0d8cd21a949&scene=0#rd)
作 者：陈焦滨&kevin
