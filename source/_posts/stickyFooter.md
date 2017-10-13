---
title: stickFooter
date: 2017-10-13 10:15:01
tags: 
- stickyFooter
- flex
categories:
- 前端
- CSS
- stickyFooter
---
如果页面内容不够长的时候，页脚块粘贴在视窗底部；如果内容足够长时，页脚块会被内容向下推送。<!--more-->
### 普通css的解决方案
```
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<style>
	html,body{
		width: 100%;
		height: 100%;
		margin: 0;
		padding: 0;
	}
	.space{
		min-height: 100%;//这个百分比比较重要
	}
	.box{
		background-color: red;
		height: 500px;
		padding-bottom: 32px;//这个是为了当页面高于屏幕宽度时能够保持底部在他下面
	}
	.footer{
		height: 32px;
		background: green;
		margin: -32px auto 0 auto ;
	}
</style>
<body>
	<div class="space">
			<div class="box">这是什么鬼</div>
	</div>
	<div class="footer"></div>
</body>
</html>
```
### FlexBox解决方案
```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<style>
		html,body{
			display: flex;
			flex-flow: column; //重要
			width: 100%;
			height: 100%;
			margin: 0;
			padding: 0;
		}
		.header{
			background-color: red;
			/*height: 100px;*/
		}
		.main{
			background-color: green;
			flex:1; //设置了这个才能使得中间的容器能将剩余的页面撑满，也就是页面始终是占满视口，很重要
		}
		.footer{
			background-color: orange;
			/*height: 100px;*/
		}
	</style>
</head>
<body>
<!--这个header必须有内容才能撑开-->
<div class="header">dfasda</div> 
<div class="main">
	<p>这是什么鬼</p>
</div>
<!--这个footer必须有内容才能撑开-->
<div class="footer">sfdasfs</div>
</body>
</html>
```
