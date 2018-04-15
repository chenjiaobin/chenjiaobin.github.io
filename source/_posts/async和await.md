---
title: async和await
date: 2018-04-15 15:50:00
tags: 
- async
- await
categories:
- 前端
- JS
---
async和await作为ES7新的API,解决了异步编程过程中的很多问题，在Node.js V7也添加了对这两个API的支持。<!--more-->

如果async函数直接return出来一个普通值async function a(){return 'hello'}，执行完a函数后返回的是一个promise对象，也就是说async会把这个直接量通过Promise.
