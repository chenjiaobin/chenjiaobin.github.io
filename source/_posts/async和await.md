---
title: async和await
date: 2018-04-15 15:50:00
tags: 
- async
- await
categories:
- 前端
- JS
---
async和await作为ES7新的API,解决了异步编程过程中的很多问题，在Node.js V7也添加了对这两个API的支持。<!--more-->
### async
如果async函数直接return出来一个普通值`async function a(){return 'hello'}`，执行完a函数后返回的是一个promise对象，也就是说async会把这个直接量通过Promise.resolve封装成一个Promise对象，那么其实我们可以通过then这种方式来接受promise的值，然后在then里面输出来`a().then((v)=>{console.log(v)})`
### await
await这个则是在async内部调用的一个接口，一般后面接的也是一个返回promise的函数，当然也可以是直接量或者是普通函数，<font color="#FF0000">但是，</font>如果await等到的不是promise的话，那么await表达式的运算结果就是它得到的值，如果等到的是Promise对象的话，那么await就忙起来了，它会阻塞后面的代码的执行，等着Promise的resolve，然后得到resolve的值，这个值就是await的值<font color="#ff0000">(这里的阻塞就是await必须用在async里面的原因了，async函数的调用不会造成阻塞，它内部所有的阻塞都是封装在Promise对象中异步执行)</font>
await的promise对象有可能运行结果是rejected，所以最好把await命令放在try...catch里面
```
async function test(){
  try{
     await demo();
  }.catch(err){
    console.log(err)
  }
}
//另外一种写法是
async function test(){
  await demo().catch{function(err){console.log(err)}}
}
```
await 命令只能用在 async 函数之中，如果用在普通函数，就会报错。
```
async function dbFuc(db) {
  let docs = [{}, {}, {}];

  // 报错
  docs.forEach(function (doc) {
    await db.post(doc);
  });
  //上面代码会报错，因为 await 用在普通函数之中了。但是，如果将 forEach 方法的参数改成 async 函数，也有问题。
  async function dbFuc(db) {
  let docs = [{}, {}, {}];

  // 可能得到错误结果
  docs.forEach(async function (doc) {
    await db.post(doc);
  });
}
//上面代码可能不会正常工作，原因是这时三个 db.post 操作将是并发执行，也就是同时执行，而不是继发执行。正确的写法是采用 for 循环。
```
上面的这个例子就给我们引出了另外一个问题，**并行处理**
```
async function dbFuc(db) {
  let docs = [{}, {}, {}];
  for (let doc of docs) {
    await db.post(doc);
  }
}
//如果确实希望多个请求并发执行，可以使用 Promise.all 方法。
async function dbFuc(db) {
  let docs = [{}, {}, {}];
  let promises = docs.map((doc) => db.post(doc));

  let results = await Promise.all(promises);
  console.log(results);
}

// 或者使用下面的写法

async function dbFuc(db) {
  let docs = [{}, {}, {}];
  let promises = docs.map((doc) => db.post(doc));

  let results = [];
  for (let promise of promises) {
    results.push(await promise);
  }
  console.log(results);
}

```
### async/await的优势
async/await 的优势在于处理 then 链
单一的 Promise 链并不能发现 async/await 的优势，但是，如果需要处理由多个 Promise 组成的 then 链的时候，优势就能体现出来了（很有意思，Promise 通过 then 链来解决多层回调的问题，现在又用 async/await 来进一步优化它）。
```
//单一的Promise
function task(){
  return new Promise(resolve=>{
    setTimeout(()=>{resolve('这个是延时显示的内容')},1000)
  })
}
task().then(v=>{
  console.log(v);
})
//用await代替实现
async function demo(){
  const v=await task();
  console.log(v);//这个只有等到上面await函数的内容执行完才会到这一步，这就是被阻塞了
}

```
从上面看出来其实单一的promise发挥不出async的优点。
假设一个业务，分多个步骤完成，每个步骤都是异步的，而且依赖于上一个步骤的结果。我们仍然用 setTimeout 来模拟异步操作：
```
/**
 * 传入参数 n，表示这个函数执行的时间（毫秒）
 * 执行的结果是 n + 200，这个值将用于下一步骤
 */
function takeLongTime(n) {
    return new Promise(resolve => {
        setTimeout(() => resolve(n + 200), n);
    });
}

function step1(n) {
    console.log(`step1 with ${n}`);
    return takeLongTime(n);
}

function step2(n) {
    console.log(`step2 with ${n}`);
    return takeLongTime(n);
}

function step3(n) {
    console.log(`step3 with ${n}`);
    return takeLongTime(n);
}
//用普通的promise来实现
function doIt() {
    console.time("doIt");
    const time1 = 300;
    step1(time1)
        .then(time2 => step2(time2))
        .then(time3 => step3(time3))
        .then(result => {
            console.log(`result is ${result}`);
            console.timeEnd("doIt");
        });
}
doIt();
//可以看出promise不断的通过then调用下一个函数来完成自己的目的
//通过async/await来实现
async function doIt() {
    console.time("doIt");
    const time1 = 300;
    const time2 = await step1(time1);//只有这一步执行完才会执行下一步，这样下一步就能去到上一步的返回的值了
    const time3 = await step2(time2);
    const result = await step3(time3);
    console.log(`result is ${result}`);
    console.timeEnd("doIt");
}
doIt();

```
### Generator与yield
async 函数是 Generator 函数的语法糖
```
//通过generator和yield来实现文件的读取
var fs = require('fs');

var readFile = function (fileName) {
  return new Promise(function (resolve, reject) {
    fs.readFile(fileName, function(error, data) {
      if (error) reject(error);
      resolve(data);
    });
  });
};

var gen = function* () {
  var f1 = yield readFile('/etc/fstab');
  var f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
//接下来通过async/await来实现
var asyncReadFile = async function () {
  var f1 = await readFile('/etc/fstab');
  var f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```
<font color="#ff0000">async函数就是将 Generator 函数的星号（*)替换成async，将yield替换成await，仅此而已。</font>
**async的优点**
* 内置执行器：generator需要依靠执行器，所以才有了CO函数库(这个是TJ大神写的一个使用生成器函数来解决异步流程问题，可以看做是生成器函数执行器),但是async有自带执行器
* 语义更好
* 更广的适用性:co函数库规定，yield后面跟的是Thunk函数或Promise对象,但是await后面则可以是promise对象或者原始数据类型(数值\字符串\布尔值，但是这是属于同步操作)
### 参考文档
[文档1](https://segmentfault.com/a/1190000007535316)
[文档2](https://www.cnblogs.com/zczhangcui/p/6636885.html)
[文档3](http://www.ruanyifeng.com/blog/2015/05/async.html)
