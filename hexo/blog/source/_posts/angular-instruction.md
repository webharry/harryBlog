---
title: angular-instruction
date: 2017-03-05 07:34:13
tags:
---
# Angular几个常用指令

###### 1.ng-if指令
**定义：**指令用于表达式值为false时移除 HTML 元素；如果if语句的执行结果为true，会添加移除元素，并显示。ng-if 指令不同于 ng-hide， ng-hide 隐藏元素，而 ng-if 是从 DOM 中移除元素。

**用法实例：**通过ng-if控制元素隐藏/显示，达到点击隐藏和显示的效果也是常见的用法。推荐这种方法，简单快捷有木有。



	<div ng-click="a!=a" ng-if="aa">
    	<!--每当点击div元素，改变a的值，达到隐藏显示div元素的目的-->
    	...
	</div>
	<script>
   		var a=true;
	</script>

###### 2.ng-show指令
**定义：**ng-show 指令在表达式为 true 时显示指定的 HTML 元素，否则隐藏指定的 HTML 元素

**用法:**
`<div ng-show="expression">
    ...
</div>`

Expression是表达式，你可以自己定义。
###### 3.ng-hide指令
**定义：**指令在表达式为 true 时隐藏 HTML 元素

**用法:**


    <div ng-hide="expression">
        ...
    </div>

###### 4.ng-repeat指令
**定义：**指令用于循环输出指定次数的 HTML 元素
集合必须是数组或对象

**用法：**

	<body ng-app="moduleName" ng-controller="CtrlName">

		<h1 ng-repeat="item in group">{{item}}</h1>

	<script>
    var app = angular.module("moduleName", []);
    app.controller("CtrlName", function($scope) {
        $scope.group = [
            "RMB",
            "Doller",
            "Chnia",
            "Beijing"
        ]
    });
	</script>

	</body>


渲染出的DOM：

	<body ng-app="moduleName" ng-controller="CtrlName">
		<h1>RMB</h1>
		<h1>Doller</h1>
		<h1>Chnia</h1>
		<h1>Beijing</h1>
	</body>

Ng-reapt在处理后台数据时经常用到，后台返回的数据往往比这要复杂，可能是数组里包含了对象，比如在上面group里加一个List对象：

	$scope.group = [
         "RMB",
         "Doller",
         "Chnia",
         "List":{a:1,b:2}
    ]

要取得a的值，就要这么写：

	<h1 ng-repeat="item in group">{{item.List.a}}</h1>

更多内容敬请关注博客，欢迎和我交流哦*~*
