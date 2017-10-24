---
title: 跨域解决方法-JSONP
date: 2017-10-24 20:47:22
tags:
    - frontend
    - JavaScript
    - '构建生态'
---

  之前的文章谈到由于浏览器的同源策略，当请求不同源的资源时就会遇到跨域问题，今天尝试用jsonp来解决跨域问题。
#### JSONP是什么  

JSONP(JSON with Padding)是json的一种“使用模式”,可以让网页取得不同源上的资源数据，它不需要使用XMLHttpRequest对象，而是使用script标签来请求不同源的数据资源。  
使用JSONP的关键是使用回调函数进行服务器和客户端的数据交互。来看下面的实例：
#### 解决实例  
在客户端，即html文件：
```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JSONP-demo</title>
</head>
<body>
<p>test</p>
<script>
  function test(data){
    let p = document.getElementsByTagName('p')[0];
    p.innnerHTML = data;
  }
</script>
<script src="http://localhost:3002?callback=test"></script>
<!--src里的问号？后的参数（callback=test)可以在3002端口页面中可以通过req.query.callback获取-->
</body>
</html>

```
在客户端起一个服务在3000端口（这里用express）：
```js

var express = require('express'); // 引用express模块
var app = express();  // 创建一个简单的服务器

var requestPort = 3000;

app.use(express.static(__dirname));

```
在3002端口页面，即服务器：
```js
var express = require('express');
var app = express();

var responsePort = 3002;

app.get('/',function(req,res){
  var callbackName = req.query.callback;
  res.send(callbackName + "('hello jsonp!')");
})

```
#### 跨域解决方法
##### [1.CORS（cross-origin resourse sharing)跨域资源共享](https://webharry.github.io/2017/10/23/CORS/)
##### [2.JSONP](https://webharry.github.io/2017/10/24/JSONP/)
##### 3.document.domain + iframe
##### 4.window.name + iframe
##### 5.postMessage
##### [6.proxy](https://webharry.github.io/2017/10/23/proxy/)