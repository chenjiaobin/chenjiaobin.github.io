---
title: vuex和vue通信
date: 2018-03-11 16:19:00
tags: 
- vuex
- $emit
- props
- $on
categories:
- 前端
- 框架
---
vue的核心内容是组件库和数据绑定，在组件的状态管理上vue通过vuex进行完美的融合，以及在vue通信上以单向数据流来进行传递数据<!--more-->
### vue通信
vue2.0以上取消了之前的dispatch和broadcast的数据通信方式，也取消了子组件直接修改父组件数据的方式。现在组件之间的通信主要是通过$emit和$on，以及props的方式进行通信，当项目的复杂程度比较大的时候可以通过vuex状态管理插件来对组件进行状态的管理。
1. 父组件传递数据到子组件
```
//父组件
<template>
  <div>
    <child :tochild="word"></child>
  </div>
</template>
<script>
  export default {
    data(){
      return {
        word:'我是你父亲'
      }
    }
  }
</script>
//子组件
<template>
  <div>
    <p>{{tochild}}</p>
  </div>
</template>
<script>
  export default {
   props:['tochild'] //通过props接受父亲的东西
  }
</script>
```
2. 子组件传递数据给父组件
```
<template>
  <div>
    //@changeParent这个名字必须跟在子组件定义时的Key值要一样
    <child @changeParent="some"></child>
    <p>{{word}}</p>
  </div>
</template>
<script>
  export default {
    data(){
      return {
        word:''
      }
    },
    methods:{
      some(val){//这个val就是子组件传来的数据
        this.word=val;
      }
    }
  }
</script>
//子组件
<template>
  <div>
    <button @click="change">我要改父亲的东西</button>
  </div>
</template>
<script>
  export default {
   methods:{
   data(){
    return {
      somethig:"我是要给父亲的"
    }
   },
   change(){
      this.$emit("changeParent",this.something);
     }
   }
  }
</script>
```
3. 子组件修改父组件的数据还可以通过对象的方式
```
//做法：父组件传递给子组件的数据是一个对象，因此其实传递到子组件的是一个引用，只要在子组件修改这个父组件传递过来的值就会同时修改父组件和子组件的这个值
<template>
  <div>
    <child :tochild="word"></child>
    <p>{{word.a}}</p>
  </div>
</template>
<script>
  export default {
    data(){
      return {
        word:{a:"陈焦滨"}
      }
    }
  }
</script>
//子组件
<template>
  <div>
    <p>{{tochild.a}}</p>
    <button @click="changeMe">改变父组件传递过来的数据</button>
  </div>
</template>
<script>
  export default {
   props:['tochild'], //通过props接受父亲的东西
   methods:{
    changeMe(){
      this.word.a="好帅啊";//这样就实现了子组件和父组件同步的问题
    }
   }
  }
</script>
```
4. 兄弟组件传递数据
```
//兄弟组件之间的传递我们可能第一时间会想到从子组件传递数据到父组件，再从父组件传递数据到另外一个子组件，但是当我们的项目越来越大的时候，就会发现这种方式会引发很多问题，会变得不好维护
//所以我们最好使用vuex状态管理或者以下方法，通过实例化一个vue对象，然后通过$emit发射,$on接收，如下
a. 创建一个js文件，内容如下bus.js
import vue from 'vue'
export default new vue();
b. 子组件1
<template>
  <div>
    <button @click="changeMe">传数据给我兄弟</button>
  </div>
</template>
<script>
import bus from '../bus.js'
  export default {
   data(){
    return {
     word:'这是给我兄弟的'
    }
   },
   methods:{
    changeMe(){
      bus.$emit('msg',this.word);
    }
   }
  }
</script>
c. 组件2
<template>
  <div>
    <p>{{some}}</p>
  </div>
</template>
<script>
import bus from '../bus.js'
  export default {
   data(){
    return {
     some:''
    }
   },
   mounted(){
    var that=this;//切记，这个要加进去，要不在下面是取不到你这个实例里面的some的值的
    bus.$on("msg",function(val){  //通过$on接收兄弟传过来的值
     that.some=val;
    })
   }
  }
</script>
d. 父组件
<template>
 <div>
  <child-one></child-one>
  <child-two></child-two>
 </div>
</template>
<script>
import childOne from '../childOne'
import childTwo from '../childTwo'
export default {
 components:{
  'child-one':childOne,
  'child-two':childTwo
 }
}
</script>
```
### vuex状态管理
这个对于大型项目的组件的状态管理是非常方便的
```
//创建一个store文件夹，然后分别在这个文件夹下面创建mutation.js,action.js,store.js,getter.js,type.js(这个是用来存变量的)
//type.js
export  const  CHANGEDATA='CHANGEDATA';
export const  INCREMENT="INCREMENT"
// action.js
import * as  type from './type'
export default {
	changeMydata:({commit})=>{
		commit(type.CHANGEDATA);
	}
}
//mutation.js
import getters from "./getter"
import {CHANGEDATA,INCREMENT} from './type'
const state={
	count:0
}
const mutations={
	[CHANGEDATA](state){
		state.count++;
	}
// 相當於
// CHANGEDATA:(state)=>{
// 	state.count++
// }
}
export default {
//注意，下面这个名字必须这样写，不要写错了
	state,
	mutations,
	getters
}
//getter.js
export default {
	getCount:(state)=>{
		return  state.count;
	}
}
//store.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

import actions from './actions'
import mutation from './mutation'

export default new Vuex.Store({
	modules:{
		mutation
	},
	actions
})
//最后在main.js入口文件引入我们的store.js就ok了
new Vue({
  el: '#app',
  store:stores, //注意,这里的store也不要写错了
  router,
  components: { App },
  template: '<App/>'
})
```
以上是个人的一些常用方式，有什么纰漏希望指点

