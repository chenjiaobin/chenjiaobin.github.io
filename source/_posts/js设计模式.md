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
### 建造者模式
将一个复杂的对象与表示分离，简单点讲就是我只要提出我的要求具体的事情怎么做我不要管
```
//情景白富美想盖房子，找到包工头，包工头再找工人造房子
function fanzhi(){
  this.chufan="";
  this.ketin="";
  this.toilet="";
}
function baogongtou(){
  this.gaifangzhi=function(){
    
  }
}
funtcion gongren(){
  this._gaichufan=function(){
				console.log("厨房改好了");
			}
			this._gaiketin=function(){
				console.log("客厅盖好了");
			}
			this._gaitoilet=function(){
				console.log("厕所盖好了");
			}
			//交付房子
			this.jiafangzhi=function(){
				var newfanzhi=new fanzhi();
				newfanzhi.chufan="ok";
				newfanzhi.ketin="ok";
				newfanzhi.toilet="ok";
			}
}
var gonren_work=new gongren();
var baogontou_order=new baogongtou();
baogontou_order.gaifagnzhi(gongren);//包工头发出指令

```
### 工厂模式
简单点讲就是解决多个相似的问题，工厂下还有对应的子类，比如一家工厂他下面还有生产衣服的子公司和生产鞋子的子公司，实际开发中我们用的也比较多，比如jquery中的$.ajax(url:'a.php'),a.php就是我这个厂长发出要制造的东西的指令，$.ajax这个方法就是工厂，他负责把这个指令传达给下面的子公司去完成，这里的子公司就是jquery后台代码封装的一些处理方法，也就是做具体的工作
```
//简单的工厂
var factory=function(){

		  }
		  factory.createXMLHttp=function(){
		  	var xmlhttp=null;
		  	if(window.XMLHttpRequest){
		  		xmlhttp=new XMLHttpRequest();
		  	}else if(window.ActiveObject){
		  		xmlhttp=new ActiveObject("Microsoft.XMLHTTP");
		  	}
		  	return xmlhttp;
		  }
		  var hander=function(){
		  	var xmlhttp=factory.createXMLHttp();//具体的操作
		  }
	}
 
 //复杂的工厂
 	var xmlFactory=function(){

	}
	xmlFactory.prototype=function(){
  //这个工厂是其他子工厂的总部，负责分工，不负责制造
		throw new Error('这不是我的工作');
	}
	var childFactory=function(){
		xmlFactory.call(this);
	}
	childFactory.prototype=new xmlFactory();//覆盖父厂的方法，这才是我的本职工作
	childFactory.prototype.constructor=childFactory;
	childFactory.prototype=function(){
		// 这里面才是我真正工作的地方
		if(window.XMLHttpRequest){
		  		xmlhttp=new XMLHttpRequest();
		  	}else if(window.ActiveObject){
		  		xmlhttp=new ActiveObject("Microsoft.XMLHTTP");
		  	}
		  	return xmlhttp;
	}
```
### 代理模式
简单点讲就是通过中介替我们做事
```
//代理模式需要三方
//买家
function maijia(){
  this.namme="小明";
}
//中介
function zhonjie(){}
zhonjie.prototype.maifan=function(){
  new fandong(new maijia()).maifan("20万");
}
function fandong(){
  this.maijia_name=maijia.name;
  this.maifan=function(money){
  	console.log('收到了来自'+this.maijia_name)
  }
}
(new zhongjie).maifan();
```
### 命令模式
用来对方法调用进行参数化的处理和传送，经过这样处理过的方法调用可以在任何需要的时候执行，比如司令发布一个作战指令给连长，连长发布命令给小分队，然后上级可以先将一二分队先上，三四分队后上这就是参数化的处理
```
var lian={}
    //炮兵
    lian.paobin=function(pao_num){
    	console.log(pao_num+"炮"+"开始战斗");
    }
    //步兵出动人数
    lian.bubin=function(bubin_num){
    	console.log(bubin_num+"人")
    }
    //连长接收命令
    lian.lianzhan=function(minlin){
    	lian[minlin.type](minlin.num);
    }
    //司令发出作战指令
    lian.lianzhan({
    	type:"bubin",
	num:200
    })
```
