---
title: 点击ul中的li，显示其索引
comments: true
date: 2016-07-07 11:21:57
tags: js
---

在点击ul中的li时，如何获取其索引？

最常见的方法就是遍历li。

<a class="jsbin-embed" href="http://jsbin.com/kewiro/embed?html,output">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?3.36.11"></script>

``` javascript
window.onload=function() {
var ulEle=document.getElementById("box");
var liEle=ulEle.getElementsByTagName("li");
  for (var i=0; i<liEle.length; i++) {
    liEle[i].onclick=(function(i) {
      return function() {
        alert("我是第"+(++i)+"个list");
  　　}
    }(i))
  }
}
```

在这种方法中，遍历所有的li元素，并且在for循环内部将对应的i的值传入到内部函数中，在点击时输出其索引值。

还有另外一种方法可以输出其索引值。

<a class="jsbin-embed" href="http://jsbin.com/giyipo/embed?html,js,output">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?3.36.11"></script>

``` javascript
Array.prototype.indexOf.call(obj.parentNode.children, obj)
```

在这个例子中，obj就是指当前点击的li。

[知乎]:https://www.zhihu.com/question/20322273
