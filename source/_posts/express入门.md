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
这是一款基于node.js的快速、开发、极简的开发开发框架,也算是目前比较流行的基于node.js的开发框架，可以快速搭建一个完整的网站。使用express之前需要在本机安装node.js环境<!--more-->Express 是一个自身功能极简，完全是由路由和中间件构成一个的 web 开发框架：从本质上来说，一个 Express 应用就是在调用各种中间件
### demo
```
//创建文件夹
mkdir demo
//进入文件夹
cd demo 
//执行初始化，创建出package.json文件，这个过程需要填写一些参数 
npm init 
//安装express依赖,直接安装在本项目需要添加save
npm install express --save
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

### Mock.js
介绍：Mock.js是一款数据生成器，能使前端独立开发，不依赖后端，也可以编写单元测试
使用前就需要通过npm这个包管理器进行安装
`npm install mockjs -g`
对应的api可以通过该网站学习[mockjs](http://mockjs.com/0.1/),这里面也有一个在线数据模版编辑器，能够在线将数据模板调试成功然后复制到我们自己的服务器生成我们的数据
```
//如果我们是使用简单的一个html文件来使用mockjs的话，那么我们直接引入mockjs文件，然后调用里面的方法就行了
<!-- （必选）加载 Mock -->
<script src="http://mockjs.com/dist/mock.js"></script>
<script>
//调用方法
var data = Mock.mock({
    'list|1-10': [{
        'id|+1': 1
    }]
});
$('<pre>').text(JSON.stringify(data, null, 4))
    .appendTo('body')
</script>
```
如果是node.js搭建的环境，那么我们需要通过下载依赖包
```
//引入依赖包
npm install mockjs -g
/使用
var Mock = require('mockjs');
var data = Mock.mock({
    'list|1-10': [{
        'id|+1': 1
    }]
});
console.log(JSON.stringify(data, null, 4))
```
### 学习网站
[mockjs文档](http://mockjs.com/0.1)
[express文档](http://www.expressjs.com.cn/4x/api.html)







