---
title: angular+select2
date: 2017-09-27 15:50:45
tags:
    - plugin
    - jquery
    - frontend
---

select2 是一个jQuery的插件，扩展了select的功能。可以搜索，下面是使用方法:
>官网地址：https://select2.org/
### 一、引入插件select2

#### 1.访问GitHub下载：
>https://github.com/select2/select2  
解压后将js和css文件引入你的html文件：

```js

//select2
require('../select2/select2.min.js');
require('../select2/select2.min.css');

```

#### 2.或者通过远程动访问引入文件，在你的html文件引入js和css文件：

```html

<link href="https://cdnjs.cloudflare.com/ajax/libs/select2/4.0.4/css/select2.min.css" rel="stylesheet" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/select2/4.0.4/js/select2.min.js"></script>

```
### 二、使用
例子1：给select添加类名:

```html
<select id="tags" class="js-example-basic-single" ng-model="someValue">
  <option value="">请选择</option>
  <option ng-repeat="s in arr" value="{{s.id}}" on-finish-render-filters>{{s.value}}</option>
</select>
```
在js文件中调用select2的API：
```js

//select2插件实现下拉框的模糊搜索匹配功能
$(document).ready(function() {
    $('.js-example-basic-multiple').select2();
});

```

### 三、select2初始化

通过在.select2()方法中添加一个对象实现：

```js

$('.js-example-basic-multiple').select2({
  width:200px,//设置默认宽度
  //width: 'resolve', // 需要在html用行内样式重新定义宽度
  multiple: 'multiple',     // 多选


});

```
### 四、select2事件

```js

$('#tags').on('select2:select', function (e) {
      //获取select当前选中值得一些信息
      var data = e.params.data;
      console.log(data);
      $scope.someValue = data.id;
      
});

```
### 五、Data sources

```html

<select class="js-data-example-ajax"></select>

```

```js

$('.js-example-data-ajax').select2({
  ajax: {
    url: 'https://api.github.com/search/repositories',
    //dataType: 'json'
    // Additional AJAX parameters go here; see the end of this chapter for the full code of this example
    data: function (params) {
      var query = {
        search: params.term,
        type: 'public'
      }

      // Query parameters will be ?search=[term]&type=public
      return query;
    }
  }
});

```

### 六、select2提供的event
摘自网友博客：
>$(“#txt_tag”)  
>  .on(“change”, function(e) {​})​ // 当 select2 值被改变的時候 ( 改变后触发 )​  
> .on(“select2-opening”,function(){}) // ​当下拉出现时 ( 开启选项下拉前触发)  
>   .on(“select2-open”, function() {}) //当下拉出现时(开启选项下拉后触发)​  
> .on(“select2-close”,function(){}) // 当选项关闭的时候  
> .on(“select2-highlight”,function(e){})// hover 到下拉时  
> .on(“select2-selecting”,function(e){})// 选中选项的时候 (选中前触发 ) ​​  
> .on(“select2-removing”,function(e){}) // 移除选项的时候 ( 选中前触发) ​​  
> .on(“select2-removed”,function(e){}) //移除选项的时候 ( 选中后触发 ) ​​​  
> .on(“select2-loaded”,function(e){}) // load资料的时候 ( Loaded 完成后触发)  
> .on(“select2-focus”,function(e){}) // input 获得焦点的触发  
> .on(“select2-blur”,function(e) {}) // input 失去焦点触发  


### 七、踩过的坑

#### 1.在angular项目里使用时，select中的ng-model未绑定选中值，看起来似乎失效了。

```html
<select id="tags" class="js-example-basic-single" ng-model="someValue">
  <option value="">请选择</option>
  <option ng-repeat="s in arr" value="{{s.id}}" on-finish-render-filters>{{s.value}}</option>
</select>
```
解决办法：在调用插件时，添加一个事件，将选中值赋值给变量：
```js

//获取select选中值，赋值给全局变量$scope.someValue
  $('#tags').on('select2:select', function (e) {
      //获取select当前选中值得一些信息
      var data = e.params.data;
      console.log(data);
      $scope.someValue = data.id;
      
  });

```

总结：select2提供了很多配置参数，处理事件，可以更具业务需求进行设置。本文主要用到了select2的搜索功能，以及处理与angular同时使用时的问题。
