---
title: CORS（cross-origin resourse sharing)跨域资源共享
date: 2017-10-12 10:30:14
tags:
    - js
    - frontend
    - '构建生态'
---
今天从同源策略出发，探究跨域问题出现的原因及其原理，以及一些常见解决方法。这篇文章主要讲跨域解决办法1.CORS（cross-origin resourse sharing)跨域资源共享。

### 同源策略
所谓同源，就是比较两个页面的协议、域名、端口是否相同，相同则是同源，有一个不同就视为不同源。
>根据MDN给出的例子：
>下表给出了相对http://store.company.com/dir/page.html同源检测的示例:

<table class="standard-table">
 <tbody>
  <tr>
   <th>URL</th>
   <th>结果</th>
   <th>原因</th>
  </tr>
  <tr>
   <td><code><span class="nowiki">http://store.company.com/dir2/other.html</span></code></td>
   <td>成功</td>
   <td><font face="consolas, Liberation Mono, courier, monospace"> </font></td>
  </tr>
  <tr>
   <td><code><span class="nowiki">http://store.company.com/dir/inner/another.html</span></code></td>
   <td>成功</td>
   <td><font face="consolas, Liberation Mono, courier, monospace"> </font></td>
  </tr>
  <tr>
   <td><code><span class="nowiki">https://store.company.com/secure.html</span></code></td>
   <td>失败</td>
   <td>不同协议 ( https和http )</td>
  </tr>
  <tr>
   <td><code><span class="nowiki">http://store.company.com:81/dir/etc.html</span></code></td>
   <td>失败</td>
   <td>不同端口 ( 81和80)</td>
  </tr>
  <tr>
   <td><code><span class="nowiki">http://news.company.com/dir/other.html</span></code></td>
   <td>失败</td>
   <td>不同域名 ( news和store )</td>
  </tr>
 </tbody>
</table>

### 跨域问题
浏览器为了保证信息安全，规定了资源访问使用同源策略。所以，当浏览器访问的资源来自不同源，就会出现跨源资源访问，即跨域问题。 

#### 解决方法
##### 1.CORS（cross-origin resourse sharing)跨域资源共享
CORS是浏览器解决跨域问题的一种策略，在CORS中，浏览器把ajax发起的请求分为简单请求和非简单请求，分别对两种请求进行处理，再将ajax请求发往服务器。
##### ①简单请求就是满足以下条件的：

请求方式为这几种：
```shell
GET,POST,HEAD
```

HTTP头信息不超出以下几种：

```shell
Accept
Accept-Language
Content-Language
Last-Event-ID
content-type只限于三种：text/plain，multipart/form-data，application/x-www-form-urlencoded

```
对于简单请求，浏览器直接发出CORS请求，在头信息中增加一个字段：Origin,表示请求源域名。例子：
```js

Accept:*/*
Accept-Encoding:gzip, deflate, br
Accept-Language:zh-CN,zh;q=0.8
Connection:keep-alive
Host:localhost:3001
Origin:http://localhost:3000

```
Origin:http://localhost:3000字段告诉服务器请求源是http://localhost:3000，服务器就可以做出相应响应。 

如果服务器发现Oringin字段的源不在接受范围，就会发送一个正常HTTP回应，浏览器发现这个回应的头信息中没有包含Accsess-Control-Allow-Origin字段，就抛出一个错误，被XMLHttpRequest的onerror捕获，即跨域问题出现了。

好了，知道报错原因，我们就可以从以下方法解决跨域问题了：  
概括为一句话：在服务器设置接受的源信息。
###### 实例
客户端请求：
```js
var xhr = new XMLHttpRequest();
var url = 'http://localhost:3000';

xhr.open('GET',url);
xhr.send(null);
xhr.onreadystatechange = () => {
  if (xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) { // 如果请求成功
      text.innerHTML = xhr.response;
  }
}

```
服务器设置（例子用的express）：
```js
var express = require('express');
var test = express();

test.get('/',(req,res) => {
  res.set('Access-Control-Allow-Origin','http://localhost:3000');
  res.header('Access-Control-Allow-Methods', 'GET, POST, PUT');
  res.header('Access-Control-Allow-Headers', 'X-Custom-Header');
  res.header('Access-Control-Allow-Credentials', 'true');
  res.send("hello CORS.");
})
```
如上，服务器对请求的头信息进行设置，  
Access-Control-Allow-Origin字段表示接受的源，表示接受来自http://localhost:3000这个域名的请求。设置为*表示接受任意域名的请求。

Access-Control-Allow-Methods列举出服务器接受的请求方式。  
Access-Control-Allow-Headers可以设置允许请求源的header特殊参数。  
Access-Control-Allow-Credentials该字段可选。它的值是一个布尔值，表示是否允许发送Cookie。  

如此，就可以实现跨域访问了。
##### ②非简单请求
非简单请求，我的理解是排除了以上简单请求的，比如PUT请求。  

浏览器对于非简单请求，在发送正式请求前，会先发送一个OPTION请求，向服务器询问是否接受请求，称为“预检”请求。  
如果服务器接受才会发送正式请求，否则就报错。  
与简单请求一样，服务器无非也就是判断请求的域名、头信息和HTTP动词等是否在允许范围。  
实例：  
客户端发送PUT请求：
```js
    var xhr = new XMLHttpRequest();
    var url = 'http://localhost:3000';
    xhr.open('PUT',url);
    xhr.setRequestHeader('X-Custom-Header', 'value');
    xhr.send();
    xhr.onreadystatechange = () => {
      if(xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
        text1.innerHTML = xhr.response;
      }
    }
```
非简单请求例子中，我设置了特殊头信息“X-Custom-Header”，告知服务器接受这个参数信息。  

在服务器设置接受信息：  
```js
var express = require('express');
var test = express();

var responsePort = 3001;//服务器端口
test.all('*', (req, res) => {
  res.set('Access-Control-Allow-Origin', 'http://localhost:3000');
  res.header('Access-Control-Allow-Methods', 'GET, POST, PUT');
  res.header('Access-Control-Allow-Headers', 'X-Custom-Header');
  res.header('Access-Control-Allow-Credentials', 'true');
  res.send("hello CORS.");
})
```
至此，对于两种请求方式，都可以实现跨域访问了。
其他解决方法参看如下：  

#### 跨域解决方法
##### [1.CORS（cross-origin resourse sharing)跨域资源共享](https://webharry.github.io/2017/10/23/CORS/)
##### 2.JSONP
##### 3.document.domain + iframe
##### 4.window.name + iframe
##### 5.postMessage
##### [6.proxy](https://webharry.github.io/2017/10/23/proxy/)