---
title: React入门爬坑记（一）
date: 2018-08-30 14:15:00
tags: 
- React
categories:
- 前端
- 框架
---
 React是众多前端开发中较为青睐的框架，但是相对于其他，React的学习成本相对比较高，但是在适合的场景下通过React开发出来的产品性能以及结构会比较好<!--more-->
### demo
地址：[https://github.com/chenjiaobin/react_demo](https://github.com/chenjiaobin/react_demo)
### 安装全局依赖
首先在开始使用React之前我们需要安装相应的依赖
- 全局安装React`npm install react -g`
- 全局安装react-dom `npm install react-dom -g`
- 全局安装webpack-dev-server `npm install webpack-dev-server -g`这个是为了作为前端开发的服务
- 全局安装webpack `npm install webpack -g`这个是项目打包工具
### 创建项目以及安装项目依赖
1. 通过`npm init`生成package.json，并填写相应的配置信息，最终生成package.json文件
2. 安装项目依赖
```
"babel-core": "^6.26.3" //这个是babel的核心，一下的babel相关都是为了转码es6和react
"babel-loader": "^7.1.4"
"babel-preset-es2015": "^6.24.1"
"babel-preset-react": "^6.24.1"
"babelify": "^8.0.0"
"html-webpack-plugin": "^3.2.0"  //这个是为了热更新加上的，每次更改代码会重新打包html
"react": "^16.4.0"
"react-dom": "^16.4.0"
"webpack": "^4.10.2"
"webpack-dev-server": "^3.1.4" //项目的微服务
"babel-preset-stage-0": "^6.24.1"  //使用es7feature,在这个demo是为了解决react中static状态报错
"bower-webpack-plugin": "^0.1.9"
"open-browser-webpack-plugin": "^0.0.5" //编译完项目自动打开浏览器
"prop-types": "^15.6.1" //react在15.5.0版本以后就将propType移除，被独立成一个新的包prop-type,所以需要安装prop-types,并在组件中import进来
"react-mixin": "^2.0.2"  //在es6语法的基类中不能使用mixin，所以只能通过第三方插件来解决这个问题
"webpack-cli": "^3.0.1"
```
3. 配置webpack,新建文件webpack.config.js,内容如下
```
var webpack = require('webpack')
var path = require('path')
// 打包html
var HtmlWebpackPlugin = require('html-webpack-plugin');
var OpenBrowserPlugin = require('open-browser-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: path.join(__dirname,'/src/index.js'),//这是入口文件，你需要在src文件夹下创建index.js入口文件
  output: {
    path: path.join(__dirname + '/build'),
    filename: 'bundle.js'
  },
  devtool: 'eval-source-map',
  devServer: {
  	port: 8080,
  	contentBase: './src', //src文件夹里面的内容改变就会重新打包
  	historyApiFallback: true,
  	hot: true,
  	inline: true //devServer有iframe(默认)和inline模式
  },
  module: {
    rules: [
      {
        test: /\.js?$/,//表示要编译的文件的类型，这里要编译的是js文件
        loader: 'babel-loader',//装载的哪些模块
        exclude: /node_modules/,//标示不编译node_modules文件夹下面的内容
        query: {//具体的编译的类型，
            compact: true,//表示不压缩
            presets: ['es2015', 'react', 'stage-0']//我们需要编译的是es6和react,stage-0这个插件是为了解决静态属性static
        }
      }
    ]
  },
  plugins: [
  	new HtmlWebpackPlugin({
  		title: "This is the result",
        filename: "./index.html",
        template: "./index.html"
  	}),
    new OpenBrowserPlugin({ url: 'http://localhost:8080' })
  ]
}
```
4. 创建入口文件,demo文件内容如下
```
var React = require('react')
var ReactDom = require ('react-dom')
import ComponentHeader from  './components/header.js'
import ComponentBody from  './components/bodyIndex.js'
import ComponentBottom from  './components/footer.js'
class Index extends React.Component {
  render () {
    return (
    		<div>
        	<ComponentHeader name="我是阿布"/>
        	<ComponentBody userId={123456} sex="male"/>
        	<ComponentBottom/>
    		</div>
      )
  }
}
ReactDom.render(
    <Index/>,
    document.getElementById('app')
  )
```
5. 再将对应的组件创建出来，详情代码请看[demo](https://github.com/chenjiaobin/react_demo)
6. 开始打包,在控制台执行`webpack-dev-server --devtool eval-source-map --progress --colors --hot --inline --content-base ./build`，这个我已经在package.json配置好了，所以如果在使用demo的时候直接使用`npm run dev`就好了
### React的坑
参考地址： [地址](https://segmentfault.com/a/1190000005863630)
> React.createClass和React.Component的区别(React.createClass在16.0版本以后已经被废除了)
- 语法区别
```
// 注意：组件名字的首字母大写，如Mydemo
import React from 'react'

// 这个是React.createClass的语法
const Mydemo = React.createClass({
    render () {
        return (
            <div>这个是demo</div>
        )
    }
})
export default Mydemo

// 这个是React.Component的语法
export default class Mydemo2 extends React.Component {
    constructor (props) {
        super(props)
    }
    render () {
        return ({
            <div>这个事React.Component的语法</div>
        })
    }
}
// 后一种方法使用ES6的语法，用constructor构造器来构造默认的属性和状态
```
> propType 和 getDefaultProps
```
// React.createClass：通过proTypes对象和getDefaultProps()方法来设置和获取props.

const Mydemo = React.createClass({
    propTypes: {
        name: React.PropTypes.string //设置props中name的类型
    }
    getDefaultProps() {
        return {
            name: '陈焦滨&kevin' //设置props中name的默认值
        };
      },
    render () {
        return (
            <div>这个是demo</div>
        )
    }
})

// React.Component：通过设置两个属性propTypes和defaultProps

import PropTypes from 'prop-types'
export default class Mydemo2 extends React.Component {
    constructor (props) {
        super(props)
    }
    // 在propTypes在15.5.0版本以后就被移除了，被单独封装成一个包prop-type,用的时候需要安装依赖prop-types
    //static propTypes = {
    //   name: React.PropTypes.string
    //};
    //使用static的时候记得安装babel-preset-stage-0
    static defaultProps = {
        name: ''
    };
    render () {
        return ({
            <div>这个事React.Component的语法</div>
        })
    }
}
// 使用这个之前需要先安装依赖prop-types
Mydemo2.propTypes  = {
	userId: PropTypes.number.isRequired
}
```
> 状态state的区别
```
// React.createClass：通过getInitialState()方法返回一个包含初始值的对象
const Mydemo = React.createClass({
    // 初始化state数据,这个在es6中的class就失效了，直接可以通过在constructor设置state
    getInitialState(){ 
        return {
            isEditing: false
        }
    }
    render () {
        return (
            <div>这个是demo</div>
        )
    }
})

export default class Mydemo2 extends React.Component {
    constructor (props) {
        super(props)
        this.state = {
            name: '陈焦滨&kevin'
        }
    }
    render () {
        return ({
            <div>这个事React.Component的语法</div>
        })
    }
}
```
> this区别
```
// React.createClass：会正确绑定this
const Mydemo = React.createClass({
    handlerFun () {
        console.log('测试')
    }
    render () {
        return (
            {/*这是注释：这里的点击事件不用绑定this，createClas这种方式的this已经是指到当前实例*/}
            <div onClick="this.handlerFun">这个是demo</div>
        )
    }
})

// React.Component：由于使用了 ES6，这里会有些微不同，属性并不会自动绑定到 React 类的实例上。
export default class Mydemo2 extends React.Component {
    constructor (props) {
        super(props)
        this.state = {
            name: '我是state参数'
        }
        // 推荐这种绑定方式，因为在这里只需要绑定一次就好了，如果是在render里面绑定，每一次调用render(可以说是非常频繁！)一个新的函数都会被创建。与在构造函数里只绑定一次相比就慢一些
        // this.handlerFun = this.handlerFun.bind(this)
    }
    handlerFun (val) {
        console.log(val)
    }
    render () {
        return ({
            {/*这里通过bind绑定this*/}
            <div onClick="this.handlerFun.bind(this,'我是参数')">这个事React.Component的语法</div>
        })
    }
}

// 还可以通过箭头函数直接替换函数在类中的声明，这是es7的方式
export default class MyDemo2 extends React.Component {
    constructor (props) {
        super(props)
        this.state = {
            name: '我是初始值'
        }
    }
    const handerFun = (event) = {
        this.setState({
            name:'我被改变了'
        })
    }
}
```
> mixin的使用
```
// React.createClass：使用 React.createClass 的话，我们可以在创建组件时添加一个叫做 mixins 的属性，并将可供混合的类的集合以数组的形式赋给 mixins。
// 创建一个文件MxinCommon.js
let MyMixin = {
    doSomething(){
        console.log('我是mixin')
    }
}
export default MyMixin

// React.createClass
import React from 'react';
import MyMixin from './MxinCommon.js'
let TodoItem = React.createClass({
    mixins: [MyMixin]
    render(){
        return <div></div>
    }
})
// React.Component,es6没有mixin这种混合，只能通过第三方插件react-mixin来使用，安装完这个依赖后就可以使用啦
import ReactMixin from 'react-mixin'
import mixinLog from './MxinCommon.js'
export default class MyDemo2 React.Component {
    constructor (props) {
        super(props)
        this.handler = this.handler.bind(this)
    }
    handler () {
        mixinLog.doSomething() //这样就调用到mixin的方法了
    }
    render () {
        return (
            <div onClick="this.handler"></div>    
        )
    }
}
ReactMixin(MyDemo2.prototype, mixinLog)
```
### 结语
祝各位前端开发的小伙伴爬坑快乐 <a name=":blush:"/>
