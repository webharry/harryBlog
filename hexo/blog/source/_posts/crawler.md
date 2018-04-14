---
title: crawler
date: 2018-04-14 14:27:24
tags:
    - node.js
    - frontend
    - JavaScript
---
### node.js实现简单爬虫
工具：cheerio  

cheerio 是nodejs特别为服务端定制的，能够快速灵活的对JQuery核心进行实现。它工作于DOM模型上，且解析、操作、呈送都很高效。  
更多API参看：
>https://github.com/cheeriojs/cheerio

我们以慕课网页面为例，爬取每个视频课程的标题和课程对应id,期望结构如下：
```shell
titles = [{
  chapterTitle: chapterTitle,
  id: id
}]
```
第一步，我们用node写一个请求，获取想要爬虫的网站html,这里以慕课网为例
```js
var http = require('http')
var url = 'http://www.imooc.com/course/list?c=nodejs'

http.get(url, function(res){
  var html = ''
  
  res.on('data', function(data){
    html += data
  })
  
  res.on('end', function(){
    var result = filterHml(html)
    print(result)
  })
}).on('error', function(){
  console.log('获取数据错误！')
})
```
第二步，我们根据需求来编写过滤HTML的函数，将过滤后的数据打印在控制台。
```js
function filterChapters(html) {
    var $ = cheerio.load(html)
    var chapters = $('.course-card-container')//以类名获取节点元素

    var titles = []
    chapters.each(function (item) {
        var chapter = $(this)
        var chapterTitle = chapter.find('h3').text()
        var id = chapter.find('a').attr('href').split('learn/')[1]
        titles.push({
            chapterTitle: chapterTitle,
            id: id
        })
    })

    return titles
}

function printCourseInfo(courseData){
    courseData.forEach(item => {
        console.log('【' + item.id + '】' + item.chapterTitle + '\n')
    });
}

```
爬虫结果：
```shell
【935】Vue+Webpack打造todo应用

【882】基于websocket的火拼俄罗斯（单��版）

【861】基于Websocket的火拼俄罗斯（基础）

【866】前端性能优化-通用的缓存SDK

【773】AC2016腾讯前端技术大会

【728】创业公司的Nodejs工程师

【725】Roundtable前端分享专场

【637】进击Node.js基础（二）

【590】阿里D2前端技术论坛——2015融合

【564】去哪儿前端沙龙分享第三期

【556】慕课网技术沙龙之前端专场

【434】去哪儿前端沙龙分享第二期

【367】Qnext前端交互沙龙

【348】进击Node.js基础（一）

【221】D2前端技术论坛——2014绽放

【197】node建站攻略(二期)——网站升级

【75】node+mongodb 建站攻略（一期）
```

小结：node.js使得JavaScript代码能够运行在服务端，从而进行一些操作，node.js的更多用法参看后续文章.
