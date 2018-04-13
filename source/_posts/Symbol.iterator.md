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
1.index索引为字符串型数字，不能直接进行几何运算
2.遍历顺序有可能不是按照实际数组的内部顺序
3.使用for in会遍历数组所有的可枚举属性，包括原型。例如上栗的原型方法method和name属性
所以for in更适合遍历对象，不要使用for in遍历数组。
