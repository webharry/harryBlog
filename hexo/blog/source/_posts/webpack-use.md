---
title: webpack2简单使用
date: 2017-07-18 20:17:28
tags:
---

webpack是一个模块打包器。能够对模块的依赖关系进行静态分析，然后根据指定规则生成静态资源。webpack2在配置文件格式上做了一些变动，比webpack1新增了

安装
----
前提是安装好[Node.js](https://nodejs.org/en/download/)，  
全局安装：

    npm install -g webpack

作为项目开发依赖安装：

    npm install --save-dev webpack

使用
----
这里以新建配置文件来执行，项目根目录下新建webpack.config.js文件，配置如下：

    var webpack = require('webpack');
    var path = require('path');

    var config = {
        entry:"entry.js",//入口文件路径
        output:{//输出文件
            path:path.resolve(__dirname,'build'),
            filename:"[name].bundle.js"
        },
        module:{//模块
            ...
        }
    }

    module.exports = config;

配置文件的结构大致是以上的结构。

运行
----

    $ webpack

常用xxx-Loader的使用：
----
style-loader和css-loader
----  

style-loader把js字符串生成为style节点；css-loader是把css转换为js输出。两者经常一起使用。

安装
----

    npm install --save-dev css-loader

使用
----
在webpack.config.js文件配置如下：

    module:{
        rules:[{
            test:/\.css$/,
            use:[
              { loader:"stycss-loader" },
              { loader:"css-loader" }
            ]
        }]
    }

html-loader
----  
html-loader是把html文件转换为js.  

安装
----  

    npm install --save-dev html-loader

使用
----  
在webpack.config.js文件配置如下：  

    module:{
        rules:[{
            test:/\.html$/,
            use:[
                {
                    loader:"html-loader",
                    option: {
                        minimize:true
                    }
                }
            ]
        }]
    }

less-loader
----
less-loader是把less编译为css.  

安装
----

    npm install --save-dev less-loader

使用
----
在webpack.config.js文件配置如下：

    module:{
        rules:[
            {
                test:/\.less$/,
                use:[
                    {
                        loader:"style-loader"
                    },
                    {
                        loader:"css-loader"
                    },
                    {
                        loader:"less-loader"
                    }
                ]
            }
        ]
    }

sass-loader
----  
sass-loader是将sass编译为css。
安装
----  

    npm install --save-dev sass-loader

使用
----
在webpack.config.js文件配置如下：  

    modu:{
        rules:[
            {
                test:/.\scss$/,
                use:[
                    {
                        loader:"style-loader"
                    },
                    {
                        loader:"css-loader",
                        option:{ sourceMap:true }
                    },
                    {
                        loader:"sass-loader",
                        option:{ sourceMap:true }
                    }
                ]
            }
        ]
    }

sourceMap:true用于启用CSS的source map  

插件使用
----
想要使用一个插件，你只需要require()它，然后把它添加到plugins数组中。  

BannerPlugin
----
内置插件BannerPlugin，用来给输出的文件头部添加注释信息。

使用
----
在webpack.config.js文件配置如下：   

    plugins:[
        new webpack.BannerPlugin('添加注释信息...')
    ]

HtmlWebpackPlugin
----
简化HTML文件的创建，为webpack包提供服务，可以用来生成HTML5文件。

安装
----

    npm install --save-dev html-webpack-plugin

使用
----
在webpack.config.js文件配置如下：  

    var webpack = require('webpack');
    var path = require('path');
    var HtmlWebpackPlugin = require('html-webpack-plugin');

    var config = {
        entry:"./entry.js",
        output:{
            path：path.resolve(__path,'build'),
            filename:"bundle.js"
        }
        plugins:[
            new HtmlWebpackPlugin()
        ]
    }

    module.exports = config;

这将会生成一个包含bundle.js引入信息的HTML文件。
