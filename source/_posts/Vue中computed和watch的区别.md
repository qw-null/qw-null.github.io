---
title: Vue中computed和watch的区别
date: 2022-08-09 11:40:23
tags:
- 理解Vue
---
computed和watch都能实现对数据的监听，但是二者还是存在区别：
+ 功能上：```computed```是计算属性，```watch```用来监听一个值的变化
+ ```computed```有缓存，```data```中的数据不发生变化，不会重新计算；而```watch```每次监听的值发生变化都会执行回调
+ ```computed```默认首次加载就开始监听，```watch```默认第一次加载不进行监听（设置属性```immediate:true```会开启默认第一次加载就开始监听）
+ ```computed```属性值是一个函数，那么默认会走```get```方法，必须要有```return```返回值，```watch```不必有```return```返回值
+ ```watch```支持异步
+ **选择computed还是watch？** 官网说：<span style="background:yellow;">当需要数据变化时执行异步或者开销较大的操作时，watch方式是最有用的。所以watch支持异步</span>

### 总结：
watch和computed都是以Vue的依赖追踪机制为基础的，当某一个依赖型数据发生变化的时候，所有依赖这个数据的相关数据会自动发生变化，即自动调用相关的函数，来实现数据的变动。
>（依赖型数据：简单理解即放在 data 等对象下的实例数据）

**当依赖的值变化时，在watch中，是可以做一些复杂的操作的，而computed中的依赖，仅仅是一个值依赖于另一个值，是值上的依赖。**


