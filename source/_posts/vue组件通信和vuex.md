---
title: vuex和vue通信
date: 2018-03-11 16:19:00
tags: 
- vuex
- $emit
- props
- $on
categories:
- 前端
- 框架
---
vue的核心内容是组件库和数据绑定，在组件的状态管理上vue通过vuex进行完美的融合，以及在vue通信上以单向数据流来进行传递数据<!--more-->
### vue通信
vue2.0以上取消了之前的dispatch和broadcast的数据通信方式，也取消了子组件直接修改父组件数据的方式。现在组件之间的通信主要是通过$emit和$on，以及props的方式进行通信
