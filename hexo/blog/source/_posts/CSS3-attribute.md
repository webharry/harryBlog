---
title: CSS3几种常用属性
date: 2017-08-22 18:20:26
tags:
---
/*text-shadow
*可以给一个对象应用一组或多组阴影效果，方式如前面的语法显示一样，用逗号隔开。
*text-shadow: X-Offset Y-Offset Blur Color中
*X-Offset表示阴影的水平偏移距离，其值为正值时阴影向右偏移，如果其值为负值时，阴影向左偏移；
*Y-Offset是指阴影的垂直偏移距离，如果其值是正值时，阴影向下偏移反之其值是负值时阴影向顶部偏移；
*Blur是指阴影的模糊程度，其值不能是负值，如果值越大，阴影越模糊，反之阴影越清晰，如果不需要阴影模糊可以将Blur值设置为0；
*Color是指阴影的颜色，其可以使用rgba色。著作权归作者所有。
*原文: https://www.w3cplus.com/blog/52.html © w3cplus.com
*/

    h1 {
        text-shadow: 1px 1px 1px #FF0000, 0 0 0 #ccc, 2px 2px 2px #ff00de;
    }


/*
*transform-style
*使转换的子元素保留其3D转换
*transform-style 属性规定如何在 3D 空间中呈现被嵌套的元素。
*语法：transform-style: flat|preserve-3d;
*注释：该属性必须与 transform 属性一同使用。
*transform :旋转 div 元素. http://www.w3school.com.cn/cssref/pr_transform.asp
*rotateX(angle)定义沿着 X 轴的 3D 旋转
*rotateY(angle)定义沿着 Y 轴的 3D 旋转
*/

    .box {
        width: 10rem;
        height: 1rem;
        background: #ccc;
        border: 1px solid #FF0000;
        -webkit-transform: rotateY(60deg);
        transform: rotateY(60deg);
        -webkit-transform-style: preserve-3d;
        transform-style: preserve-3d;
    }


/*
*animation
*使用简写属性，将动画与 div 元素绑定
*animation-name 规定需要绑定到选择器的 keyframe 名称
*animation-duration 规定完成动画所花费的时间，以秒或毫秒计
*animation-timing-function规定动画的速度曲线 ease-in-out动画以低速开始和结束。
*animation-delay transform 规定在动画开始之前的延迟
*animation-iteration-count 规定动画应该播放的次数. infinite规定动画应该无限次播放.
*animation-direction 规定是否应该轮流反向播放动画
*/

    .box {
        width: 6rem;
        height: 1rem;
        background: #ccc;
        border: 1px solid #FF0000;
        -webkit-transform: rotateY(180deg);
        transform: rotateY(180deg);
        -webkit-transform-style: preserve-3d;
        transform-style: preserve-3d;
        animation: run ease-in-out 1s infinite;
        -moz-animation: run ease-in-out 1s infinite;
        -webkit-animation: run ease-in-out 11s infinite;
        -ms-animation: run ease-in-out 1s infinite;
    }


/*
@keyframes 规则
*通过 @keyframes 规则，您能够创建动画。
*创建动画的原理是，将一套 CSS 样式逐渐变化为另一套样式。
*在动画过程中，您能够多次改变这套 CSS 样式。
*以百分比来规定改变发生的时间，或者通过关键词 "from" 和 "to"，等价于 0% 和 100%。
*0% 是动画的开始时间，100% 动画的结束时间。
*为了获得最佳的浏览器支持，您应该始终定义 0% 和 100% 选择器。
*注释：请使用动画属性来控制动画的外观，同时将动画与选择器绑定。
*/

    @keyframes run {
        0% {
            -webkit-transform: rotateX(-5deg) rotateY(0);
            transform: rotateX(-5deg) rotateY(0);
        }
        50% {
            -webkit-transform: rotateX(0) rotateY(180deg);
            transform: rotateX(0) rotateY(180deg);
            text-shadow: 1px 1px 1px #ccc, 0 0 10px #fff, 0 0 20px #fff, 0 0 30px #fff, 0 0 40px #3EFF3E, 0 0 70px #3EFFff, 0 0 80px #3EFFff, 0 0 100px #3EFFee, 0 0 150px #3EFFee;
        }
        100% {
            -webkit-transform: rotateX(5deg) rotateY(360deg);
            transform: rotateX(5deg) rotateY(360deg);
        }
    }

    @-moz-keyframes run {
        0% {
            -moz-transform: rotateX(-5deg) rotateY(0);
        }
        50% {
            -moz-transform: rotateX(0) rotateY(180deg);
            text-shadow: 1px 1px 1px #ccc, 0 0 10px #fff, 0 0 20px #fff, 0 0 30px #fff, 0 0 40px #3EFF3E, 0 0 70px #3EFFff, 0 0 80px #3EFFff, 0 0 100px #3EFFee, 0 0 150px #3EFFee;
        }
        100% {
            -moz-transform: rotateX(5deg) rotateY(360deg);
        }
    }

    @-webkit-keyframes run {
        0% {
            -webkit-transform: rotateX(-5deg) rotateY(0);
        }
        50% {
            -webkit-transform: rotateX(0) rotateY(180deg);
            text-shadow: 1px 1px 1px #ccc, 0 0 10px #fff, 0 0 20px #fff, 0 0 30px #fff, 0 0 40px #3EFF3E, 0 0 70px #3EFFff, 0 0 80px #3EFFff, 0 0 100px #3EFFee, 0 0 150px #3EFFee;
        }
        100% {
            -webkit-transform: rotateX(5deg) rotateY(360deg);
        }
    }

    @-ms-keyframes run {
        0% {
            -ms-transform: rotateX(-5deg) rotateY(0);
        }
        50% {
            -ms-transform: rotateX(0) rotateY(180deg);
        }
        100% {
            -ms-transform: rotateX(5deg) rotateY(360deg);
        }
    }
