---
title: gulp构建一个项目
date: 2017-08-23 14:02:00
tags:
---
介绍：这篇文章总结了使用gulp构建一个项目的详细过程。包括开发环境搭建，构建项目并用于生产部署。  

gulp自动化构建工具具备以下特点：
基于流、任务化
常用API：src、dest、watch、task、pipe

一．安装
----
1.安装gulp：(注意这里默认大家已经安装了npm和nodejs)
新建项目文件，cmd切换到项目目录下，这里我新建了项目文件webapp  

安装全局gulp：

    npm i -g gulp


2.接下来安装node依赖文件
首先用npm初始化项目配置文件：

    npm init

然后命令行一直回车就行，配置文件之后可以更改


3.作为项目的开发依赖（devDependencies）安装：在当前目录安装gulp
在当前目录安装gulp:

    npm i --save-dev gulp

--save-dev的意思是将当前文件保存到配置文件当中，nodejs模块保存到packge.json文件当中，当我们需要项目保存到git仓库的时候，只需要保存packge.json文件到git仓库就行。当别人需要编译你的项目，只要npm i就可以拿到你的项目配置信息。

我们可以看下package.json文件：在依赖模块添加了gulp信息

然后我们看项目文件夹webapp的node_modules文件下多了个gulp文件夹，这个就是我们刚添加的gulp模块

4.扩展插件--在当前项目文件webapp目录下，安装gulp依赖文件。
安装命令：

    npm i --save--dev 插件名称

需要安装的gulp插件：

    gulp-clean  
    gulp-concat（文件合并）  
    gulp-connect  
    gulp-minify-html（html压缩）  
    gulp-minify-css（CSS压缩）
    gulp-cssmin（CSS压缩）（任选一个）  
    gulp-jshint（JS代码检查）
    gulp-imagemin（压缩图片）  
    gulp-less（编译Less）    
    gulp-sass（编译Sass）
    gulp-load-plugins  
    gulp-uglify （JS压缩）
    gulp-livereload（自动刷新）
    open

比如说：

    npm i --save--dev gulp-clean

如果你不想一个个安装，有个批量安装的小窍门：

    npm i --save--dev gulp-clean gulp-concat gulp-connect ...

各个模块之间以空格隔开。
安装好之后呢，我们在package.json文件里可以看到这些依赖插件

5.进行配置
在webapp项目根目录下，新建gulpfile.js文件，在文件中写入如下配置代码：

    var gulp = require('gulp');          //引入模块
    var $ = require('gulp-load-plugins');  //有了这个模块，其他的模块可以以$符号引入
    var open = require('open');         //引入open

    var app = {//声明一个全局变量，定义项目的路径
        srcPath : 'src/',      //源文件
        devPath : 'build/',   //开发环境--整合之后的路径
        prdPath : 'dist/'     //生成部署
    };

6.拷贝命令
定义任务的API，第二个参数是一个回调函数，在回调函数里我们写入逻辑操作代码

    gulp.task('任务名',function () {
        gulp.路径(路径)		//读取文件
        .pipe(...操作文件)
        .pipe(gulp.dest(目录))	//生成文件
    })


二．使用
----
应用实例：
1.拷贝HTML：
a.新建文件夹src存放源代码


b.新建完之后，我们在gulpfile.js文件下敲入如下代码进行拷贝html：（前提之前已配置好各个路径）

    gulp.task('html',function () {
        gulp.src(app.srcPath + '**/*.html')
        .pipe(gulp.dest(app.devPath))  //pipe是一个API  写文件的API--dest 拷贝到app.devPath + 'vendeor'
        .pipe(gulp.dest(app.prdPath))     //生成
        .pipe($.connect.reload());//通知服务器刷新浏览器，ie8不支持
    })

c.可以测试下，命令行打开，在项目路径下，输入

    gulp html

d.执行成功后可以看到项目文件下生成了新的文件夹build


2.拷贝第三方依赖库lib：前提是安装好bower并且用bower安装了第三方依赖文件（本例使用Bower安装了Angular，在bower_componets文件下）
题外话：全局安装bower：

    npm install bower -g

Bower安装第三方依赖Angular:

    bower install --save angular

开始拷贝JS：
a.在gulpfile.js文件下敲入如下代码进行拷贝js：（前提之前已配置好各个路径）

    gulp.task('lib',function () {
        gulp.src('bower_components/**/*.js')
        .pipe(gulp.dest(app.devPath + 'vendor'))
        .pipe(gulp.dest(app.prdPath + 'vendor'))
        .pipe($.connect.reload());//通知服务器刷新浏览器，ie8不支持
    })

