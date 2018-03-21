---
title: call和apply和bind
date: 2018-03-21 14:15:00
tags: 
- call
- apply
- bind
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
 
