---
title: 串行和并行
date: 2018-04-19 10:18:00
tags: 
- 串行
- 并行
categories:
- 前端
- JS
---
在实际开发中有时获取数据是相互依赖的，这就需要用到请求中的串行和并行请求了<!--more-->
## 概念
> 串行请求

实际就是一个请求执行完之后再执行另外一个请求

> 并行请求

实际就是几个请求同时执行完成后，再执行某个操作

### 串行
**方式：**串行执行的方式有两种，一种是按顺序写请求函数，然后将其请求设置成同步(async=false)，另外一种就是可以不按顺序写，但是要设置成异步发送(async=true)
```
//串行执行分两种。  
//一是用同步模式async: false，三个ajax请求连着写就可以了。  
$.ajax({
  url:'请求1',
  async:false,  //这个是重点
  success:function(){
    console.log('请求1完成')
  }
})
$.ajax({
  url:'请求2',
  async:false,  //这个是重点
  success:function(){
    console.log('请求2完成')
  }
})
$.ajax({
  url:'请求3'
  async:false,  //这个是重点
  success:function(){
    console.log('请求3完成')
  }
})
//二是用异步模式async: true，三个ajax请求嵌套写，ES6可以通过then链来实现嵌套，ES7可以通过async/await来实现同样的嵌套，会更简单
$.ajax({  
    url: "ajax请求1",  
    async: true,  
    success: function (data) {  
        console.log("ajax请求1 完成");  
        $.ajax({  
            url: "ajax请求2",  
            async: true,  
            success: function (data) {  
                console.log("ajax请求2 完成");  
                $.ajax({  
                    url: "ajax请求3",  
                    async: true,  
                    success: function (data) {  
                        console.log("ajax请求3 完成");  
                    }  
                });  
            }  
        });  
    }  
});  
//用async/await
async function sendAjax(){
  let a=await func1();
  let b=await func2();
  func3(a,b);//这一步会等到前两个请求完成之后才会执行这一步，应为await会阻塞后面代码的执行
}
```
### 并行
**方式：**只能通过异步模式来实现,并设置变量进行计数，ES6的Promise.all也可以实现
```
var num = 0;
function isAllSuccess(){
  num++;
  if(num>=3){
    console.log('三个请求已经完成了')
  }
}
$.ajax({  
    url: "ajax请求1",  
    async: true,  //设置成异步
    success: function (data) {  
        console.log("ajax请求1 完成");  
        isAllSuccess();  //下面的每个请求都需要带上他进行计数
    }  
});  
$.ajax({  
    url: "ajax请求2",  
    async: true,  
    success: function (data) {  
        console.log("ajax请求3 完成");  
        isAllSuccess();  
    }  
});  
$.ajax({  
    url: "ajax请求3",  
    async: true,  
    success: function (data) {  
        console.log("ajax请求3 完成");  
        isAllSuccess();  
    }  
});  
```




