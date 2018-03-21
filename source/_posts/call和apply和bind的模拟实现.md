---
title: call和apply和bind
date: 2018-03-21 14:15:00
tags: 
- call
- apply
- bind
- 函数柯里化
categories:
- 前端
- JS
---
原生js模拟实现call和apply以及bind
### call
```
var foo = {
    value: 1
};
function bar() {
    console.log(this.value);
}
bar.call(foo); // 1
```
其实我们也可以通过以下方式实现（但是我们在对象里面多创建了一个函数的属性）
```
var foo = {
    value: 1,
    bar: function() {
        console.log(this.value)
    }
};
foo.bar(); // 1
```
那么我们可以通过这种思想来模拟实现一个call
1.将函数设为对象的属性
2.执行该函数
3.删除该函数
 ```
 Function.prototype.call2=function(context){
  var context=context||window;
  context.fn=this;//通过这个this获取到绑定bind函数的原函数bar,并在这个contest里面新建一个属性fn存放该函数
  //call还可能传递其他参数过来
  var arg=[];
  for(var i=0,len=arguments.lemgth;i>len;i++){
    arg.push('arguments['+i+']');
  }
  var result=eval('context.fn('+arg+')'); //eval会自动调用Array.toString()
  delete context.fn;
  return result;
 }
 ```
 ### apply
 apply和call很相似，只是传递参数的类型不一样
 ```
 Function.prototype.apply = function (context, arr) {
    var context = Object(context) || window;
    context.fn = this;

    var result;
    if (!arr) {
        result = context.fn();
    }
    else {
        var args = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            args.push('arr[' + i + ']');
        }
        result = eval('context.fn(' + args + ')')
    }

    delete context.fn
    return result;
}
 ```
 ### bind
 > bind() 方法会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。(来自于 MDN )
 例子
 ```
 var foo = {
    value: 1
};
function bar(name, age) {
    console.log(this.value);
    console.log(name);
    console.log(age);

}
var bindFoo = bar.bind(foo, 'daisy');
bindFoo('18'); 
// 1
// daisy
// 18
 ```
 ```
 Function.prototype.bind2=function(context){
    if (typeof this !== "function") {
      throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
    }
    var self=this;
    var arr=Array.prototype.slice.call(arguments,1);//因为第一个参数是作为它运行时的this，所以从第二个开始截取到末尾
    return function(){
        var binArr=Array.prototype.slice.call(arguments);// 这个时候的arguments是指bind返回的函数传入的参数
        return self.apply(context,arr.concat(binArr));
    }
 }
 ```
 这样还不是很全面，我们还可能通过对bind返回的函数进行实例化并传入参数,此时的this指向又发生了变化
 ```
 var value = 2;

var foo = {
    value: 1
};

function bar(name, age) {
    this.habit = 'shopping';
    console.log(this.value);
    console.log(name);
    console.log(age);
}

bar.prototype.friend = 'kevin';

var bindFoo = bar.bind(foo, 'daisy');

var obj = new bindFoo('18');
// undefined
// daisy
// 18
console.log(obj.habit);
console.log(obj.friend);
// shopping
// kevin
 ```
 完整版模拟实现bind
 ```
 Function.prototype.bind=function(){
    if(typeof this == 'Function'){
        throw new Error("this can not a function");
    }
    var self=this;
    var arr=Array.prototype.slice(arguments,1);
    var flagFn=function(){}
    var resFn=function(){
        var binArr=Array.prototype.slice(arguments);
        self.apply(this instanceof resFn?this:context,arr.concat(binArr));  //这里是判断bind返回后的函数是否被实例化,是的话就改变this的指向
    }
    flagFn.prototype=this.prototype;
    resFn.prototype=new flagFn();
    return resFn;
 }
 ```
 ### 函数柯里化
 > 把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术
 ```
 function a(func){
	var arr=Array.prototype.slice.call(arguments,1);
	var fn=function(){
		if(arguments.length===0){
			return func.apply(this,arr);
		}else{
			for(var i=0;i<arguments.length;i++){
				arr.push(arguments[i]);
			}
			return fn;
		}	
	}
	return fn; 
}
function add(){
	return Array.prototype.reduce.call(arguments,function(m,n){return m+n})
}

a(add,1,2,3,4)(23,1)(3)()//37  这里可以不断连接下去
 ```
bind函数的分装也是应用到了这个柯里化的这个技术，它还可以延1. 参数复用；2. 提前返回；3. 延迟计算/运行
### 参考文章
[文章1](https://github.com/mqyqingfeng/Blog/issues/12)
