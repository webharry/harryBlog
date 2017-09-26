---
title: plugins-browsersync
date: 2017-08-14 18:25:02
tags:
---

Browsersync浏览器同步测试工具的使用
----

Browsersync能够快速响应文件(html、css、js、less、sass等)的更改，试想当你敲代码的同时，浏览器能够快速响应文件更改，免去手动刷新浏览器，对于开发者来说无疑会提高开发效率。

下面是分别是在gulp和webpack中的安装及使用方法。  

一、gulp + browser-sync安装
----
安装
----
前提是你已[安装gulp](https://webharry.github.io/2017/07/20/gulp-use/)
这里用Node.js的包管理安装，前提是你已安装[node.js]()。
全局安装：

    npm install -g browser-sync

作为项目开发依赖安装：

    npm install --save-dev browser-sync

使用：
----

在gulpfile.js文件配置如下：  

    var gulp = require('gulp');
    var browserSync = require('browser-sync').create();
    var reload = browserSync.reload;

    gulp.task('server',function() {
        browserSync.init({
            server: "./dist"//生产目录
        });

        gulp.watch("./src/**/*.less",['less']);
        gulp.watch("./src/**/*.html",['htmlmin']);
        gulp.watch("./dist/**/*.css").on('change',reload);
        gulp.watch("./dist/**/*.html").on('change',reload);
    });

    gulp.task('default',['server'],function() {

    });
