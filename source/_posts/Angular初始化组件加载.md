---
title: Angular初始化组件加载
comments: true
date: 2016-05-25 17:37:11
tags: angular
---

在之前的文章中，提到过在使用JSFiddle的过程中遇到过一个报错，后来Google了之后也解决了。是因为JSFiddle的load type没有设置正确，只能选择`No wrap - on head/body`，但是不明白为什么选择onDOMReady不可以，于是乎在SO上提了个[问题][StackOverflow]，两个答案也很好的解释了不可以这样做的原因。

根据这两个答案，也看了看源码，总算是明白了angular初始化的过程。

在Angular中，它自己注册了一个DOMReady事件：
``` javascript
jqLite(window.document).ready(function() {
  angularInit(window.document, bootstrap);
});
```

[jqLite]其实就是jQuery的一个子集，它仅仅实现了一些最常用的函数。上面的函数等同于：
``` javascript
$(document).ready(function() {
  angularInit(window.document, bootstrap);
})
```

上面的代码中，注册了一个事件，在DOM加载完成后，执行angularInit方法，下面是angularInit方法部分的代码：

``` javascript
var appElement,
    module,
    config = {};
forEach(ngAttrPrefixes, function(prefix) {
  var name = prefix + 'app';
  var candidate;

  if (!appElement && (candidate = element.querySelector('[' + name.replace(':', '\\:') + ']'))) {
    appElement = candidate;
    module = candidate.getAttribute(name);
  }
});
if (appElement) {
  config.strictDi = getNgAttribute(appElement, "strict-di") !== null;
  bootstrap(appElement, module ? [module] : [], config);
}
```

在这个方法中，会遍历`ngAttrPrefixes`，它是一个数组，包含了`ng-、data-ng－、ng:、x-ng-`，然后通过拼接字符串app去获取appElement并且得到这个module。

比如在html标签上加了`ng-app='MyApp'`这个属性，那么Angular在执行的时候会通过element.querySelector获取到html元素，并且得到了MyApp这个module，最后通过bootstrap方法启动整个应用程序。

`bootstrap`方法是用来启动一个Angular程序，一般很少会用到，除非你想自己控制初始化的过程。

根据[官网的描述][Angular-bootstrap]，`bootstrap`做了以下事情：
1. 加载相关的module
2. 创建injector
3. 将`ngApp`做为根节点编译DOM

下面看下`bootstrap`的代码，看看它到底做了哪些事情。
``` javascript
var injector = createInjector(modules, config.strictDi);
injector.invoke(['$rootScope', '$rootElement', '$compile', '$injector',
   function bootstrapApply(scope, element, compile, injector) {
    scope.$apply(function() {
      element.data('$injector', injector);
      compile(element)(scope);
    });
  }]
);
return injector;
```

第一行就是加载相关的module及创建injector，在`createInjector`中，有一个方法`loadModules(modulesToLoad)`用来加载指定的module，参数就是要被加载的module。

``` javascript
// loadModules()，在该方法内部，会判断当前参数是否是符合规范的Angular module。
if (isString(module)) {
  moduleFn = angularModule(module);
  /*
  ...
   */
}

// 将setupModuleLoader的返回值赋值给angularModule
// 相当于angularModule = function module(name, requires, configFn){ /* code */ }
angularModule = setupModuleLoader(window);

function setupModuleLoader(window) {
  function ensure(obj, name, factory) {
    return obj[name] || (obj[name] = factory());
  }

  return ensure(angular, 'module', function() {
    // angularModule()方法内部实现方式
    return function module(name, requires, configFn) {
      return ensure(modules, name, function() {
        if (!requires) {
          throw $injectorMinErr('nomod', "Module '{0}' is not available! You either misspelled " +
             "the module name or forgot to load it. If registering a module ensure that you " +
             "specify the dependencies as the second argument.", name);
        }
      }
    }
  }
}
```

在`bootstrap`方法中，`compile(element)(scope)`则执行了编译操作。

现在就要回到对开头疑惑的解释了，假设也注册了一个事件E用来监听DOM是否加载完成。

``` javascript
document.addEventListener('DOMContentLoaded',function() {
  angular.module('MyApp',[]);
}, false);
```

由于Angular也已经存在了一个ready事件，由于文件的加载顺序，angularInit()事件会先于自己注册的E事件执行，因此会通过`ng-app`找到需要被加载的module`MyApp`，然后使用angularModule方法进行验证，由于requires(如果指定，则创建新的module，否则去加载指定的module)为`undefined`，也就是需要加载`MyApp`这个module，但此时它还不存在，因此就报错了。

这也就解释了为什么不能将创建module的代码放在`DOMContentLoaded`，同样的，也不能放在`onLoad`事件中。

因此只能采用下面的形式去创建module。

``` html
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular.min.js"></script>
<script>
  var app = angular.module('MyApp',[]);
</script>
```

在这种情况下，在执行angularInit方法时，app已经创建完成了。





[StackOverflow]:http://stackoverflow.com/q/37318817/2674213
[jqLite]:https://docs.angularjs.org/api/ng/function/angular.element
[Angular-bootstrap]:https://docs.angularjs.org/guide/bootstrap
