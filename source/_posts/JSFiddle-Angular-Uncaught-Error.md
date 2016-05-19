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





[Frameworks and Extensions]:http://doc.jsfiddle.net/basic/introduction.html#frameworks-and-extensions
