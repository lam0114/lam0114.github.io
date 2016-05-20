---
title: JSFiddle Angular Uncaught Error
comments: true
date: 2016-05-18 17:23:33
tags: jsfiddle
---

今天在用JSFiddle练习Angular的时候，控制台报了一个错，整个程序也没法跑起来。

`Uncaught Error: [$injector:modulerr] Failed to instantiate module MyApp due to: Error: [$injector:nomod] Module 'MyApp' is not available!`

刚开始很纳闷，检查了几遍发现写的也没任何问题。况且写的是最基础的Hello World，难道我连最基础的都写不出来了？

后来Google了一下，发现不是写法的问题，而是JS的load type没有设置正确，导致了报错。

它一共有[4个选项][Frameworks and Extensions]，分别是：

* onLoad：默认值，将代码放到onLoad事件中运行
* onDomready：将代码放到onDomReady事件中运行
* No wrap - in head：直接放在head标签内部
* No wrap - in body：直接放在body标签内部

下面是不同的type在编译后的格式：

``` javascript

// type:onLoad

//<![CDATA[
window.onload=function(){
var n = 100;
}//]]>

// type:onDomready(非全部代码)

//<![CDATA[
var VanillaRunOnDomReady = function() {
  var n = 100;
}

document.addEventListener("DOMContentLoaded", function(){
    VanillaRunOnDomReady();
}, false)//]]>

// type: no wrap - in head/body

//<![CDATA[
var n = 100;
//]]>

```

由于onload事件是在所有资源全部加载完成后才会执行，而Angular是在DOM加载完成后就会去执行的，因此像下面的代码就会报错：

``` javascript
//<![CDATA[
window.onload=function(){
  angular.module('MyApp',[]);
}//]]>
```

同样的，放在DOMContentLoaded中也是不行的：

``` javascript
//<![CDATA[
document.addEventListener("DOMContentLoaded", function(){
    angular.module('MyApp',[]);
}, false)//]]>
```

之所以会这样，原因就是，在Angular中，也有一个用来监听DOMContentLoaded的事件，在这个事件中会执行一些初始化操作(`angularInit`)，它会去遍历寻找`ng-app`，并且会`启动(bootstrap)`一个angular应用。

正是在这个过程中，无论把创建module的代码放在onload或是自定义的DOMContentLoaded事件中，`angularInit`都无法找到MyApp这个module，因此就报了上面的错误。





[Frameworks and Extensions]:http://doc.jsfiddle.net/basic/introduction.html#frameworks-and-extensions
