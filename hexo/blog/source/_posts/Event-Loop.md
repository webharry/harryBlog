---
title: 了解JavaScript执行机制
date: 2018-05-03 09:44:03
tags:
    - JavaScript
    - frontend
---
## 小例子
```js

console.log('script start');
setTimeout(function() {
  console.log('setTimeout');
}, 0);
Promise.resolve().then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});

```
这段代码的输出是：
```js
script start
promise1
promise2
setTimeout
```
浏览器兼容性：但是不同的浏览器可能会出现不同的输出顺序。

　　Microsoft Edge, FireFox 40, iOS Safari 以及 Safari 8.0.8 将会在 'promise1' 和 'promise2' 之前输出 'setTimeout'。但是奇怪的是，FireFox 39 和 Safari 8.0.7 却又是按照正确的顺序输出。
## 原理深入

### 1.同步异步
* javascript是单线程的，js执行依照代码书写顺序向下执行。
正因为js是单线程的，js任务只能一个一个顺序执行，前一个任务未完成，后面的任务只能等待。但是，实际情况是，在渲染页面过程中，常常需要请求像图片视频这样的服务器的资源，这样的js任务很耗时且返回时间未知，会导致页面非常卡，那后面js任务就只能一直等这些资源加载回来，这会导致我们的网页体验非常差，Javascript语言将任务的执行模式分成两种：同步（Synchronous）和异步（Asynchronous）。
* 同步任务
* 异步任务 
网页的渲染过程就是一堆同步任务，比如页面的骨架和页面元素的渲染。而像加载图片音乐之类占用资源大耗时久的任务，就是异步任务。 

Js怎么处理同步异步任务呢？？？
这就要涉及到一个概念，叫Event Loop（事件循环）。
### 2.Javascript事件循环Event Loop

