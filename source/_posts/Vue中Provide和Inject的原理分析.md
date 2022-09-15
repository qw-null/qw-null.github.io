---
title: Provide和Inject的原理分析
date: 2022-09-15 09:09:32
tags:
- Vue
---

provide和inject主要用于开发高阶插件和组件时使用。

🌰 举个例子：
```javascript
// 父组件提供foo 子组件注入
//父 
var Provider = {
  provide:{
    foo:'bar'
  },
  //....
}
//子
var Child = {
  inject:['foo'],
  created(){
    console.log(this.foo);
  }
}
```
<mark>优点：</mark>
1. 祖先组件不需要知道哪些后代组件使用它提供的属性；
2. 后代组件不需要知道被注入的属性来自哪里；
<mark>缺点：</mark>
1. 数据来源不明确
2. 重名问题

### 源码分析：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220915091703.png)
组件实例初始化的时候会调用`Vue.prototype._init`，`vm._init`中在`data/props`前面调用了`initInjections`，在`data/props`后面调用了`initProvide`。
#### initInjections
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220915092011.png)
该方法主要做了以下两件事：
1. 获取vm.$options.inject，通过resolveInject方法找到对应的key集合；
2. 遍历key集合，对其进行响应式监听；

<mark>➡️ resolveInject</mark>
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220915092221.png)
该方法实现了通过$parent向上查找祖先节点数据：是通过遍历source.$parent逐级向上查找的，知道找到为止。

#### initProvide
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220915092332.png)
该方法单纯把组件注册的provide值，赋值给vm._provided，resolveInject中有使用到
