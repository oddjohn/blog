title: "浅谈js模块化之AMD&CMD"
date: 2015-05-10 14:47:52
tags:
- 模块化
- CMD
- AMD
---
## AMD&CMD
* [AMD](https://github.com/amdjs/amdjs-api/wiki/AMD)（Asynchronous Module Definition），代表作requirejs，作者 James Burke；
* [CMD](https://github.com/cmdjs/specification/blob/master/draft/module.md)（Common Module Definition），代表seajs，作者玉伯。

## 依赖解析

### 1、Function.prototype.toString()

通过`Function.prototype.toString()`这样的方法，把`define(factory)`中为function的factory转为string，然后就可以通过正则匹配去分析依赖了。（像Angularjs中的依赖注入那么的智能，也是用这种方式去toString()然后匹配的）。

### 2、依赖分析

以下就是seajs和requirejs用来匹配的正则

{% codeblock sea.js lang:javascript %}
modName = /^require\s*\(\s*(['"]).+?\1\s*\)/.test(s2)
{% endcodeblock %}

{% codeblock require.js lang:javascript %}
cjsRequireRegExp = /[^.]\s*require\s*\(\s*["']([^'"\s]+)["']\s*\)/g
{% endcodeblock %}

正因为这样的匹配模式，所以下面这种模块依赖的写法是不被支持的：
``` javascript
// error
define(function(require){
    var text = isModA ? 'a' : 'b';
    var mod = require('mod' + text);
});
```
可以这样写：
``` javascript
// 可以这样写
define(function(require){
    var mod;
    if (isModA) {
        mod = require('moda');
    }
    else {
        mod = require('modb');
    }
});
```

### 3、加载依赖
通过页面插入`script`标签来加载js文件，js文件下载完成之后，`factory`的执行时机seajs和requirejs的表现是不一样的。
* seajs：仅加载，不执行`factory`（懒执行）
* requirejs:： 加载，并执行`factory` (预解析)

下面来看第一个例子
``` javascript
// a.js
define(function (require) {
    console.log('执行了a');
});
```
``` javascript
// b.js
define(function (require) {
    console.log('执行了b');
});
```
``` javascript
// main.js
define(function (require) {
    var mod;
    if (true) {
        mod = require('./a');
    }
    else {
        mod = require('./b');
    }
});
```

结果:
seajs的js加载情况：
![](/blog/AMD-CMD/seajs.jpg)
requirejs的js加载情况：
![](/blog/AMD-CMD/requirejs.jpg)
使用seajs输出结果如下: 
> 执行了a

使用requirejs输出结果如下: 
> 执行了a
> 执行了b

由此可知，seajs和requirejs都一样的分析依赖和加载依赖的js文件，但是requirejs加载完成js文件之后，会执行js模块的`factory`，而seajs是在真正用到该模块的时候才会去执行模块的`factory`。

接着来看下面第二个例子：
``` javascript
// main.js
define(function (require) {
    console.log('执行了main');
    require('./a').doSomething();
    require('./b').doSomething();
});
```

``` javascript
// a.js
define(function (require) {
    console.log('执行了a');
    return {
        doSomething: function () {
            console.log('a做了些事');
        }
    };
});
```

``` javascript
// b.js
define(function (require) {
    console.log('执行了b');
    return {
        doSomething: function () {
            console.log('b做了些事');
        }
    };
});
```
运行结果:
seajs输出如下: 
> 执行了main
> 执行了a
> a做了些事
> 执行了b
> b做了些事


requirejs输出结果如下: 
> 执行了a
> 执行了b
> 执行了main
> a做了些事
> b做了些事

同样的代码，依赖seajs和依赖requirejs却得到完全不同的两种输出。首先依赖seajs的运行结果可以看出，当执行main.js的factory时，main所依赖的a、b模块的factory才相继被执行，这就使得我们看到的输出结果是如我们写代码顺序的结果的。
而使用requirejs作为模块化加载工具时，首先是main依赖的a、b模块的factory先被执行了，然后才会去执行main的factory，这就是requirejs的依赖预解析，这样也确保了依赖都加载执行无误了，才会去执行主模块。至于这样两种执行模式，个人觉得各有千秋。

## 其他

### exports和return
module的return优先级更高，没有return值时，才用exports，两者同时出现，模块的返回值是return的值。

``` javascript
// a.js
define(function (require, exports, module) {
    // 引入a模块时，publicFn不会成为模块的输出方法
    exports.publicFn = function () {
        console.log('执行publicFn');
    }
    return {
        doSomething: function () {
            console.log('a做了些事');
        }
    };
});
```

### 循环依赖
// TODO

## 参考资料
- AMD规范（[https://github.com/amdjs/amdjs-api/blob/master/AMD.md](https://github.com/amdjs/amdjs-api/blob/master/AMD.md)）
- CMD 模块定义规范（[https://github.com/seajs/seajs/issues/242](https://github.com/seajs/seajs/issues/242)）
- Seajs与RequireJS 的异同（[https://github.com/seajs/seajs/issues/277](https://github.com/seajs/seajs/issues/277)）
- SeaJS与RequireJS最大的区别（[http://www.douban.com/note/283566440/](http://www.douban.com/note/283566440/)）
- LABjs、RequireJS、SeaJS 哪个最好用？为什么？（[http://www.zhihu.com/question/20342350](http://www.zhihu.com/question/20342350)）