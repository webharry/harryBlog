---
title: 如何在npm上发布一个插件
date: 2017-12-03 16:04:53
tags:
    - frontend
    - JavaScript
    - npm
---
今天尝试在在npm上发布一个插件，做下笔记给大家分享。
### 1.在GitHub新建一个项目
这次用我自己写的一个gulp插件来做例子，演示下如何一步一步在npm上发布自己的插件。  

首先，在GitHub上新建一个项目，我的插件取名叫gulp-px-to-rem,如图:
![图3](https://upload.cc/i/UqKSsB.jpg)
新建成功后，将gulp-px-to-rem项目克隆到本地。这就是即将发布到npm的插件项目:
![图片2](https://upload.cc/i/9bi6o1.jpg)

### 2.在npm注册一个账号
访问npm官网注册一个账号：https://www.npmjs.com。  
用命令行登录：
```shell
npm login
```

### 3.发布插件
进入本地项目gulp-px-to-rem的根目录下:  

```shell
npm publish
```
发布完成后，在npm官网下搜索我们刚发布的插件：
![图1](https://upload.cc/i/08il1d.jpg)
如上，搜搜到插件gulp-px-to-rem,表示已经发布成功！  

### 发布常见问题
1.小坑：注意每次再发布的时候，到package.json文件改下版本号再发布，否则会报错。  
2.从github克隆项目下来后，记得使用npm init初始化项目，npm install 安装项目依赖。

备注：
为了在GitHub同步代码，可以把每次发布更新到GitHub。只需三步命令：
```shell
git add .
git ci -m '备注'
git push
```
