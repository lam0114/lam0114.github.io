---
title: React Controlled Component
comments: true
date: 2016-04-07 18:10:29
tags: react
---

React的Form组件支持一些属性用于用户交互。

* 用于`<input>`和`<textarea>`的`value`属性
* 用于`checkbox`和`radio`的`checked`属性
* 用户`<option>`的`selected`属性

其中，input和select支持defaultValue用于设置默认值，checkbox和radio可以使用defaultChecked用来设置默认选中的选项。

同时，Form组件提供了`onChange`属性用于监听这些属性的变化。

*在React中，应该通过`value`属性为`<textarea>`设置值。*

### Controlled组件

假设有这样一个组件，定义了一个文本框。

``` javascript
render: function() {
  return (
    <input type="text" value="Hello!" />
  );
}
```

当你在页面中想要修改文本框的内容时，你会发现文本框的内容并没有什么变化。这是因为React已经将这个文本框的value设置为"Hello!"。之前提到过，React提供了`onChange`用于监听`value`的变化，但是在这个例子中，并没有提供onChange属性，也就是并没有任何handler用于处理value的变化，因此该组件的value一直为"Hello!"。

那么你可能会问，就算我没有提供onChange事件用来处理value的变化，但是我敲的那些字母符号为什么没有在input框中显示出来？

在React的[文档][Components are Just State Machines]中，介绍了React是将UI组件当作状态机器(State Machines)，通过将UI组件看成处在不同的状态并且去渲染这些状态的方式，对于保持组件一致性是非常方便的。在React中，你只需要更新组件的状态，并且根据这个新的状态去渲染一个新的UI，然后React会为你用最高效的方式去更新DOM。

不同于HTML，[React组件一定是代表着某个时间点的视图的状态，不仅仅是初始化的时候。][Why Controlled Components]

此时，再来看上面的例子，初始化的时候，React将组件的value设置为"Hello!"，此时该组件的state我们可以假设为stateA，当我们在输入内容时，并没有任何方法去修改state，也就是无论输入什么，该组件的state一直时stateA，因此，组件就一直是通过stateA渲染的，所以就不会有任何效果了。

那么我们怎么去响应用户的输入呢，只需要为该组件添加`onChange`属性绑定一个时间用来更新state。

```  javascript
getInitialState: function() {
  return {value: 'Hello!'};
},
handleChange: function(event) {
  this.setState({value: event.target.value});
},
render: function() {
  return (
    <input
      type="text"
      value={this.state.value}
      onChange={this.handleChange}
    />
  );
}
```

在这个例子中，handleChange会监听用户的输入，同时会即时的修改state的值，然后React会根据state的值去渲染新的UI，也就能即时的看到input框内容的变化了。

现在，就可以总结一下，什么是Controlled Component。该组件的value应该被React管理，而且需要设置该组件的value属性，同时该组件应该有一个handler用于处理onChange事件。

A controlled component does not maintain its own internal state; the component renders purely based on props.

### Uncontrolled组件

不带value属性（或者值为null）的input就是一个uncontolled组件，你的任何输入都会作用到该组件上，如果想监听value值的变化，你只需添加一个onChange属性。

```  javascript
render: function() {
  return <input type="text" />;
}
```

An uncontrolled component maintains its own internal state.

[Why Controlled Components]: http://facebook.github.io/react/docs/forms.html#why-controlled-components
[Components are Just State Machines]: http://facebook.github.io/react/docs/interactivity-and-dynamic-uis.html#components-are-just-state-machines
