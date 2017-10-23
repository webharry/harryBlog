---
title: 跨域解决方法-proxy
date: 2017-10-23 15:34:57
tags:
    - gulp
    - frontend
    - '构建生态'
---
今天尝试使用http-proxy-middleware插件解决本地开发中的跨域问题。这篇文章主要讲的是跨域解决方法6.proxy，使用代理。  

#### 跨域解决方法-proxy
##### http-proxy-middleware
下面在本地开发中复现AJAX跨域请求。
本地开发，需要请求远程服务器资源，使用ajax请求获取远程资源：
```js
$(document).ready(function() {
  $.ajax({
    type:"GET",
    url:"./api/.....",//远程地址
    dataType:"json",
    success:function(data) {
      console.log(data);
    }
  });
})
```
此时浏览器会报错，跨域问题。
意思是出现跨域请求错误。
#### 解决方法一：browser-sync+http-proxy-middleware做代理

在gulpfile.js文件中做配置：
```js

var browserSync = require('browser-sync').creat();
var proxyMiddleware = require('http-proxy-middleware');

var proxy = ('./api',{
  target:"http://echo.websocket.org",//远程地址url
  changeOrigin:true,//虚拟主机站点需要
  pathRewrite:{
    '^/api/(.*)':'/$1'  //正则匹配，替换api起始的路径
  }
});

gulp.task('server',function(){
  browserSync.init({
    server:'./dist',
    notify:false,
    middleware:[proxy]
  });
});

```
其他解决方法参看如下：  

#### 跨域解决方法
##### [1.CORS（cross-origin resourse sharing)跨域资源共享](https://webharry.github.io/2017/10/23/CORS/)
##### 2.JSONP
##### 3.document.domain + iframe
##### 4.window.name + iframe
##### 5.postMessage
##### [6.proxy](https://webharry.github.io/2017/10/23/proxy/)