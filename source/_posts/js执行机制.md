---
title: js执行机制
date: 2018-01-29 13:30:08
tags: 
- Event Loop
- task queue
categories:
- 前端
- JS
---
js执行是单线程的，也就是同一时间只能干一件事。但是HTML5后面又提出了一个web workers的概念，它允许js创建多线程，但是子线程是被主线程控制的，子线程不能操作dom元素，这个新标准并没有改变js单线程的本质。<!--more-->
所有任务可以分为同步任务和异步任务
- 同步任务：在主线程上执行的任务，只有当上一个任务结束后才能执行下一个任务
- 异步任务：这里的任务没有进入主线程，而是在任务队列里面，只有当**任务队列**通知主线程的时候，这个异步任务才会进入主线程进行执行
### 异步执行的运行机制
- 所有同步任务在主线程里面执行，形成一个**执行栈**
- 主线程之外还有一个**任务队列(task queue)**,只要异步任务有了运行结构，就在“任务队列里面放置一个事件”
- 一旦“执行栈”所有同步任务执行结束后，系统就会读取“任务队列”，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行
- 主线程不断重复上面的第三步
### Event Loop
主线程从“任务队列”中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为Event LOOP(事件循环),event loop有两种，一种是在浏览器上下文的，一种是在work上下文的，浏览器上下文至少有一个event loop,像一个iframe，浏览器窗口都会有一个event loop,而对于work上下文的则比较简单，work进程管理着一个event loop
### Event Loop的运行流程
```
1.在最老的任务队列中取出最老的任务，如果没有任务就直接跳入microtask队列中
2.将event loop的当前任务设置成最老的任务
3.执行当前任务队列中最老的任务
4.将当前运行任务设置成null
5.在队列中将刚刚执行的那个任务移除
6.执行microtask队列算法
7.更新渲染
8.如果是一个work event loop，但是没有任务，并且workerglobalscrope对象的closing flag为true的，就销毁event loop并终止这些步骤，并执行web worker的run worker算法
9.返回到第一步，这就形成了一个event loop事件循环的机制
```
由上面的步骤可以看出，没执行一次任务都会去清空microtask队列，也就是第6步
### Microtask与Macrotask的区别
一次event loop(事件循环)就会取一个macrotask执行，但是会将一个microtask队列清空，也就是如，microtask队列如果太长的话就会导致阻塞下一个macrotask的执行，在异步中，虽然js是异步非阻塞的，但是却是使用**同步**的方式来执行micortask。另外从字面上来说，macrotask属于大型的任务，microtask属于小任务
```
macrotask: setTimeout\setInterval\setImmediate\I/O
microtask: promist/process.nextTick/
```
### 例子
```
console.log("first");
setTimeout(()=>{
  console.log("second");
},0)
Promist.resolve().then(()=>{
console.log("third")
}).then(()=>{
console.log("four")
})
console.log("five")
//最后chrome输出的顺序是first five second third four
```
### js执行机制
js并不是完完全全的一行一行往下执行的，当我们遇到Promise和setTimeout这一类的代码的时候就会发现，因为js里面包含了同步和异步的程序执行方式
```
const first = () => (new Promise((resovle,reject)=>{
    console.log(3);
    let p = new Promise((resovle, reject)=>{
         console.log(7);
        setTimeout(()=>{
           console.log(5);
           resovle(6); 
        },0)
        resovle(1);
    }); 
    resovle(2);
    p.then((arg)=>{
        console.log(arg);
    });

}));

first().then((arg)=>{
    console.log(arg);
});
console.log(4);
```
* 第一轮事件循环
先执行宏任务，主script ，new Promise立即执行，输出【3】，执行p这个new Promise 操作，输出【7】，发现setTimeout，将回调放入下一轮任务队列（Event Queue），p的then，姑且叫做then1，放入微任务队列，发现first的then，叫then2，放入微任务队列。执行console.log(4)，输出【4】,宏任务执行结束。
再执行微任务，执行then1，输出【1】，执行then2，输出【2】。到此为止，第一轮事件循环结束。开始执行第二轮。
* 第二轮事件循环
先执行宏任务里面的，也就是setTimeout的回调，输出【5】。resovle不会生效，因为p这个Promise的状态一旦改变就不会在改变了。 所以最终的输出顺序是3、7、4、1、2、5。
> 概念：宏任务和微任务
### repaint(重绘)和reflow(回流)
- reflow:这个几乎是无法避免的，即某个子元素样式发生改变，直接影响到了其父元素以及往上追溯很多祖先元素包括兄弟元素，这个时候浏览器要重新渲染这个子元素相关联的所有元素的过程称为**回流**
- repaint:这个就是改变子元素不影响到父元素的另一种叫法，叫**重绘**
注意：repaint速度明显快于reflow

### 参考文章
- [文章1](https://www.cnblogs.com/MasterYao/p/5563725.html)
