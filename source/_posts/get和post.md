---
title: get和post
date: 2017-11-07 9:31:00
tags: 
- get
- post
categories:
- 前端
- JS
---
GET和POST的真正区别有哪些，当然浅层的区别大家都知道啦，但是但我们在面试的时候被问到的时候回答大家都知道的那就太没意思了，那么我就带大家来认识一下真正的深层次的区别。<!--more-->
### GET和POST的区别（浅层次）
* GET传递数据是以url方式传递，而POST传递则是把参数放在request body（请求主体）进行发送
* GET请求回退是无害的，而POST请求需要再一次提交请求
* GET请求数据会被主动cache,而post不会，除非注定设置
* GET只能支持url编码，而post没有限制
* GET只接受ASCII,而POST没有限制
* GET传递的参数是有限制的（一般在2kb左右），而post没有限制
但是，这只是他们两公婆之间浅层次的区别，要想自己逼格高一点，那么我们就要进一步了解他较为深层次的区别
### GET和POST的区别（深层次）
HTTP是基于TCP/IP的关于输入如何在万维网进行数据通信的协议，简单点讲HTTP就是交通规则，TCP/IP就是运输车
而GET和POST方法是HTTP上传递数据的方法之二，还有PUT，DELETE等，所以GET和POST的底层也是TCP/IP,也就是说，GET/POST都是TCP链接。GET和POST能做的事情是一样一样的。你要给GET加上request body，给POST带上url参数，技术上是完全行的通的。
当然，运输公司进行装货和卸货都是有成本的，所以不能无限制的在url上添加参数，对于GET请求在request body发送部分数据，有部分服务器是不接受的，也就不会帮你读出数据。
他们之间还有一个*更大*的区别，那就是GET产生一个TCP数据包，而POST产生两个TCP数据包，为什么呢？
GET请求的时候会把head和data一并发送给服务器，服务器响应200;而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok(返回数据)。从中我们也认识到get发送请求的速度还是比较快的。
当然如果在网络比较好的情况下其实发送一次和发送两次没什么区别。
此文结束~~~~~~

参考网址：[地址](https://juejin.im/post/59fc04ecf265da4317697f26?utm_source=gold_browser_extension)
作者：陈焦滨&kevin


