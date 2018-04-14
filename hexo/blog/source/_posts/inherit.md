---
title: 浅谈ES5，ES6继承
date: 2017-12-31 19:17:03
tags:
    - frontend
    - JavaScript
---
今天来看下JavaScript中是如何实现继承的？es5和es6在继承实现上有什么区别？带着这两个问题，进入下文。  

我们都知道，在JavaScript中，继承是通过原型链来实现的。原型链的知识请自行参考文章"从instanceof浅谈原型链"。  

### es5  
在es5中继承是这样定义的：子类的原型是父类的一个实例，以此子类可以继承父类的原型上的属性和方法，即继承是通过原型链实现。看图：
![es5](https://upload.cc/i/oiYBmQ.png)
看代码：
```js
function Super() {
    this.property = true;
};

Super.prototype.getSuperValue = function() {
    return this.property;
};

function Sub() {
    this.subproperty = false;
};

//继承了Super
Sub.prototype = new Super();

Sub.prototype.getSubValue = function() {
    return this.subproperty;
};

var instance = new Sub();
console.log(instance.getSuperValue());//true
```
代码中，定义了两个构造函数Super和Sub,以及他们各自的属性和方法。我们注意到一句代码，Sub.prototype = new Super();Sub的原型实际上是Super类型的一个实例，这样Sub就能够使用Super的方法和属性，即实现了继承。    

它们的关系可以进一步检验：  

```js
Sub.prototype.constructor == Sub;//true
Sub.prototype.__proto__ == Super.prototype;//true
instance.__proto__ == Sub.prototype;//true
instance.constructor == Sub;//true
```  

es5中，继承存在的一个问题是，每次子构造函数从父构造函数继承，需要调用多次父构造函数：继承时，父构造函数需要new一个实例，调用一次；当子构造函数调用父的属性方法会调用多次。  

### es6
在es6中,继承实现和es5是一样的，都是通过原型链实现的。es6中Class 可以通过extends关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。
```js
class Point() {

};

class ColorPoint extends Point() {
    constructor(x,y,color) {
        super(x,y);//调用父类的constructor(x,y)
        this.color = color;
    }

    toString() {
        return this.color + '' + super.toString();//调用父类的toString()
    }
};

var instance = new ColorPoint(25，8，'red');
cp instanceof ColorPoint//ture
cp instanceof Point // true
```
上面代码中，constructor方法和toString方法之中，都出现了super关键字，它在这里表示父类的构造函数，用来新建父类的this对象。

子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

如果子类没有定义constructor方法，这个方法会被默认添加，也就是说，不管有没有显式定义，任何一个子类都有constructor方法。

看图：
![es6](https://upload.cc/i/qYNUJS.png)
从图中，很清晰的看到，相比于es5的继承，子类的__proto__属性指向其父类。

* 小结：  

es5的继承，实质是先创造子类的实例对象this,然后再将父类的方法添加到this上面。ES6 的继承机制完全不同，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。