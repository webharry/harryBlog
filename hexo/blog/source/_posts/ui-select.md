---
title: angular+ui-select+select2
date: 2017-12-07 11:23:56
tags:
    - frontend
    - plugin
    - angular
---
今天项目需求，增加一个下拉搜索框，找来了ui-selet插件,就插件的使用做一下总结分享。
### 1.引入
npm：
```shell
npm install ui-select
```
在angular项目的文件引入模块：
```js
impprt 'ui-select';

var module = angular.module('myapp', ['ui.select', 'ngSanitize']);
```
小坑：
* 这个ui-select版本还不完善，需要自己手动引入下css文件，在node_module中找到ui-slect的主文件index.js，将css文件导出一下，如下：
```js
require('./dist/select.js');
require('./dist/select.css');
module.exports = 'ui.select';
```
* 此外需要引入angular依赖文件：
```html

<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular-sanitize.js"></script>

```

### 2.基本使用
在html文件中写入：

```html

<ui-select ng-disabled="disabled" ng-model="arr.selected" style="min-width: 300px;">
    <ui-select-match placeholder="请选择">
        <span ng-bind="$select.selected.name"></span>
    </ui-select-match>
    <ui-select-choices repeat="item in arr | filter:   $select.search"><span ng-bind="item.name"></span>
    </ui-select-choices>
</ui-select>

```
js文件：
```js
'use strict';

var app = angular.module('demo', ['ngSanitize', 'ui.select']);
app.controller('DemoCtrl', function ($scope, $http, $timeout, $interval) {
    $scope.arr = [
        {
            name:'slecet1'
            id:1
        },
        {
            name:'slecet2'
            id:2
        }
    ]
}
```
### 3.增加event事件
在使用select2插件时，发觉事件ng-click和ng-change并不会触发执行。这个问题在ui-select插件的event事件中可以解决。on-selsect="expression"：
```html
<ui-select ng-disabled="disabled" ng-model="arr.selected" on-select="change($item, $model)" style="min-width: 300px;">
    <ui-select-match placeholder="请选择">
        <span ng-bind="$select.selected.name"></span>
    </ui-select-match>
    <ui-select-choices repeat="item in arr | filter:   $select.search"><span ng-bind="item.name"></span>
    </ui-select-choices>
</ui-select>
```
在js文件：
```js
//change()函数可以取到当前选中的选项信息$item, $model
$scope.change = function($item, $model) {
        //这里写入业务逻辑代码
        $scope.id = $item.id;
    }
```
### 4.设置ui-select默认值
```js
$scope.arr.selected = {//ui-select 赋初值
    id: '1',
    name: '111'
}
```
因为在给ui-select组件绑定的是对象，所以在设置默认值的时候也必须保持类型一致。
### 5.总结

小结：更多使用方法参看官网例子：https://github.com/angular-ui/ui-select
