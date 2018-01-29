---
title: js执行机制
date: 2018-01-29 13:30:08
tags: 
- Event Loop
- task queue
categories:
- 前端
- JS
---
js执行是单线程的，也就是同一时间只能干一件事。但是HTML5后面又提出了一个web workers的概念，它允许js创建多线程，但是子线程是被主线程控制的，子线程不能操作dom元素，这个新标准并没有改变js单线程的本质
