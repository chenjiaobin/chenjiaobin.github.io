---
title: vue-resource
date: 2018-03-09 14:15:00
tags: 
- vue-resource
categories:
- 前端
- 框架
---
vue-resource作为vue请求发送的一个插件<!--more-->
### get请求
```
<button @click="getSend">点击发送</button>
method:{
  getSend:function(){
    this.$http.get('../a.json',{b:2,c:3}).then(function(res){
      //成功
    },function(res){
      //失败
      console.log(res.status)
    })
  }
}
```
### post请求
```

<button @click="postSend">点击发送</button>
method:{
  postSend:function(){
  //emulateJSON设置头，post必须设置
    this.$http.get('../a.json',{b:2,c:3},{emulateJSON:true}).then(function(res){
      //成功
    },function(res){
      //失败
      console.log(res.status)
    })
  }
}
```
### 跨域请求
```
<button @click="jsonSend">点击发送</button>
method:{
  jsonSend:function(){
    this.$http.jsonp('../a.json',{
      params:{
        a:1,
        b:2
      },
      jsonp:'cb'//这个值要看后台返回的回调函数名是什么，默认callback
    }).then(function(res){
      //成功
    },function(res){
      //失败
      console.log(res.status)
    })
  }
}
```
### vue-resource的另外一种配置发送请求
```
<button @click="getSend">点击发送</button>
methods:{
  getSend:function(){
    this.$http({
      url:'',
      data:{
        a:1,
        b:2
      },
      method:'post'
      jsonp:'cb'
    })
  }
}

```
