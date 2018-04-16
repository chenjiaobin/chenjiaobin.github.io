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
迭代器，它是用于访问集合类的标准访问方法，它可以把访问逻辑从不同类型集合中抽象出来，从而避免向外部暴露集合内部的结构。比如我们访问一个数组可能使用for循环或者map,foreach,filter等`for(int i=0; i<array.size(); i++) { ... get(i) ... } `， 但是当我们想要遍历链表(linkedlist)的时候就得使用while循环`while((e=e.next())!=null) { ... e.data() ... }`, 以上两种方式我们都必须知道集合的内部结构是怎么样的我们才可以使用对应的循环方式去循环整个集合，那么这样就造成了很大的耦合度，当我们把一个集合的类型从Arrarlist变成Linkedlist的时候，那么原来客户端的代码必须重写，因为我们集合变了，遍历的方式也必须改成对应的方式。为解决以上问题，Iterator模式总是用同一种逻辑来遍历集合： `for(Iterator it = c.iterater(); it.hasNext(); ) { ... } `
### for-of和for-in的区别
遍历数组通常使用for循环，ES5的话也可以使用forEach，ES5具有遍历数组功能的还有map、filter、some、every、reduce、reduceRight等，只不过他们的返回结果不一样。但是使用foreach遍历数组的话，使用break不能中断循环，使用return也不能返回到外层函数
```
var myArray=[1,2,4,5,6,7]
myArray.name="杀马特"
Array.prototype.method=function(){
　　console.log(this.length);
}
for (var index in myArray) {
  console.log(myArray[index]);
}
```
使用for in 也可以遍历数组，但是会存在以下问题：
1. index索引为字符串型数字，不能直接进行几何运算
2. 遍历顺序有可能不是按照实际数组的内部顺序
3. 使用for in会遍历数组所有的可枚举属性，包括原型。例如上栗的原型方法method和name属性
所以for in更适合遍历对象，不要使用for in遍历数组。

那么除了使用for循环，如何更简单的正确的遍历数组达到我们的期望呢（即不遍历method和name），ES6中的for of更胜一筹.
```
var myArray=[1,2,4,5,6,7]
myArray.name="杀马特"
Array.prototype.method=function(){
　　console.log(this.length);
}
for (var index of myArray) {  //这里的index输出的是value而不是索引
  console.log(index);
}
```
<font color="#dd0000">注意:for-of目前js实现的对象有array，string，argument以及后面更高级的set，Map</font>
当我们遍历对象的时候可以使用for-in，不过这种遍历方式会把原型上的属性和方法也给遍历出来，当然我们可以通过`hasOwnProperty`来过滤掉除了实例对象的数据，但是for-of在object对象上暂时没有实现，但是我们可以通过Symbol.iterator给对象添加这个塑性，我们就可以使用for-of了，代码如下
```
var p={
	name:'kevin',
	age:2,
	sex:'male'
}
Object.defineProperty(p,Symbol.iterator,{
	enumberable:false,
	configurable:false,
	writable:false,
	value:function(){
		var _this=this;
		var nowIndex=-1;
		var key=Object.keys(_this);
		return {
			next:function(){
				nowIndex++;
				return {
					value:_this[key[nowIndex]],
					done:(nowIndex+1>key.length)
				}
			}
		}
	}
})
}
//这样的话就可以直接通过for-of来遍历对象了
for(var i of p){
  console.log(i)
}
输出的是：kevin,2,male
```
其实for-of的原理最终也是通过调用p\[Symbol.iterator]()这个函数，这个迭代器函数返回一个next函数，for循环会不断调用next
那么知道原理之后，我们可以自己来调用iterator.next来实现循环
```
var students = {}
students[Symbol.iterator] = function() {
  let index = 1;
  return {
    next() {
      return {done: index>100, value: index++}
    }
  }
}
var iterator = students[Symbol.iterator]();
var s=iterator.next();
while(!s.done) {
  console.log(s.value);
  s=iterator.next();
}
```
上例中使用 iterator.next 和 while 结合实现了 for循环。
除了使用iterator 之外，我们还可以使用 yield 语法来实现循环，yield相对简单一些，只要通过 yield 语句把值返回即可：
```
let students = {
  [Symbol.iterator]: function*() {
    for(var i=0;i<=100;i++) {
      yield i;
    }
  }
}
for(var s of students) {
  console.log(s);
}
//这个yield其实最后返回的就是iterator函数
```
### 参考文档
[文档1](https://blog.csdn.net/lihongxun945/article/details/48952017)
[文档2](https://blog.csdn.net/gjc9620/article/details/47681271)
