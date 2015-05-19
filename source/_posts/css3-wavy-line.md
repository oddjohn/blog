title: "CSS3绘制波浪线"
date: 2015-05-18 21:52:51
tags:
- css3
- radial-gradient
- 波浪线
---

## 开篇
某些业务场景下常会用到波浪线边缘来模拟邮票或者其他各种票的样式，如下demo就是模拟电影票，优惠券之类的样式。传统的做法是切图然后用`background-repeat`来生成一条完整的波浪线。本文将介绍使用CSS3来实现这么一个波浪线。

{% raw %}
<div class="demo">
    <div class="ticket">
        <div class="head">电影票</div>
        <div class="body">
            太平洋电影城(天利店)  2号厅<br>
            5月18日 周一 23:37<br>
            2排10座  4排10座<br>
            总价：￥152
        </div>
        <div class="h-wave"></div>
    </div>
    <div class="coupon selected">
        <div class="content">
            <div class="title">优惠券</div>
            <div class="des">复仇者联盟2立减10元，限量10000张，每人1张，先到先得</div>
            <div class="member">会员专享：会员卡支付立减10元/张</div>
            <div class="circle-checkbox"></div>
        </div>
        <div class="wave"></div>
        <div class="wave right"></div>
    </div>
</div>
{% endraw %}

