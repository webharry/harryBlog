---
title: BFC
date: 2017-09-26 19:57:25
tags:
    - CSS
    - frontend
---

### BFC是什么？

BFC(Block Formatting Context)块级格式化上下文，也是CSS一种盒模型渲染样式，w3c定义只要满足一下条件之一，就能创建一个BFC：

#### 1.float属性不为none;

#### 2.display属性值不为static和relative;

#### 3.display属性为以下之一：table-cell,table-caption,inline-block,flex,inline-flex；

#### 4.overflow属性不为visible。

### 解决了什么问题？

创建一个BFC很简单，只需包含上文定义中的4点任意一个就OK，来看看能解决什么问题：

#### 1.解决外边距折叠

```html

<div class="container">
  <div class="sub-div"></div>
  <div class="bfc">
    <div class="sub-div"></div>
  </div>
</div>

```

```js

.container{
  background-color:#ccc;
  height: 200px;
  width: 100%;
}
.sub-div {
  background-color: red;
  height:50px;
  margin：10px 0;
}
.bfc {
  overflow: hidden;//添加overflow属性，创建一个新BFC,从而避免外边距折叠
}

```

#### 2.用于清除浮动

```html

<div class="container">
    <div class="sub"></div>
    <div class="sub"></div>
</div>

```

```css

.container{
  background-color: #ccc;
  overflow:hidden;//通过给父元素添加overflow 属性，创建一个BFC,达到清除浮动效果。
}
.sub {
  float: left;
  height:30px;
  width:50px;
  background-color: red;
  margin:0 10px;
}
```

#### 3.多列布局中避免折行显示

```html

<div class="container">
    <div class="sub"></div>
    <div class="sub"></div>
    <div class="sub"></div>
</div>
```

由于浏览器舍入规则，子元素的宽度可能会超过容器总宽度，将会导致折行：

```css

* {
 margin:0;
}
.container{
  background-color: #ccc;
}
.sub {
  float: left;
  height:30px;
  background-color:green;
  width:31.33%;
  margin:0 1%;
}
.sub:last-child {//通过给最后一个子元素 创建一个BFC解决折行问题。
  float:none;
  overflow:hidden;
}
```

### 总结

BFC就是通过创建一个新的盒模型，使之脱离原本的布局限制，达到互不影响布局的效果。创建BFC 可以通过以上4种方式达到。