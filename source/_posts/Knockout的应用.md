---
title: Knockout
date: 2017-10-11 14:15:00
tags: 
- Knockout
- MVVM
categories:
- 前端
- JS
---
这是一个js的MVVM(Module-view-ViewModel)框架,这个方便了前端开发者可以不用去写大量的dom交互。它的兼容性也比较好，IE6以上和firfox上应用的都没什么问题。
### 使用步骤
1. 引入jQuery.min.js,引入Knockout.js，[Knockout.js下载地址](https://github.com/knockout/knockout/releases)
2. 定义构造函数--实例化--绑定，代码如下
```
//构造函数，定义在标签上要绑定的名字和数据
var ViewModule=function(){
  var self=this;
  self.gg=ko.observable("测试");
  self.userName=ko.observable();
  self.city=ko.observableArray(['深圳','广州','上海']);
  self.hobby=ko.observableArray([{key:'10001',value:'篮球'},{key:'10002',value:'羽毛球'}])
  self.selectedHobby=ko.observable();
}
var currentViewModule=new ViewModule();//实例化
ko.applyBindings(currentViewModule);//绑定
```
```
//html，应用上面js绑定好的数据
<div data-bind="html:gg"></div>//这里面会输出‘测试’
<input type="text" data-bind="value:userName">
<p data-bind="html:userName"></p>//每次在上面的input输完东西后鼠标离开后都会将输入的数据填入到这个p标签里面
<select data-bind="options:hobby,selectedOptions:selectedHobby,optionsCaption:'--请选择--',optionsText:'key',optionsValue:'value'"></select>
<b data-bind="html:selectedHobby"></b>
```
接下来来点比较好用的
###
**点击事件和循环遍历**
```
<table>
	<thead>
		<tr>
			<th>id</th>
			<th>name</th>
			<th>des</th>
		</tr>
	</thead>
	<tbody data-bind="foreach:arr">
		<tr>
			<td data-bind="html:$index"></td>//$index是konckout自带的参数，自动计算个数进行0,1,2,3...
			<td data-bind="html:(name=='月球'?'这是嫦娥的家':name)"></td>//还可以进行条件判断
			<td data-bind="html:des"></td>
		</tr>
	</tbody>
</table>
<button data-bind="click:addOne">添加一个星球</button>
<script>
$(document).ready(function(){
var arr=[
			{id:1,name:"火星",des:"火星探索"},
			{id:2,name:"月球",des:"月球探索"},
			{id:3,name:"地球",des:"人类居住的地方"}
		]
    
  var ViewModule=function(){
  var self=this;
  self.arr=ko.observableArray(arr);
  self.addOne=function(){
     self.arr.push({id:"4",name:"接口的",des:"d快递费"});
     return false;
  }
}
var currentViewModule=new ViewModule();//实例化
ko.applyBindings(currentViewModule);//绑定
})
</script>
```
**自定义输出格式**
```
<input type="text" name="" data-bind="value:year">
<input type="text" name="" data-bind="value:month">
<input type="text" name="" data-bind="value:day">
<input type="text" name="" data-bind="value:date2">
<script>
$(document).ready(function(){
  var ViewModule=function(){
  var self=this;
 self.year=ko.observable("");
self.month=ko.observable("");
self.day=ko.observable("");
//注意：computed和pureComputed的区别就是computed不能双向绑定，就是我现在跟在了year或month或day里面的值会改变date的值，但是改变date里面的值不会改变year和month和day的值，但是pureComputed可以做到
self.date=ko.computed(function(){
  return self.year()+"年"+self.month()+"月"+self.day()+"日"
})
//下面这个比较厉害点
self.date2=ko.pureComputed({
  write:function(value){
    var y=value.indexOf("年");
    var m=value.indexOf("月");
    var d=value.indexOf("日");
    self.year(value.substring(0,y));
    self.month(value.substring(y+1,m));
    self.day(value.substring(m+1,d));
  },
  read:function(){
    return self.year()+"年"+self.month()+"月"+self.day()+"日";
  },
  owner:self		
})
}
var currentViewModule=new ViewModule();//实例化
ko.applyBindings(currentViewModule);//绑定
})
</script>
```













