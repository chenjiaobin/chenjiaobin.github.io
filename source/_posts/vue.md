---
title: vue
date: 2018-02-11 20:15:00
tags: 
- vue
categories:
- 前端
- 框架
---
vue是当前比较火的框架之一，给我们前端开发减轻了很多dom上的操作以及数据的绑定等，注重逻辑的处理<!--more-->
### vue生命周期是什么
> vue有一个完整的生命周期，从实例的创建、初始化数据、编译模板、挂载dom模板、渲染-更新-渲染、卸载，通俗点讲就是vue从实例创建开始到销毁的过程，就是我们说的生命周期

![vue生命周期图](https://raw.githubusercontent.com/chenjiaobin/chenjiaobin.github.io/Source/themes/raytaylorism/source/css/images/vue.jpg)

**beforeCreate**
> 在实例初始化之后，数据观测(data observer) 和 event/watcher 事件配置之前被调用。

**created**
> 实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。

**beforeMount**
> dom挂载之前被调用

**Mounted**
> el被vm.$el替代，并挂载到实例上去后调用该钩子

**beforeUpdate**
> 在虚拟dom重新渲染和打补丁之前调用

**Updated**
> dom渲染之后调用的钩子,当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态，因为这可能会导致更新无限循环。

**beforeDestroy**
> 实例销毁之前调用的函数

**Destroyd**
> 实例销毁之后调用的函数

### 参考文章
1. [文章1](https://segmentfault.com/a/1190000008570622)
