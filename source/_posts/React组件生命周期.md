---
title: React组件生命周期
comments: true
date: 2016-04-06 17:31:10
tags: react
---

React 组件的生命周期

* Mounting: A component is being inserted into the DOM
  * getInitialState()：组件状态变为mounted之前执行。有状态组件应该在此初始化state对象。
  * componentWillMount()：在mounting发生之前执行。
  * componentDidMount()：在mounting发生之后执行。Initialization that requires DOM nodes should go here.

* Updating: A component is being re-rendered to determine if the DOM should be updated
  * componentWillReceiveProps(object nextProps)：当一个mounted组件接收到新的props时执行。比较this.props及nextProps并使用this.setState()更新state。
  * shouldComponentUpdate(object nextProps, object nextState)：当组件判定任何变化是否值得更新到DOM上，return false使React忽略更新。
  * componentWillUpdate(object nextProps, object nextState)：在更新之前执行。在这里不能调用this.setState()
  * componentDidUpdate(object prevProps, object prevState)：在更新之后执行。

* Unmouting: A component is being removed from the DOM
  * componentWillUnmount()：组件状态变为unmounted及被销毁之前执行

对于任何mounted的组件，如果它的状态的一些深层次方面发生了变化而非使用`this.setState()`，那么就可以使用`component.forceUpdate()`去更新该组件。