![js任务执行](https://user-gold-cdn.xitu.io/2017/11/21/15fdd88994142347?imageslim)

图中要表达的含义是：
* 同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。
* 当指定的事情完成时，Event Table会将这个函数移入Event Queue。
* 主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。
* 上述过程会不断重复，也就是常说的Event Loop(事件循环)。  

js引擎存在monitoring process进程，会持续不断的检查主线程执行栈是否为空，一旦为空，就会去Event Queue那里检查是否有等待被调用的函数。

看个小例子：

演示：[loup](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)
```js
console.log('script start');
setTimeout(function() {
  console.log('setTimeout');
},5000);
console.log('script end');
```
js是单线程的，执行栈先执行同步任务，同步任务执行完毕，才执行异步任务：
![1](http://chuantu.biz/t6/299/1525081743x1822611341.jpg)
![2](http://chuantu.biz/t6/300/1525230876x-1404817515.jpg)
![3](http://chuantu.biz/t6/300/1525230913x-1404817515.jpg)
小结：事件循环是js实现异步的一种方法，也是js的执行机制。

### 3.宏任务和微任务
如果将之前的代码改下：
```js
console.log(1)  // snippet1
Promise.resolve().then(function() {  // snippet2
    console.log(2);
})
setTimeout(function() { // snippet3
    console.log(3);
    setTimeout(function() { // snippet4
        console.log(4)
    }, 0)
}, 0)
console.log(5) // snippet5
```
这段代码的输出顺序是1, 5, 2, 3, 4。
这是因为 promise 的 then 方法，被认为是在微任务队列当中。JavaScript宏观的将任务分为两种：

* macro-task（宏任务）：包括整体代码script，setTimeout，setInterval
* micro-task（微任务）：Promise，process.nextTick， MutationObserver

Microtask微任务 通常来说就是需要在当前 task 执行结束后立即执行的任务，例如需要对一系列的任务做出回应，或者是需要异步的执行任务而又不需要分配一个新的 task，这样便可以减小一点性能的开销。microtask 任务队列是一个与 task 任务队列相互独立的队列，microtask 任务将会在每一个 task 任务执行结束之后执行。每一个 task 中产生的 microtask 都将会添加到 microtask 队列中，microtask 中产生的 microtask 将会添加至当前队列的尾部，并且 microtask 会按序的处理完队列中的所有任务。

接下来的主要介绍这两个任务的概念和线程表现:
1.这两种类型的任务会进入与之对应的Event Queue
2.事件循环的顺序，决定JS代码的执行顺序
3.先是进入整体代码的宏任务，开始事件循环，然后紧接着执行当前宏任务的微任务
4.执行完当前宏任务的微任务后 进入Event Queue里面的下一个宏任务
![事件循环](http://chuantu.biz/t6/297/1524834771x-1404775551.jpg)

这段代码的执行过程是：
1. snippet1 push 到执行栈，执行完并清空执行栈
2. snippet2 的回调 push 到 microtask 队列中
3. snippet3 交给 Web Apis，0ms 后将回调 push 到 marcotask 队列
4. snippet5 的回调 push 到执行栈，执行完并清空执行栈
5. script task 执行完后，将 snippet2 中的回调从 microtask 队列取出，push 到执行栈，执行完并清空执行栈
6. snippet3 的回调 push 到执行栈，执行完并清空执行栈，同时将 snippet4 交给 Web Apis，0ms 后将回调 push 到任务队列
7. snippet4 的回调 push 到执行栈，执行完并清空执行栈

* 微任务执行场景
microtask 通常来说就是需要在当前 task 执行结束后立即执行的任务，例如需要对一系列的任务做出回应，或者是需要异步的执行任务而又不需要分配一个新的 task，这样便可以减小一点性能的开销。microtask 任务队列是一个与 task 任务队列相互独立的队列，microtask 任务将会在每一个 task 任务执行结束之后执行。每一个 task 中产生的 microtask 都将会添加到 microtask 队列中，microtask 中产生的 microtask 将会添加至当前队列的尾部，并且 microtask 会按序的处理完队列中的所有任务。microtask 类型的任务目前包括了 MutationObserver 以及 Promise 的回调函数和 node 中的 process.nextTick。
* 浏览器支持

#### 进阶 microtask
我们来看一段代码：
```html
<div class="outer">
    <div class="inner"></div>
</div>
```
```js
var outer = document.querySelector('.outer');
var inner = document.querySelector('.inner');

// 给 outer 添加一个观察者
new MutationObserver(function() {
  console.log('mutate');
}).observe(outer, {
  attributes: true
});

// click 回调函数
function callback() {
  console.log('click');

  setTimeout(function() {
    console.log('timeout');
  }, 0);

  Promise.resolve().then(function() {
    console.log('promise');
  });

  outer.setAttribute('data-random', Math.random());
}

inner.addEventListener('click', callback);
outer.addEventListener('click', callback);

// inner.click();
```
```css
.outer {
	height: 100px;
	width: 400px;
	background: #ccc;
}
.inner {
	height: 50px;
	width: 200px;
	background: #ddd;
}
```
我们看下不同浏览器输出：
![浏览器运行对比图](http://chuantu.biz/t6/301/1525268844x-1404817635.jpg)  
按照上面的推导 Chrome 的输出是正确的。
>通过上面的例子可以测试出，FireFox 和 Safari 能够正确的执行 microtask 队列，这一点可以通过 MutationObserver 的表现中看出，不过 Promise 被添加至事件队列中的方式好像有些不同。 这一点也是能够理解的，由于 jobs 和 microtasks 的关系以及概念目前还比较模糊，不过人们都普遍的期望他们都能够在两个事件监听器之间执行。这里有 FireFox 和 Safari 的 BUG 记录。（目前 Safari 已经修复了这一 BUG）
在 Edge 中我们可以明显的看出其压入 Promise 的方式是错误的，同时其执行 microtask 队列的方式也不正确，它没有在两个事件监听器之间执行，反而是在所有的事件监听器之后执行，所以才会只输出了一次 mutate 。Edge bug ticket （目前已修复）  

接下来，将上面代码最后一行注释去掉，再执行，我们看到输出顺序是这样的：
![浏览器执行对比图2](http://chuantu.biz/t6/301/1525272394x-1404775431.jpg)

>同理在之前的例子中由于我们调用 click()，使得事件监听器的回调函数和当前运行的脚本同步执行，所以当前脚本的执行栈会一直压在 JS 执行栈当中（简单来说就是click的回调并没有加入任务队列中，而是直接执行了）。所以在这个 demo 中 microtask 不会在每一个 click 事件之后执行，而是在两个 click 事件执行完成之后执行。所以在这里我们可以再次的对 microtask 的检查点进行定义：当执行栈(JS Stack)为空时，执行一次 microtask 检查点。这也确保了无论是一个 task 还是一个 microtask 在执行完毕之后都会生成一个 microtask 检查点，也保证了 microtask 队列能够一次性执行完毕。  
  
mutate只输出一次的原因是，MutationObserver微任务只在微任务队列注册一次，因为第二次执行回调函数callback时，发现微任务队列已经注册了dom变化的监听事件，所以不再注册到微任务队列。


### 原理验证--借用插件演示
推荐一个JS执行的可视化工具[loupe](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)  [lu:p]（备注：暂时不支持微任务演示）
### 拓展-setTimeout
1.setTimeout
setTimeout的作用就是让异步任务延迟执行
先上一段代码：
```js
setTimeout(() => {
    task();
},3000)
console.log('执行console');
function task(){
	console.log('task')
}
```
来推断一下执行结果：setTimeout是异步的，应该先执行console.log这个同步任务，所以我们的结论是：
```js
//执行console
//task()
```
我们到验证器里运行一下结果，结果是正确的。
在看一段有意思的代码：
```js
//代码1
console.log('先执行这里');
setTimeout(() => {
    console.log('执行啦')
},0);
```
我们来分析下这段代码，延迟0秒是立即执行的意思吗？显然不是，我们按照js执行机制分析下代码怎么执行的。
* 首先，console.log进入执行栈执行
* 接下来解析到setTimeout，发现是异步任务，扔到解析API进行处理（比如浏览器webAPI）,解析API延时0秒，把setTimeout的回调函数注册到Event Queue，等到js执行栈为空了，就把Event Queue的任务推入执行栈执行，也就是执行回调函数。  

## 检验--两段有意思的代码
如果你能正确知道这段代码的输出，就说明你真正的理解同步异步了：
```js
var start;
start = +new Date(); 
setTimeout(function(){
  console.log('setTimeout',+new Date() - start);// part1
},200);
while(start + 2000 > +new Date()){
  
};
```
part1打印的时间是多少？？？  

输出是：
```js
setTimeout 2003
```
很明显，part1打印出的时间大于2000毫秒，但不会大于2200毫秒。我们把代码拷贝到控制台运行一下，发现结果是小于2200毫秒的。

### 关于同步异步另一段有意思的代码
先看一段代码：
```html
<button id="btn">click me</button>
```
```js
let btn = document.getElementById('btn');
console.log('script start');
btn.onclick = function() {
    console.log('click')
}
console.log('script end');
```
如果我们把js代码改一下：
```js
let btn = document.getElementById('btn');
console.log('script start');
btn.onclick = function() {
    console.log('click')
}
btn.click();
console.log('script end');
```
这个执行顺序是：
script start
click
script end

原因是我们调用 .click()，使得事件监听器的回调函数和当前运行的脚本同步执行而不再是异步,简单来说就是click的回调并没有加入任务队列中，而是直接执行了。
>参考链接：
https://www.cnblogs.com/dong-xu/p/7000139.html
http://blog.xieluping.cn/2018/03/08/event-loop/
