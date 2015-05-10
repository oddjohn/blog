title: "简单3步，使用weinre进行移动端页面调试"
date: 2015-05-09 22:28:51
tags: 
- weinre
- mobile
- remote debug
---
{% raw %}
<style>
    @media screen and (min-width: 980px) {
        .img-wrapper img {
            width: 50%;
            vertical-align: top;
        }
        .img-wrapper img[title=mobile] {
            width: 30%;
        }
    }
</style>
{% endraw %}
![weinre logo](http://people.apache.org/~pmuellr/weinre-docs/latest/images/weinre-icon-128x128.png)

[weinre](http://people.apache.org/~pmuellr/weinre-docs/latest/)全称WEb INspector REmote，发音像单词"winery"或者"weiner"。weinre是一个像FireBug那样的页面调试器（仅适用于基于webkit内核的浏览器）。它除了可以远程调试外，更重要的是它可以让你在手机这样的移动设备上进行调试。

## 1、安装
使用npm包管理器进行安装
``` bash
$ npm install -g weinre
```

## 2、运行
安装完成后，输入如下命令，weinre就会在你本机起个服务，其中`--httpPort`是监听的端口号，`--boundHost`的作用是绑定ip地址，默认是`localhost`。你也可以设置为自己本机的ip，设置为`-all-`可以兼容ip地址和localhost两种情况。
``` bash
$ weinre --httpPort 8069 --boundHost -all-
```
然后打开浏览器输入`http://localhost:8069`就可以已进入这样的界面
![](/blog/weinre/weinre.jpg)

## 3、调试页面
在需要调试页面中插入一段script标签，其中 *localhost* 替换成你机器的ip。

``` html
<script src="http://localhost:8069/target/target-script-min.js#anonymous"></script>
```
进入上面提到weinre的调试页面，然后用手机浏览器访问你需要调试的页面。当修改weinre调试页面中的样式时，手机中的页面样式也会跟着变化。
<div class="img-wrapper">
![debug](/blog/weinre/debug.jpg) ![mobile](/blog/weinre/mobile.jpg)
</div>
更多其他调试的方式，请查看[官方文档](http://people.apache.org/~pmuellr/weinre-docs/latest/UserInterface.html)。
