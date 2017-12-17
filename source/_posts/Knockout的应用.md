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
**多个事件的绑定**
```
<b data-bind="html:count,style:{color:styleColor},visible:count()>3"></b>//千万要记住count()要加上括号
<button data-bind="event:{click:addCount,mouseover:mouseOverEvent,mouseout:mouseOutEvent}">点击我把</button>
<script>
$(document).ready(function(){
    var moduleView=function(){
    	self.count=ko.observable(0);
	self.styleColor=ko.observable("black");
	self.addCount=function(){
		var currentCount=self.count();
		var currentCount=currentCount+1;
		self.count(currentCount);
	},
	self.mouseOverEvent=function(){
		self.styleColor("red");
	},
	self.mouseOutEvent=function(){
		self.styleColor("black");
	}
    }
    var currentModuleView=new modulwView();
    ko.applyBindings(currentModuleView);
})
</script>
```
**定义组件**
关键词：components\register\viewModel\template
```
//两种方式引入组件
<div data-bind="component:'messageList'"></div>//组件名字必须以字符串的方式引入，就是要用引号引入
<div data-bind="component:{name:'messageList',params:'chenjiaobin'}"></div>//可以传递一个params的参数
<script>
$(document).ready(function(){
ko.components.register(
	'messageList',{
		viewModel:function(params){//这个viewModule的名字不能改，必须是这个
			var self=this;
			self.account=ko.observable(params!=null?params:"tom");
			self.message=ko.observable("");
			self.send=function(){
				self.messages.push({mes:self.message(),num:self.account()});
				self.message("");
			};
			self.messages=ko.observableArray([]);
		},
		template:'<input type="text" name="" data-bind="value:message"><button data-bind="click:send">发送</button><ul data-bind="foreach:messages"><li><span data-bind="html:num"></span><span data-bind="html:mes"></span></li></ul>'
	}
	);
	ko.applyBindings();
})
</script>
```
**级联**
```
<select data-bind="options:cityWrap,optionsCaption:'--请选择城市--',optionsText:'name',optionsValue:'code',value:cs"></select>
<select data-bind="options:currentCity,optionsCaption:'--请选择地区--',optionsText:'name',optionsValue:'areaCode',value:dq"></select>
<script>
var viewModel=function(){
	self.cs=ko.observable("");
	self.dq=ko.observable("");
	self.cityWrap=ko.observableArray([
			{name:'北京',code:1001},
			{name:"上海",code:1002}
		]);
	self.area=ko.observableArray([
			{name:'天安门',areaCode:100010,cityCode:1001},
			{name:'天坛',areaCode:100011,cityCode:1001},
			{name:'圆明园',areaCode:100012,cityCode:1001},
			{name:'上海门',areaCode:100013,cityCode:1002},
			{name:'上海侨',areaCode:100014,cityCode:1002}
		]);
	self.currentCity=ko.computed(function(){
		//这个是knock的数组过滤的方法
		return ko.utils.arrayFilter(self.area(),function(item){
			return item.cityCode==self.cs();
		})
	})
}
var currentViewModel=new viewModel();
ko.applyBindings(currentViewModel);
</script>
```