## 工具
要实现上面的波浪线边缘的效果，最主要借助CSS3中的*径向渐变*属性`radial-gradient`。没错，它一个用来实现渐变的属性，渐变是从一种颜色逐渐过度到另一种颜色，像这样：
{% raw %}
<div class="radial-test">
</div>
{% endraw %}
了解radial-gradient的更详细的介绍请查看{% link css.doyoe.com http://css.doyoe.com/values/image/radial-gradient().htm %}，这里就不再一一介绍。

## 实现
基础代码如下：
``` html
<div class="wrapper">
    <div class="wavy-line"></div>
</div>
```
``` css
.wrapper {
    padding: 10px;
    background-color: #eee;
}
.wavy-line {
    width: 100%;
    height: 50px;
}
```
首先画一个半径为10px的圆(不是用border-radius哦),效果如下：
``` css
.wavy-line {
    background-image: -webkit-radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
    background-image: -moz-radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
    background-image: radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
}
```
{% raw %}
<div class="wrapper">
    <div class="wavy-line step1"></div>
</div>
<style>
    .wrapper {
        padding: 10px;
        background-color: #eee;
    }
    .wavy-line {
        width: 100%;
        height: 50px;
        background-image: -webkit-radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
        background-image: -moz-radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
        background-image: radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
    }
    .step2 {
        background-size: 20px 20px;
    }
    .step3 {
        height: 10px;
    }
    .step4 {
        background-image: -webkit-radial-gradient(circle, transparent, transparent 9px, orange 9px, orange 10px, transparent 10px, transparent);
        background-image: -moz-radial-gradient(circle, transparent, transparent 9px, orange 9px, orange 10px, transparent 10px, transparent);
        background-image: radial-gradient(circle, transparent, transparent 9px, orange 9px, orange 10px, transparent 10px, transparent);
    }
</style>
{% endraw %}
以上`transparent`是透明色，上面的css代码意思是从0px开始，到10px的位置都是透明色，然后从10px开始，一直到结束都是用白色。用这种一个颜色的开始即另一个颜色的结束的方法，可以巧妙的避过颜色的逐渐过度，画出边线分明的圆形。
现在默认这个圆以外的白色部分是占满整个屏幕的，接着，我们要限定包围圆的区域的大小，此处用`background-size`，效果如下:
``` css
.wavy-line {
    background-image: -webkit-radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
    background-image: -moz-radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
    background-image: radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
    background-size: 20px 20px;
}
```
{% raw %}
<div class="wrapper">
    <div class="wavy-line step2"></div>
</div>
{% endraw %}
上面代码设置了背景图案的大小是20px*20px，前面设置了半径为10px的圆，这样的背景大小刚好可以包含直径为20px的圆，还预留出2px。
接下来要改变高度，使得高度刚好适应半个圆：
``` css
.wavy-line {
    width: 100%;
    height: 10px;
    background-image: -webkit-radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
    background-image: -moz-radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
    background-image: radial-gradient(circle, transparent, transparent 10px, #fff 10px, #fff);
    background-size: 20px 20px;
}
```
{% raw %}
<div class="wrapper">
    <div class="wavy-line step2 step3"></div>
</div>
{% endraw %}
可以看到，一条波浪线就出来了，如果还想给波浪线再加上边框，可以继续在`radial-gradient`中增加一层颜色，像这样:
``` css
.wavy-line {
    width: 100%;
    height: 10px;
    background-image: -webkit-radial-gradient(circle, transparent, transparent 9px, orange 9px, orange 10px, transparent 10px, transparent);
    background-image: -moz-radial-gradient(circle, transparent, transparent 9px, orange 9px, orange 10px, transparent 10px, transparent);
    background-image: radial-gradient(circle, transparent, transparent 9px, orange 9px, orange 10px, transparent 10px, transparent);
    background-size: 20px 20px;
}
```
{% raw %}
<div class="wrapper">
    <div class="wavy-line step2 step3 step4"></div>
</div>
{% endraw %}
## 小结
发挥你的想象力，可以使用`linear-gradient`和`radial-gradient`实现更多有趣的图案。

{% raw %}
<style>
    .demo {
        padding: 10px;
        background-color: #efefef;
        font-size: 14px;
        overflow: hidden;
    }
    .demo > div {
        float: left;
        margin: 0 20px;
    }
    .ticket {
        width: 300px;
        background-color: #fff;
    }
    .head {
        color: #fff;
        padding: 5px 10px;
        font-size: 16px;
        background: #ff4683;
    }
    .body {
        padding: 10px;
    }
    .h-wave::before {
        content: "";
        display: block;
        height: 5px;
        width: 100%;
        background-image: -webkit-radial-gradient(#efefef 0px, #efefef 3px, #d7dbde 3px, #d7dbde 4px, transparent 4px, transparent);
        background-image: -moz-radial-gradient(#efefef 0px, #efefef 3px, #d7dbde 3px, #d7dbde 4px, transparent 4px, transparent);
        background-image: radial-gradient(#efefef 0px, #efefef 3px, #d7dbde 3px, #d7dbde 4px, transparent 4px, transparent);
        background-size: 10px 10px;
        background-position: 0 10px;
    }
    .h-wave {
        height: 4px;
        border-bottom: 1px solid #ccc;
    }
    .wave {
        position: absolute;
        left: 0;
        top: 0;
        height: 100%;
        width: 4px;
        background-image: -webkit-radial-gradient( transparent 0px, transparent 3px, #ccc 3px, #ccc 4px, white 4px, white);
        background-image: -moz-radial-gradient( transparent 0px, transparent 3px, #ccc 3px, #ccc 4px, white 4px, white);
        background-image: radial-gradient( transparent 0px, transparent 3px, #ccc 3px, #ccc 4px, white 4px, white);
        background-size: 8px 8px;
        background-position: -4px 1px;
    }
    .coupon {
        position: relative;
        width: 300px;
        height: 130px;
        background-color: #fff;
        border-top: 1px solid #ccc;
        border-bottom: 1px solid #ccc;
        padding: 0 4px;
        background-clip: content-box;
        margin-bottom: 10px;
    }
    .wave.right {
        left: initial;
        right: 0;
        background-position: 0px 1px;
    }
    .content {
        margin: 14px 42px 14px 25px;
        font-size: 12px;
    }
    .title {
        margin-bottom: 10px;
        font-size: 18px;
    }
    .des {
        color: #666;
        margin-bottom: 10px;
        line-height: 1.2em
    }
    .member {
        color: #ff4683;
    }
    .coupon.selected {
        border-top: 1px solid #ff7d02;
        border-bottom: 1px solid #ff7d02;
    }
    .selected .wave {
        background-image: -webkit-radial-gradient(transparent 0px, transparent 3px, #ff7d02 3px, #ff7d02 4px, white 4px, white);
        background-image: -moz-radial-gradient(transparent 0px, transparent 3px, #ff7d02 3px, #ff7d02 4px, white 4px, white);
        background-image: radial-gradient(transparent 0px, transparent 3px, #ff7d02 3px, #ff7d02 4px, white 4px, white);
    }
    .circle-checkbox {
        display: none;
        width: 33px;
        height: 33px;
        border-radius: 17px;
        border: 1px solid #ff7d02;
    }
    .circle-checkbox::before {
        content: "";
        display: inline-block;
        width: 3px;
        height: 10px;
        border-radius: 2px 2px 0 0;
        background-color: #ff7d02;
        transform: rotate(-45deg) translate(2px, 13px);
    }
    .circle-checkbox::after {
        content: "";
        display: inline-block;
        width: 3px;
        height: 15px;
        border-radius: 2px 2px 0 0;
        background-color: #ff7d02;
        transform: rotate(45deg) translate(17px, -4px);
    }
    .selected .circle-checkbox {
        display: block;
        position: absolute;
        top: 50%;
        right: 10px;
        margin-top: -17px;
    }
    .radial-test {
        width: 200px;
        height: 100px;
        background-image: -webkit-radial-gradient(#3588f8, #fce62d, #fb6806);
        background-image: -moz-radial-gradient(#3588f8, #fce62d, #fb6806);
        background-image: radial-gradient(#3588f8, #fce62d, #fb6806);
    }
</style>
{% endraw %}