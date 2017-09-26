---
title: gulp使用
date: 2017-07-20 10:05:20
tags:
---

gulp是基于流的自动化构建工具，对代码进行打包压缩和管理，提供简单的API就能实现对复杂代码的管理，并且提供很多插件，大大提高了开发效率。

安装
----
前提是已安装好node.js,查看[node.js]()安装

全局安装命令

    npm install gulp -g  

-g代表全局安装gulp，这样在你所有的项目下都可以使用gulp

作为项目开发依赖安装命令  

    npm install --save-dev gulp  

--save-dev代表gulp将作为项目开发依赖安装，会在项目的package.js文件里添加gulp安装信息

使用
----
在项目根目录下新建gulpfile.js文件，在文件里配置如下：

    var gulp = require('gulp');

    gulp.task('default',function(){
      ...
    });

    gulp.task('taskname',function(){
      ...
    });


require引入gulp，之后就可以调用task()方法创建任务，进行管理。
task的第一个参数是任务名，当运行gulp时，默认执行default任务；task的第二个参数是一个函数，定义该任务要执行的一些操作。

运行
----
在命令行敲入命令：

    $ gulp

几种常用插件
----
gulp-uglify是用来压缩JS文件的   
安装

    npm install --save-dev gulp-uglify  

使用    
在gulpfile.js文件进行配置如下:   

    var uglify = require('gulp-uglify');

    gulp.task('uglify',function){
      gulp.src('src/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('dist'))
    });

gulp.src('文件路径')，将输出匹配路径的文件，并pipe到下一个插件中。*.js将匹配所有.js文件；src/**/*.js将匹配src所有深度下的所有.js文件。   
gulp.dest('目标文件路径')，将打包好的文件输出到目标路径下。

gulp-cssmin用来压缩css文件      
安装    

    npm install --save-dev gulp-cssmin

使用   
在gulpfile.js文件进行配置如下：

    var cssmin = require('gulp-cssmin');

    gulp.task('cssmin',function(){
      console.log('cssmin');
      gulp.src('src/css/**/*.css')
       .pipe(cssmin())
       .pipe(gulp.dest('dist/css'))；
    });

  这里将匹配src/css目录下的所有.css文件，用pipe()实现基于流的输出管理。   

  gulp-html-minify压缩html文件    
  安装    

      npm install --save-dev gulp-html-minify

使用    
在gulpfile.js文件进行配置如下：

    var htmlmin = require('gulp-html-minify');
    gulp.task('html',function(){
        gulp.src('src/**/*.js')
            .pipe(htmlmin())
            .pipe(gulp.dest('dist'));
    })

gulp-less将less文件编译成css文件    
安装    

    npm install --save-dev gulp-less    

使用    
在gulpfile.js文件配置如下：    

    var less = require('gulp-less');
    gulp.task('less',function(){
        gulp.src('src/**/*.less')
            .pipe(less())
            .pipe(cssmin())
            .pipe(gulp.dest('dist'));
    });      

gulp-sass用来编译sass文件为css文件  
安装  

    npm install --save-dev gulp-sass  

使用  
在gulpfile.js文件配置如下：  

    var sass = require('gulp-sass');
    gulp.task('sass',function(){
       gulp.src('src/**/*.scss')
           .pipe(sass())
           .pipe(cssmin())
           .pipe(gulp.dest('dist/css'));
    });  
