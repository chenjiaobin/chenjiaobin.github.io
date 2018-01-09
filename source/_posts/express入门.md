---
title: node和Express
date: 2018-01-09 23:40:00
tags: 
- node.js
- Express
categories:
- 前端
- 框架
---
###Express介绍
这是一款基于node.js的快速、开发、极简的开发开发框架,使用express之前需要在本机安装node.js环境<!--more-->
### demo
```
//创建文件夹
mkdir demo
//进入文件夹
cd demo 
//执行初始化，创建出package.json文件，这个过程需要填写一些参数 
npm init 
//安装express依赖,直接安装在本项目需要添加save
nom install express --save
//全局安装express方式为
npm install -g express
//创建一个index.js文件,不一定是index.js，这要看你在init的时候填写的入口文件是什么
在index.js里面写如下代码
var express=require('express');
var app=express();
app.get('/',function(req,res){
	res.send('chenjiaobin Hellow world');
});
var server=app.listen(3300,function(){
	var host=server.address().address;
	var port=server.address().port;
	console.log("example app listeneing at",host,port);
})
//最后执行打开服务
node index.js
//浏览器访问
localhost:3300
```
### express自动生成框架
```
//先全局安装一个express应用生成器
npm install express-generator -g
//创建demo文件夹
mkdir demo
//进入文件夹
cd demo
//生成开发框架
express demoexpress
//进入demoexpress文件夹
cd demoexpress
app.js //入口文件
bin //配置文件夹
public //静态资源文件夹
routes //路由文件夹
view //页面文件夹，页面格式为jade
//安装依赖，就是将package.json现有的依赖安装
npm install
//开启服务
npm start
//可以修改标题，要进入/routes/index.js,里面有title可以更改
//页面上访问,端口可以通过在bin/www这里面的这个www文件里面查看
localhost:3000
//但是这样的话，每次更改页面内容都得去重新启动服务npm start,那么我们可以通过 supervisor这个依赖来解决这个问题，它会实时监控每个文件的更改
//安装这个依赖
npm install -g supervisor
//启动服务不需要npm start了，直接执行
supervisor  ./bin/www

```
简单的demo就做好了
