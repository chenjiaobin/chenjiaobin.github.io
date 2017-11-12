---
title: canvas
date: 2017-11-12 16:40:00
tags: 
- canvas
categories:
- 前端
- HTML5
---
canvas这个画图标签是HTML5新增的一个标签，非常实用，可以根据需要绘制出各种各样的图形。
### 注：
* 内容实际上是绘制在屏幕上；
* 画布，即Canvas，只是规定了绘制内容时的规则；
* 内容的位置由坐标决定，而坐标是相对于画布而言的
### HTML
```
<body>
  <canvas id="canvas" width="300" height="200"></canvas>
</body>
```
### 绘制矩形
* 步骤：*
* 获得标签(获得canvas标签)
* 获得上下文(getContext)
* 填充颜色的选择(fillStyle)
* 边框颜色的选择(strokeStyle)
* 开始画图形(fillRect或strokeRect)
```
var canvas=document.getElementById('canvas');
		var context=canvas.getContext('2d');
		context.fillStyle='#000';
		context.strokeStyle='orange';
		context.lineWidth='5px';
		context.fillRect(0,0,200,100);
		context.strokeRect(20,20,50,30);
		context.strokeRect(30,30,50,30);//可以重复绘制
```
### 绘制圆形
* 步骤： *
* 获得标签
* 获得上下文
* 开始画图声明(beginPath)
* 绘制图片参数设置(arc(x,y,radius,beginAngle,endAngle,isTrueOrFalse))
* 结束绘制声明(clossePath)
* 设置圆形绘制的填充色或者边框颜色
* 开始绘制(fill()或者stoke())
```
		var canvas1=document.getElementById('canvas1');
		context1=canvas1.getContext('2d');
		context1.fillStyle='#333';
		context1.fillRect(0,0,200,100);//填充画布颜色
		context1.beginPath();//开始绘制圆形
    //参数分别表示横坐标、纵坐标、半径、起始角度、结束角度、是否顺时针方向绘制
		context1.arc(10,10,10,0,Math.PI*2,true);//设置参数
		context1.closePath();
		// context1.fillStyle='rgba(255,0,0,0.25)';
    // context1.fill();
		context1.strokeStyle='#000';
		context1.stroke();
```
* 注：*
如果不加beginPath和closePath的话，如果绘制是放在循环绘制里面的话，后面绘制图形会把前面的都继续重叠绘制上去，如果只绘制一次的话（也就是没有循环那么就没有什么区别）
