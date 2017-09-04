---
title: 作用域
date: 2017-09-04 10:48:00
tags: 
- this
- 作用域
- 闭包
categories:
- JS
- This+scrop+closure
---
日常开发中，我们经常用到this。例如用Jquery绑定事件时，this指向触发事件的DOM元素；编写Vue、React组件时，this指向组件本身。对于新手来说，常会用一种意会的感觉去判断this的指向。以至于当遇到复杂的函数调用时，就分不清this的真正指向。

本文将通过两道题去慢慢分析this的指向问题，并涉及到函数作用域与对象相关的点。最终给大家带来真正的理论分析，而不是简简单单的一句话概括。

相信若是对this稍有研究的人，都会搜到这句话：this总是指向调用该函数的对象。

然而箭头函数并不是如此，于是大家就会遇到如下各式说法：
* 箭头函数的this指向外层函数作用域中的this。

* 箭头函数的this是定义函数时所在上下文中的this。

* 箭头函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
各式各样的说法都有，乍看下感觉说的差不多。废话不多说，凭着你之前的理解，来先做一套题吧（非严格模式下）。
```
/**
 * Question 1
 */

var name = 'window'

var person1 = {
  name: 'person1',
  show1: function () {
    console.log(this.name)
  },
  show2: () => console.log(this.name),
  show3: function () {
    return function () {
      console.log(this.name)
    }
  },
  show4: function () {
    return () => console.log(this.name)
  }
}
var person2 = { name: 'person2' }

person1.show1()
person1.show1.call(person2)

person1.show2()
person1.show2.call(person2)

person1.show3()()
person1.show3().call(person2)
person1.show3.call(person2)()

person1.show4()()
person1.show4().call(person2)
person1.show4.call(person2)()
```
