---
title: vue-router前端路由的两种实现
date: 2018-03-271 14:15:00
tags: 
- vue-router
- history
- hash
categories:
- 前端
- 框架
---
随着前端应用的业务功能越来越复杂、用户对于使用体验的要求越来越高，单页应用（SPA）成为前端应用的主流形式。<!--more-->大型单页应用最显著特点之一就是采用前端路由系统，通过改变URL，在不重新请求页面的情况下，更新页面视图。
### 知识点
* 在初始化对应的history之前，会对mode做一些校验：若浏览器不支持HTML5History方式（通过supportsPushState变量判断），则mode强制设为'hash'；若不是在浏览器环境下运行，则mode强制设为'abstract'
* VueRouter类中的onReady(), push()等方法只是一个代理，实际是调用的具体history对象的对应方法，在init()方法中初始化时，也是根据history对象具体的类别执行不同操作
### Hashhistory
> #符号本身以及它后面的字符称之为hash，可通过window.location.hash属性读取。它具有如下特点：

1. 虽然hash本身出现在URL,但不会被包括在http请求中。他是用来指导浏览器动作的，对服务器端完全无用，因此，改变hash值不会重新加载页面，只会更新视图但不重新请求页面
2. 可以为hash的改变添加监听事件
```
window.addEventListener('hashchange',funcRef,false);
```
3. 每一次hash改变都会在浏览器的历史记录添加一条记录
**history.push和history.replace**
每次访问新的hash就是通过这两个其中一个将其推到历史访问栈中的，他们的根本区别是，replace并不是将新路由添加到浏览器的栈顶，而是替换掉当前的路由
### Html5 History
这是另外一种路由模式，从HTML5开始，History interface提供了两个新的方法：pushState(), replaceState()使得我们可以对浏览器历史记录栈进行修改：
这两个方法有个共同的特点：当调用他们修改浏览器历史记录栈后，虽然当前URL改变了，但浏览器不会立即发送请求该URL（the browser won't attempt to load this URL after a call to pushState()），这就为单页应用前端路由“更新视图但不重新请求页面”提供了基础。
### 两种模式的比较
hashhistory和html5history的根本区别是第一种模式多了个#号，其实history.pushState相比于直接修改hash主要有一下优势
* pushState设置新的url可以是与当前URL同源的任意URL,而hash只可以修改hash后面的内容，故只可以设置与当前同文档的URL
* pushState可以设置与当前的url一模一样，这样也会被记录到栈中，而hash设置的新值必须与原来的不一样才会触发记录添加到栈中
### history模式的一个问题
我们知道对于单页应用来讲，理想的使用场景是仅在进入应用时加载index.html，后续在的网络操作通过Ajax完成，不会根据URL重新请求页面，但是难免遇到特殊情况，比如用户直接在地址栏中输入并回车，浏览器重启重新加载应用等。
hash模式仅改变hash部分的内容，而hash部分是不会包含在HTTP请求中的：

> http://oursite.com/#/user/id   // 如重新请求只会发送http://oursite.com/

故在hash模式下遇到根据URL请求页面的情况不会有问题。
而history模式则会将URL修改得就和正常请求后端的URL一样

> http://oursite.com/user/id

在此情况下重新向后端发送请求，如后端没有配置对应/user/id的路由处理，则会返回404错误。官方推荐的解决办法是在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。同时这么做以后，服务器就不再返回 404 错误页面，因为对于所有路径都会返回 index.html 文件。为了避免这种情况，在 Vue 应用里面覆盖所有的路由情况，然后在给出一个 404 页面。或者，如果是用 Node.js 作后台，可以使用服务端的路由来匹配 URL，当没有匹配到路由的时候返回 404，从而实现 fallback。

> 转载自[此处](https://zhuanlan.zhihu.com/p/27588422)
