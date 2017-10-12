---
title: Ajax与Commet
date: 2017-10-11 14:15:00
tags: 
- Ajax
- comet
categories:
- 前端
- JS
- Ajax
---
Ajax的主要作用是实现在不刷新页面的情况下实现局部异步更新数据。<!--more-->
### 原生js实现ajax请求代码
```
//get请求方式
var xmlhttp
function xml(){
  if(window.XMLHttpRequest){
    xmlhttp=new XMLHttpRequest();
  }else{
    xml=new ActicveXObject();
  }
  xmlhttp.onreadystatechange=function(){
    var responseText=JSON.parse(xmlhttp.responseText);
    //这里返回数据后的处理过程了，我就不写了
    ........
  }
  xmlhttp.open("GET","http://www.baidu.com",true);//第三个参数表示时候异步，true就是异步
  xmlhttp.send(null);//这里面可以传递一个参数，如果不要作为请求主体发送的数据，就不需要加上这个参数，这个参数需要序列化之后才能传进去，即stringify
}

//POST请求
function xml(){
  if(window.XMLHttpRequest){
    xmlhttp=new XMLHttpRequest();
  }else{
    xml=new ActicveXObject();
  }
  xmlhttp.onreadystatechange=function(){
    var responseText=JSON.parse(xmlhttp.responseText);
    //这里返回数据后的处理过程了，我就不写了
    ........
  }
  xmlhttp.open("POST","http://www.baidu.com",true);//第三个参数表示时候异步，true就是异步
  xmlhttp.setRequestHeader("Content-Type","application/x-www-form-urlencoded");//如果是post请求的话就得加上这一句，因为加上这一句的就可以像表单一样获取到对应的参数，比如在php中就可以通过$_post这个超级全局变量获取到信息，如果不加这一句的话就在再$_post找不到数据
  xmlhttp.send(null);//这里面可以传递一个参数，如果不要作为请求主体发送的数据，就不需要加上这个参数，这个参数需要序列化之后才能传进去，即stringify
}
```
### readyState各状态代表什么
* 0：未初始化，尚未调用open()方法
* 1：启动，已经调用open方法，尚未调用send方法
* 2：发送，已经抵用send方法，尚未收到响应
* 3：接收，已经接收到部分数据，
* 4：接收全部完成
### JQuery实现ajax请求
```
$.ajax({
  type:"POST",
  url:"/api",
  data:{
    name:"chen",
    age:"bibi"
  },
  dataType:"json", //可以死json,xml,string
  async:true, //默认true
  success:function(data){
  
  },
  error:function(xhr){
    console.log(xhr.state);//获取到错误码400等
  }
  complete:function(xhr,status){
    //第一个参数是xhr的所有方法，比如complete、abort等
    //第二个参数表示的是success或者error
  },
  beforeSend:function(){
    //这个是最先执行的，如果spiner图片可以在数据为拿到之前显示这个图片
  },
  cache:false //如果数据经常变化的话就最好设置成false
})
```
### JQuery的get请求
```
//第二个参数会在执行过程中自动加到/api后面，即/api?action=get&name=chen
//$.get(url,[data],[callback])

$.get("/api",{action:"get",name:"chen",function(data,textStatus){
//如果成功的话textState的值是success,只有成功才能进入这个回调函数
  console.log(data);
}})
```
### JQery单个post方式请求
```
//$.post(url,[data],[callback],[type]) type是客户端请求的类型json,xml等

$.post("/api",{action:"get",name:"chen",function(data,textStatus){
//如果成功的话textState的值是success,只有成功才能进入这个回调函数
  console.log(data);
}})
```

