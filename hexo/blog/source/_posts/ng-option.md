---
title: 设置select默认值-Angular
date: 2017-06-08 23:25:27

---


1.HTML代码

    <body ng-app="MyModule">
    <div ng-controller="MyCtrl">
    <select ng-model="mylabel"ng-options="v.label for v in lists"></select>
    </div>
    </body>
 
2.JS代码

    var myModule =angular.module("MyModule", []);
    myModule.controller('MyCtrl', ['$scope',
    function($scope) {
    $scope.lists= [
    {"label": "步伐1","value" :"111"},
    {"label": "步伐2","value" :"233"}
    ];
    $scope.mylabel= $scope.lists[0];//默认数组第一个作为option的值
    }
    ]);
 
3.执行结果：默认显示“步伐1”
http://img.blog.csdn.net/20170110204541116?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2ViX2hhcnJ5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center
 
4.延伸--ng-options用法[1]
ng-optins属性可以在表达式中使用数组或对象来自动生成一个select的option列表，与ng-repeat类似。

一般用法：

对于数组：

    l v.value for v in array
    l v.value as v.label for v in array //将v.label值作为v.value

对于对象：

    l  label for (key , value) in object
    l  select as label for (key ,value) in object

5.引用：更多用法参考链接

[1].http://www.cnblogs.com/panda-zhang/p/5290694.html
 
 
 
