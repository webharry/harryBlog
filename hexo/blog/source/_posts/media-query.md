---
title: media query
date: 2017-12-25 19:23:28
tags:
    - CSS
    - frontend
---
### 1.媒体查询（Media query）  
* 原理  
通过对不同设备（比如屏幕宽度width）设置不同的css样式，达到响应式页面效果。
* 使用
在css文件：
```css
@media (max-width:670px;) {

 //设置670px像素下的css样式

}

@media (max-width:860px;) {

 //设置在860px屏幕宽度下的css样式

}

@media (max-width:1200px;) {

 //设置1200px屏幕宽度下的css样式

}
```
### 2.媒体查询引用

在html文件<head>标签里加入以下代码：  

```html

<html>
  <head>
  
    <meta name="viewport" content="width=device-width,
    initial-scale=1.0, maximum-scale=1.0, user-scalable=no">//媒体查询引用
    <link rel="stylesheet" href="main.less">
  </head>
</html>
```

参数解释：

* width = device-width：宽度等于当前设备的宽度

* initial-scale：初始的缩放比例（默认设置为1.0）  

* minimum-scale：允许用户缩放到的最小比例（默认设置为1.0）    

* maximum-scale：允许用户缩放到的最大比例（默认设置为1.0）   


### 3.响应式布局小技能
* 在响应式布局中，一般设置元素的宽度和高度为百分比值，不建议给元素固定的px值。
#### 实例：  

登录界面中的表单dome.  
index.html文件：
```html
<div class="login">
    <h6>LOGIN QUICK</h6>
    <input type="text" class="form" placeholder="E-mail">
    <input type="text" class="form" placeholder="Password">
    <p class="forget">Forgot Password?</p>
    <button class="btn">Login</button>
</div>
```

css样式(main.less文件)：

```css
.login {
    width:26%;//先设置一个初始值，为百分比值
    margin: 7em auto 9em;
    h6 {
        font-size: 1em;
        margin-top:90px;
        margin-bottom: 30px;
        letter-spacing: 4px;
    }
    .form {
        display: block;
        width:100%;
        font-size:.9em;
        border-radius: 40px;
        margin:15px auto;
        padding:1em 3em 1em 1em;
        box-sizing: border-box;
        border: none;
        outline: none;
        letter-spacing: 1px;
        font-weight: 400;
        font-family: 'Open Sans', sans-serif;
        background: rgba(255, 255, 255, 0.85);
    }
    .forget {
        text-align: right;
        font-size: 14px;
    }
    .btn {
        width: 56%;
        height: 40px;
        background-color: #ea2858;
        border-radius: 40px;
        border: none;
        margin:15px 0;
        color:#fff;
        font-size: 14px;
        outline: none;
        cursor: pointer;
        // transition: all .1s linear;
        transition: background-color .5s linear;
        font-weight: 400;
        font-family: 'Open Sans', sans-serif;
        &:hover {
            color: black;
            background-color: white;
        }
    }
}
//利用媒体查询，在不同设备上设置.login的宽度，以达成宽度自适应
@media screen and (min-width: 900px) and (max-width:1200px) {
    .login {
        width:36%;
        margin: 6em auto 5.7em;
    }
}
@media screen and (max-width: 768px) {
    .login {
        width:42%;
        margin: 5.4em auto 5em;
    }
}

@media (max-width: 736px) {
    .login {
        width:56%;
        margin: 5em auto 4em;
   }
}

@media screen and(max-width: 375px) {
    .login {
        width:90%;
        margin: 3.7em auto 4em;
    }
}
```
demo效果图：  

pc大屏（1366x866）：  

![1366x866](https://upload.cc/i/QAft45.jpeg)  


移动端：  
iPhone8:  


![iPhone8](https://upload.cc/i/RDugIJ.jpeg)  

iPad:  


![iPad](https://upload.cc/i/2MRqXS.jpeg)  

超小屏（389x866）：  


![389x866](https://upload.cc/i/zWod3S.jpeg)

备注：如果在这里设置login固定宽度：
```css
.login {
 width:600px;
}
```
那么网页显示在小屏幕上就会有问题，无法呈现响应式效果。  

* 小结：通过媒体查询，可以实现网页响应式，PC端和移动端渲染是木有问题啦！
