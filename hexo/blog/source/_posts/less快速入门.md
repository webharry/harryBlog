---
title: less
date: 2017-03-11 14:50:17
tags:
---
**Less语言快速入门**

**介绍：**Less是一门CSS预处理语言，它扩充了CSS语言，重点来了：它增加了诸如变量、混合（Mixins)、函数等功能，这让静态的CSS语言强大了有木有，方便制作主题（比如换肤）、扩充，更易维护。

**使用：**

**1.变量**

Less允许使用变量事先定义一些通用样式，在需要的时候引入，特点就是按需自取，自助餐有木有。
看下面栗子：

	//这里是一些简单的LESS语法
	@width:400px;
	@height:300px;
	@font_size:12px;
	@color:#ccc;
	@font-size:14px;
	textarea {
		width:@width;
		height:@height;
		font-size:@font_size;
	}
	.test {
	  color:@color;
	  background-color:@color;
	  font-size:@font-size;
	}
Less编译后的对应CSS：

	textarea {
	  width: 400px;
	  height: 300px;
	  font-size: 12px;
	}
	.test {
	  color: #cccccc;
	  font-size: 14px;
	  background-color: #cccccc;
	}
这里要注意，Less以‘@’符号作为开头来定义变量，就像JS里以var定义变量一样，且变量的值只能是属性值，不能是变量，比如说，我这样定义一个变量：

	@font:font-size;
	.test{
	@font:14px;  //这样做是不行的，less不会编译成功！
	}
**2.后代选择器---可以嵌套**

	@color:#ccc;


	div {
	  color:@color;
		p{   //添加嵌套样式
	    	color:@color;
	 	}
	  &:after{   //通过&添加伪类
	    color:#ddd;
	  }
	}

Less编译后的CSS:

	div {
	  color: #cccccc;
	}
	div p {
	  color: #cccccc;
	}
	div:after {
	  color: #ddd;
	}

这种方法也是比较常用的啦~小编我学会后就来推荐啦

**3.文件引用**

	@import “文件名”;   //文件名可以是相对路径

文件引用有两个常用方法：

①将全局变量引入到样式类文件时，使用’@import “文件名.less”;‘语句引入即可

②另一种常见用法是将初始化的Less文件引入，当需要样式类时，引入初始化Less文件即可。

**4.混合（Mixins)**

混合可以将已经写好的样式A引入到样式B,从而实现样式B对样式A所有属性的继承。

	.border-radius(@radius: 4px) {
	  -webkit-border-radius: @radius;
	  -moz-border-radius: @radius;
	  border-radius: @radius;
	}
	#form-box {
	  .border-radius;
	  div{
	    .border-radius(14px);
	  }
	}

Less编译后的CSS：

	#form-box {
	  -webkit-border-radius: 4px;
	  -moz-border-radius: 4px;
	  border-radius: 4px;
	}
	#form-box div {
	  -webkit-border-radius: 14px;
	  -moz-border-radius: 14px;
	  border-radius: 14px;
	}

这里提一个比较有意思的变量@arguments，它包含了所有传递进来的参数，如果不想一个个写参数就可以使用它。

	.margin (@top:0, @right:0, @bottom:0, @left:0) {
	  margin:@arguments;
	}
	div {
	  .margin(2px,5px);
	}

Less编译后的CSS：

	div {
	  margin: 2px 5px 0 0;
	}
**5.函数**

	div {
	  .fun(100px);  //引用函数
	}
	.fun(@px){  //函数声明和JS里的很像，括号加参数
	  width:@px;
	}
	Less编译成CSS：
	div {
	  width: 100px;
	}
注意到没有，函数是不编译的
