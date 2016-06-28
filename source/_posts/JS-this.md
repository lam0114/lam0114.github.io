---
title: JS this
comments: true
date: 2016-06-28 09:56:31
tags: JS
---

在Java中，this指向的是当前对象，比如在Bar类中有一个foo方法，那么在foo方法中输出this时，它的值一定是Bar类的实例。

但是在JS中，由于函数作用域是在方法调用时才定义的，因此this的值只有在调用时才能确定，也就是一般情况下谁调用这个方法，那this就指向谁。

**函数上下文是当该函数被调用时定义的，而非定义函数时就去定义上下文。**

JS的方法有如下几种调用方式：

1. 函数式调用
2. 方法式调用
3. 构造方法式调用
4. apply、call、bind

### 一. 函数式调用

JS中使用function定义一个函数，最常见的就是下面这种：

``` javascript
function printThis() {
  console.log(this);
}
printThis();
```

在这个例子中，定义了一个函数printSth并调用它，该函数中的this指向的就是一个全局变量（根据执行环境的不同而不同），在浏览器中就指向`window`。

再看下面这个例子：

``` javascript
var obj = {
  printThis: function() {
    console.log(this);
  }
}
var func = obj.printThis;
func();
```

在这个例子中，this仍然指向的是`window`（浏览器中），可能会有人说this应该是obj，其实不对，，在这个例子中，func在被调用时所处的上下文其实就是window，那么this的值就指向的是window了。

### 二. 方法式调用

``` javascript
var obj = {
  printThis: function() {
    console.log(this);  // this === obj 输出：true
  }
}
obj.printThis();
```

在这个例子中，this又指向什么呢？

它指向的正是obj对象。printThis是被obj调用的，因此this指向了obj。

这里还有另外一个例子：

``` javascript
var name = 'window';

var obj = {
  name: 'object',
  printName: function() {
    console.log(this.name);
  }
}

obj.printName();  // 输出：object，指向obj

var func = obj.printName;
func();           // 输出：window，指向window
```

对于方法式调用，它的上下文会被设置到函数被调用的那个对象上。

对于上面这个例子，printName()的上下文会被设置为obj，而func()的上下文会被设置为window，因此分别找到对应的name值，结果就显示为object和widnow。

**对于嵌套的函数，它的this指向的是最右边的那个元素**，还是相当于找到它的调用者。

比如`foo.bar.func()`，那么在func中的this指向的就是bar。

### 三. 构造方法式调用

构造方法通常以大写字母开头，使用构造方法创建对象时使用`new`关键字。

例如：

``` javascript
function Foo() {
  this.printThis = function() {
    console.log(this);
  }
}
var foo = new Foo();
foo.printThis();    // 输出：Foo {}
```

> 以这种方式调用构造函数经历了以下4个步骤：
> 1.创建一个新对象
> 2.将构造函数的作用域赋给新对象（因此this就指向了这个对象）
> 3.执行构造函数中的代码（为这个新对象添加属性）
> 4.返回新对象

> 构造函数通常不返回return关键字，通常初始化新对象，当构造函数的函数体执行完毕时，会显式返回。在这种情况下，构造函数调用表达式的计算结果就是这个新对象的值；如果构造函数显式地使用return语句返回一个对象，那么调用表达式的值就是该对象；如果构造函数使用return语句但没有指定返回值，或者返回一个原始值，那么将忽略返回值。

### 四. apply、call调用

两个方法的作用是相同的，它们的第一个参数相同，是为函数提供的上下文。都可以接收多个参数，除第一个参数外，

对于call方法：接收一个参数列表用以传入函数
对于apply方法：接收一个参数数组用以传入函数


即，

``` javascript
func.call(context, args0, args1, args2...);
func.apply(context, [args0, args1, args2...]);
```

对于上面的这个例子

``` javascript
var name = 'window';

var obj = {
  name: 'object',
  printName: function() {
    console.log(this.name);
  }
}

obj.printName();  // 输出：object，指向obj

var func = obj.printName;
// func();           // 输出：window，指向window
func.call(obj);     // 与使用func.apply(obj)结果相同，输出：object
```

再看下面这个例子

``` javascript
function add(a, b) {
  return this.x += a + b;
}
var obj = {x: 0};
add.call(obj, 1, 2);    // 与add.apply(obj, [1, 2])结果相同，均为3
```

**如果是非严格模式且第一个参数为null或者undefined，那么会被替换为全局对象，浏览器中是window，其它环境则是global。如果是严格模式，则输出null或undefined。**

``` javascript
function fun() { return this; }
fun.call(null);
fun.apply(null);
var f = fun.bind(null);
f();
// 输出Window
"use strict";
function fun() { return this; }
fun() === undefined
fun.call(2) === 2
fun.apply(null) === null
fun.call(undefined) === undefined
fun.bind(true)() === true
// 结果均为true
```

还有一个方法，**`bind()`**，该方法也可以修改函数的上下文，不过一旦修改，无论如何调用该函数，它的上下文都是相同的，即this指向的是同样的值。

``` javascript
var obj1 = {
  x: 1
};
var obj2 = {
  x: 2
};
function show() {
  console.log(this.x);
}
var func = show.bind(obj1);
func();     // 输出：1
func.call(obj2);     // 输出：1
func.call(window);   // 输出：1
func.bind(obj2)();   // 输出：1
```

在这个例子中，可以看到，一旦将obj1作为上下文绑定到show函数上（将绑定后的函数赋值给func），无论之后如何修改func函数的上下文，它输出的永远是1，也就是它的上下文没有任何修改，一直是obj1。

---

### 总结

1. **函数上下文是当该函数被调用时定义的。**
2. **如果是非严格模式且第一个参数为null或者undefined，那么会被替换为全局对象，浏览器中是window，其它环境则是global。如果是严格模式，则输出null或undefined。**
