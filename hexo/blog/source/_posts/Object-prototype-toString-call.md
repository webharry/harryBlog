---
title: JS中判断数据类型的几种方法
date: 2017-09-01 11:31:38
tags:
    - JavaScript
---
在搬砖途中看到一大神的代码，用Object.prototype.toString.call()判断数据类型，在这里学习总结下，在js中，判断数据类型除了用typeof运算符之外，还可以用Object.prototype.toString.call()方法、instanceof操作符。  
### 几种判断方法的区别
#### typeof判断数据类型  
使用typeof操作符，判断数据类型返回值如下：
```js
typeof 1//"number"
typeof "a"//"string"
typeof true//"boolean"
typeof {}//"object"
typeof []//"object"
typeof function(){}//"function"
```
使用时的一个问题是，在判断数组和对象存储值时，都返回"object"。

#### Object.prototype上的原生toString()方法判断数据类型  
Object.prototype.toString.call()方法既可以判断基本类型，也可以判断原生引用类型，还可以判断原生JSON对象。  

##### 判断基本类型
数字number、字符串string、布尔值boolean。
```js
Object.prototype.toString.call(2);//"[object Number]"
Object.prototype.toString.call("s");//"[object String]"
Object.prototype.toString.call(true);//"[object Boolean]"
```

##### 判断空类型
null和undefined。
```js
Object.prototype.toString.call(null);//"[object Null]"
Object.prototype.toString.call(undefined);//"[object Undefined]"
```

##### 判断复合类型
对象Object、数组Array、方法Function、日期Date、布尔Boolean、数字Number、字符串String、正则RegExp、Math。  
例如上面例子中的引用类型存储值就可以使用Object.prototype.toString()方法区别出来：
```js
Object.prototype.toString.call({});//"[object Object]"
Object.prototype.toString.call([]);//"[object Array]"
Object.prototype.toString.call(function(){});//"[object Function]"
Object.prototype.toString.call(String);//"[object Function]"
var t = new Date();
Object.prototype.toString.call(t);//"[object Date]"
var bb = new Boolean();
Object.prototype.toString.call(bb);//"[object Boolean]"
var bb = new Number();
Object.prototype.toString.call(bb);//"[object Number]"
var str = new String();
Object.prototype.toString.call(str);//"[object String]"
var reg = new RegExp("^1[34578][0-9]{9}$","g");
Object.prototype.toString.call(reg);//"[object RegExp]"
var reg = Math.valueOf();
Object.prototype.toString.call(reg);//"[object Math]"
```
##### 判断原生JSON对象
```js
var isNativeJSON = window.JSON && Object.prototype.toString.call(JSON);
console.log(isNativeJSON);//[object JSON]
```
输出为
##### 判断自定义类型
```js
function Foo(name,age) {
    this.name = name;
    this.age = age;
}
var foo =new Foo("harry",20);
Object.prototype.toString.call(foo);//"[object Object]"
```
遇到的一个问题是，使用Object.prototype.toString.call()方法不能判断foo是Foo类的实例。只能用instanceof来判断。
#### instanceof判断数据类型  
instanceof作用是判断原型与实例之间的关系。用法是：
```js
实例 instanceof 原型
```
>instanceof操作符会在实例的原型链上查找，直到找到右边构造函数的prototype属性，或者为null的时候停止。
> 引用自
> http://blog.xieluping.cn/2017/08/18/instanceof/

```js
console.log(foo instanceof Foo);//true
```
