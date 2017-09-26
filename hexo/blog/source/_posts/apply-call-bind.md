---
title: Javascript中的apply、call和bind
date: 2017-09-12 20:53:40
tags:
    - JavaScript
    - frontend
---
这三种方法的作用是一样的，都能改变函数的执行作用域，区别只是在调用时传入的参数不同。
### apply方法
apply()方法接收两个参数，第一个参数是函数内this的作用域，第二个参数是一个数组对象或者arguments。
```js
var oo = {
    a:5,
    b:3
}
var a = 9,b=1;
function foo(a,b) {
    return this.a-this.b;
}
console.log(foo.apply(oo));//2
console.log(foo.apply());//8

```
### call方法
### bind方法