b.命令行进入项目路径，执行命令：

    gulp lib

c.如果在build目录下生成vendor文件夹及其angular文件，表示成功。



3.拷贝CSS
a.src目录新建目录style，新建文件index.less和1.less，在两个文件编写测试代码。

b.然后在gulpfile.js文件新建任务：

    gulp.task('less',function () {
        gulp.src(app.srcPath + 'style/index.less')
            .pipe($.less())                     //别忘了less后的括号
            .pipe(gulp.dest(app.devPath + 'css'))
            .pipe($.cssmin())                  //压缩
            .pipe(gulp.dest(app.prdPath + 'css'))
            .pipe($.connect.reload());//通知服务器刷新浏览器，ie8不支持
    });

c.运行，在命令行输入

    gulp less



4.拷贝JS
同样的套路：
a.src下新建目录script，在script里新建文件1.js和2.js，随便写点测试代码。

b.新建gulp任务：
在gulpfile.js文件下添加如下一段代码即可：

    gulp.task('js',function () {
        gulp.src(app.srcPath + 'script/**/*.js')
            .pipe($.concat('index.js'))			//生成
            .pipe(gulp.dest(app.devPath + 'js'))    //写入开发环境
            .pipe($.uglify())					//压缩
            .pipe(gulp.dest(app.prdPath + 'js'))    //部署到生产环境
            .pipe($.connect.reload());//通知服务器刷新浏览器，ie8不支持
    });

c.运行
在命令行输入：

    gulp js

d.查看运行结果，在生产环境dist目录下生成js文件夹及其index.js文件。


5.拷贝image
套路是一样的啊，
a.在src里新建image目录，在该目录里新建一张图片，比如1.png
b.新建gulp任务，在gulpfile.js文件下添加如下代码：

    gulp.task('image',function () {
        gulp.src(app.srcPath + 'image/**/*')           //读取文件
            .pipe(gulp.dest(app.devPath + 'image'))		//拷贝到开发环境
            .pipe($.imagemin()) 					//压缩图片
            .pipe(gulp.dest(app.prdPath + 'image'))    //部署到生产环境
            .pipe($.connect.reload());//通知服务器刷新浏览器，ie8不支持
    });

c.项目路径下运行下：

    gulp image


d.查看运行结果，在生产环境目录dist里生产image目录及其文件

6.在我们部署到生产环境之后，往往需要清除build和dist 目录，避免出现旧的影响新的。这时候就需要新建一个清除任务：
在gulpfile.js文件下添加如下代码块：

    gulp.task('clean',function () {  //清除
        gulp.src([app.devPath,app.prdPath]) //要删除目录
        .pipe($.clean());
    });

运行：

    gulp clean


7新建gulp总任务。在我们真正部署的时候，往往通过新建一个gulp总任务来执行之前的所有打包压缩任务，减少繁琐操作，代码如下：

    gulp.task('build',['image','js','less','lib','html','json']);  //总任务  执行gulp build即可

运行：

    gulp build


8新建服务----自动化启动总任务build和服务器
在gulpfile.js文件下，敲入如下新建服务代码：

    //新建服务
    gulp.task('serve',['build'],function () {
        $.connect.server({        //启动一个服务器
            root:[app.devPath],   //从开发目录下读取
            livereload:true,      //自动刷新浏览器，ie不支持
            port:1234           //端口
        });
        open('http://localhost:1234');   //自动打开网址，打开浏览器

        //监听
        gulp.watch('bower_components/**/*',['lib']);
        gulp.watch(app.srcPath + '**/*.html',['html']);
        gulp.watch(app.srcPath + 'data/**/*.json',['json']);
        gulp.watch(app.srcPath + 'style/**/*.less',['less']);
        gulp.watch(app.srcPath + 'script/**/*.js',['js']);
        gulp.watch(app.srcPath + 'image/**/*',['image']);
    });

8.1有一个小技能，我们可以新建一个gulp默认任务，让它运行gulp的时候自动启动服务serve：

    //建立默认启动任务
    gulp.task('default',['serve']);//default 依赖serve


可以测试一下，命令行敲入：

    gulp

然后等待运行结束，浏览器会自动打开网址：http://localhost:1234/
你可以在src目录下的index.html页面敲入代码：hello world!
此时你会发现网页自动刷新了

当一个程序员打出hello world时就像中乐透一样哈哈:)
