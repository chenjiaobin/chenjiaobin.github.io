---
title: js设计模式
date: 2017-10-17 16:48:21
tags: 
- 设计模式
- js
categories:
- 前端
- JS
- js设计模式
---
js的设计模式增强我们代码结构的强度，可维护性更好，可复用性也更强。<!--more-->
### 单例模式
单例模式就是保证一个类只有一个实例，实现的方法是先判断实例时候存在，如果存在那么就直接返回实例，否则创建实例。
```
//例子1
//创建xiaowan和xiaomin，xiaowan想通过门铃呼叫xiaomin,那么就要先判断门门铃时候存在吗，不存在就创建，存在就直接按门铃
var xiaomin=(function(){
  var xiaominjia=func(msg){
    this.melling=msg;
  }
  var men;
  var info={
    sendMes:function(){
      if(!men){
        men=new xiaominjia();
      }
      return men;
    }
  }
  return info;
})()
var xiaowan={
  callXiaomin:function(msg){
    var a=xiaomin.sendMes(msg);
    console.log(a.melling);
    a=null;//垃圾回收机制
  }
}

//例子2（实际项目）
//一个页面有很多点击事件，而且每个点击事件可能会需要用到上一个点击事件的数据等，那么我们可以将点击事件封装成一个独立的对象
 var top={
  init:function(){
    this.render();
    this.bind();
  },
  a:4,
  //获取点击事件的元素
 render:function(){
  that=this;
  that.btn=$("#a");
 },
 //绑定点击事件
 bind:function(){
  that=this;
  that.btn.onclick=function(){
    that.test();
  }
 },
 //点击触发的事件
 test:function(){
  a=9;
 }
 }
top.init();
```
###  构造函数模式
构造出特定类型的对象
```
//情景：小明和小王的房门有不一样的门的款式，那么我们就可以传递参数给制造商
function zaomen(huawen){
  if(!(this instanceof zaomen )){
    return new zaomen();
  }
  this.suo="普通";
  var _huawen="普通";
  if(huawen){
    _huawen=huawen;
  }
  this.huawen=_huawen;
  this.create=function(){
    return "【锁头】"+this.suo+"【花纹】"+this.huawen;
 }
}
var a=new zaomen();
console.log(a.create());
var b=new zaomen();
console.log(b.create("酷炫的门"));
```
