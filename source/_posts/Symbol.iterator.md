---
title: Symbol.iterator
date: 2018-04-13 10:35:00
tags: 
- Symbol.iterator
- for-of
- for-in
- yield
categories:
- 前端
- JS
---
如果学过设计模式和java的人都知道iterator是什么东西，在Symbol.iterator出现后，JS可以定义自己的迭代器<!--more-->
### iterator
迭代器，它是用于访问集合类的标准访问方法，它可以把访问逻辑从不同类型集合中抽象出来，从而避免向外部暴露集合内部的结构。比如我们访问一个数组可能使用for循环
