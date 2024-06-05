---
title: Vue的响应式原理
date: 2022-08-08 21:56:45
tags:
- VUE
---
响应式原理是Vue的核心特性之一。数据驱动视图，当我们修改数据的时候，视图随之而更新。

Vue2.x是借助```Object.defineProperty()```实现的，而Vue3.x是借助```proxy```实现的。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/Vue的响应式原理.jpg)

Data通过```Observer```转换成```getter/setter```的形式来追踪变化。
当外界通过```watcher```读取数据时，会触发```getter```，从而将```watcher```添加到依赖中。
当数据发生变化时会触发```setter```，从而向```Dep```中的依赖（```watcher```）发送通知。```watcher```接收到通知之后，会向外界发送通知，变化通知到外界可能会触发视图更新，也可能触发用户的某个回调函数。

> 3.x与2.x的核心思想是一致的，只不过是数据劫持使用```proxy```而不是```Object.defineProperty```，```proxy```相比```Object.defineProperty```在处理数组和新增删除属性响应式的处理上更加方便。