### JSON
概念：JavaScript的对象变现法
优点：读写速度快，简短，javascript内建的方法直接解析stringify和parse
注意：键值对必须用`双引号`，不能用单引号
json的值可以是：数组，对象，数字，字符串，null
* JSON.parse()
这个的作用是将JSON字符串` '{"name":"chen"}' `转换成javascript的json对象也就是普通的对象`{name:"chen"}`,他可以传递一个参数，这个参数对每个键值对都会进行过滤筛选
```
var a={"name":"chen","age":12}
var b=JSON.parse(a,function(key,value){
  if(key==="age"){
    value=14;
  }
  return value;
})
console.log(b);
最后输出的是{name:"chen",age:14}
```
* JSON.stringify()
这个的作用是将对象转换成json字符串，也就是**序列化**的过程。可以传递其他两个参数，第一个参数是过滤器，可以是数组，也可以是一个函数
```
var a={"name":"chen","age":12}
//函数的时候
var b=JSON.stringify(a,function(key,value){
  if(key==="age"){
    value=14;
  }
  return value;
})
console.log(b);
最后输出的是{name:"chen",age:14}

//数组的时候
var b=JSON.stringify(a,["name"]);
console.log(b)
最后输出的是{"name":"chen"}

//第三个参数是字符串缩进
var b=JSON.stringify(a,null,"--");
console.log(b)
最后输出的是
{
--"name":"chen",
--"age":12
}
```

### JSON和XML的相互转换
需要下载：jquery,jquery.json2xml.js,jquery.xml2json.js
* xml转换成json对象
```
var str="";
str+="<root>";
str+="<name>chen</name>";
str+="<age>32</age>";
str+="</root>";
var obj=$.xml2json(str);
console.log(str);//结果是{name:"chen",age:32}
var json=JSON.stringify(obj)//输出是{"name":"chen","age":32}
```
* json对象转换成xml
```
var a={name:"chen",age:43};
$.json2xml(a);//输出是<root><name>chen</name><age>43</age></root>
```
### GeoJSON和TopoJSON
测试工具：geojson.io
这两种是符合JSON数据格式的表示地理信息
* GeoJSON
```
{
  "type":"point", //表示点,LineString表示面
  "coordinates":[-105,39]
}
```
### 跨域
当协议，子域名，主域名，端口号任意一个不同就是跨域
javascript的同源策略是对XHR的主要约束，他为通信设置了“相同的域、相同的端口、相同的协议”，视图访问上述限制外的资源会引发安全错误，那么就需要跨域解决方案，这个解决方案就是**CORS(跨源资源共享)** IE8通过XDR（XDomainRequest）对象支持CORS，其他浏览器通过XHR对象原生支持CORS。图像PING和JSONP也类似于CORS，也是解决跨域通信的技术。
### 解决跨域
* 代理，主要是在后端人员操作，比如北京有一台服务器www.beijin.com 想访问上海服务器 www.shanhai.com 那么我们就只需要在在北京服务器上做一个代理服务器即与北京服务器是同源的www.beijin.com/prox 然后通过这个代理去访问上海服务器的数据，然后我们前端只需要获取数据只要访问www.beijin.com/prox 就好了
* JSONP，可用于解决主流浏览器的跨域数据访问问题（不支持post请求,还有就是JSONP死是从其他域中加载代码执行，如果其他域不安全，那么web服务就不安全了）
例子1
```
在www.aaa.com页面中声明一个函数
<script src="http://www.bbb.com/jsonp.js"></script>
<script>
  function jsonp(json){
    console.log(json["name"]);
  }
</script>
在www.bbb.com页面中
jsonp({"name":"chen"})
以上的执行时a域名声明，b域名调用
```
例子2
```
//前端
$.ajax({
  type:"GET",
  url:"/api",
  data:{
    name:"chen",
    age:"bibi"
  },
  dataType:"jsonp",//改成jsonp
  jsonp:"call",//call随便都可以
  success:function(data){
  
  },
  error:function(xhr){
    console.log(xhr.state);//获取到错误码400等
  }
})

//后端php
$jsonp=$_GET["call"];
$result=$jsonp.'({"success":false,"msg":"错误"})'; //返回的数据必须用$.jsonp.({...})连接起来
```
* XHR2

### Comet
Ajax是一种从页面向服务器请求数据的技术，而Comet则是一种服务器向页面推送数据的技术
两种实现Comet的方式：**长轮询和流**。长轮询是传统轮询的翻版
**长轮询和短轮询的区别**
短轮询是服务器立即发送响应，无论数据是否有效，
长轮询是页面发起请求，服务器一直保持连接打开，知道有数据发送，发送完数据之后，浏览器关闭连接，随即又发起到一个到服务器的新请求
HTTP流，流不同于上述两种轮询，因为它在页面的整个生命周期内只使用一个HTTP连接，就是浏览器向服务器发送一个请求，而服务器保持连接打开，然后周期性的向浏览器发送数据
