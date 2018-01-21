---
title: art-template
date: 2018-01-21 23:40:00
tags: 
- art-template
- 前端模版引擎
categories:
- 知识库
---
art-template的诞生方便了前端对于页面数据的渲染，可以直接通过特定的语法来实现模板数据的渲染，不用通过字符串拼接的方式来进行渲染页面<!--more-->js模版引擎的诞生使得前端的数据和页面分离出来了，它采用预编译方式让性能有了质的飞跃，并且充分利用 javascript 引擎特性，使得其性能无论在前端还是后端都有极其出色的表现。
### 特性
* 渲染性能极强
* 调试友好：语法、运行时错误日志精确到模板所在行；支持在模板文件上打断点（Webpack Loader）
* 支持 Express、Koa、Webpack
* 支持模板继承与子模板
### 使用
* 安装tmodjs `npm install -g tmodjs`,这个是为了生成template.js用的，将art-template的语法处理的数据解析成浏览器能认识的数据
* 在项目中新建一个template文件夹
* 在template文件夹里面新建一个模板文件，比如header.html,然后再这里面写art-template的逻辑语法和数据渲染
```
<ul>
	<li>{{menu1}}</li>
	<li>{{menu2}}</li>
	<li>{{menu3}}</li>
</ul>
```
* 在项目根目录里面新建一个index.html,这个文件是一个demo，为了将我们的模版文件header.html渲染上去
* 在index.html引入`<script type="text/javascript" src="./template/build/template.js"></script>`这个是我们后面生成的一个文件
* 在index.html里面写ajax请求，比如
```
$.ajax({
			url:'http://192.168.1.109:3000/jdapi',
			type:'get',
			cache:false,
			dataType:'jsonp',
			success:function(data){
				var jdModule=template('header',data);//这里面这个header的名字就是我们刚才新建的模板文件的名字，data就是我们请求来的json数据
        $(".box").html(jdModule);//jdModule是返回的模板，数据也已经渲染好了
			},
			error:function(xhr,status,errorThrown){
				console.log(xhr.status);
				console.log(status);
			}
		})
```
* 最后通过cmd进入template文件夹`cd template`,执行`tmod .`,这个时候在template里面会新建出一个build文件夹和package.json文件，build里面有一个template.js文件，这个文件就是art-template将我们的模板文件解析压缩成这个文件
### 语法
* 普通取值
```
{{value}}
{{data.key}}
{{data['key']}}
{{a ? b : c}}
{{a || b}}
{{a + b}}
{{set temp = data.sub.content}}
```
* 逻辑判断
```
	{{if index == 0 }} 
		index = 0
	{{else if index > 0 && index < 5}}
		index 大于 0 并且 index 小于 5
	{{else}}
		index = {{index}}
	{{/if}}
```
* 循环
```
//persionList是一个数组，index是循环的下标，person是每次循环的item
{{each personList as person index}}
	    <li>{{index}} : {{person.name}} - {{person.age}}</li>
	{{/each}}
```
* 引入字模板
```
{{include './grammarSub'}} 
```
* 模板继承
 ```
 {{extend './layout.art'}}
{{block 'head'}} ... {{/block}}
//lay.art
<!--layout.art-->
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>{{block 'title'}}My Site{{/block}}</title>

    {{block 'head'}}
    <link rel="stylesheet" href="main.css">
    {{/block}}
</head>
<body>
    {{block 'content'}}{{/block}}
</body>
</html>
<!--index.art-->
{{extend './layout.art'}}

{{block 'title'}}{{title}}{{/block}}

{{block 'head'}}
    <link rel="stylesheet" href="custom.css">
{{/block}}

{{block 'content'}}
<p>This is just an awesome page.</p>
{{/block}}
//渲染 index.art 后，将自动应用布局骨架。
 ```
上面这种语法是标准的语法，art-template的原始语法处理逻辑能力更强,法标准语法则可以让模板易读写
```
<% if (user) { %>
  <h2><%= user.id %></h2>
<% } %>

<%= value %>
<%= data.key %>
<%= data['key'] %>
<%= a ? b : c %>
<%= a || b %>
<%= a + b %>
<% var temp = data.sub.content; %>
```
### art-template结合其他框架使用
首先先安装组件
`npm install art-template --save`
### 学习文档
[art-template中文文档](https://aui.github.io/art-template/zh-cn/docs/)
[art-template基础](http://www.imooc.com/article/20263)
[art-template语法](http://www.imooc.com/article/20293)
[art-template实战](http://www.imooc.com/article/20334)
