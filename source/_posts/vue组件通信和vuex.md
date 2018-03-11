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
