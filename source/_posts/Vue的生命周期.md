---
title: Vue的生命周期
date: 2022-07-28 22:05:53
tags:
- Vue
---
### 1.生命周期是什么？
Vue的生命周期是指Vue实例从创建到销毁的过程，即指从创建、初始化数据、编译模板、挂载DOM、数据变化渲染更新到DOM上、销毁等一系列过程。

### 2.生命周期有哪些？
Vue的生命周期总共可以分为8个阶段：创建前后，载入前后，更新前后，销毁前后，以及一些特殊场景的生命周期。

|  生命周期   | 描述  |
|  ----  | ----  |
| beforeCreate  | 组件实例创建之初 |
| created  | 组件实例已经完全创建 |
| beforeMounted | 组件挂载之前 |
| mounted | 组件挂载到实例上去之后 |
| beforeUpdate | 组件数据发生变化，更新之前 |
| updated | 组件数据更新之后 |
| beforeDestory | 组件实例销毁之前 |
| destoryed| 组件实例销毁之后 |
| activated | keep-alive缓存的组件激活时 |
| deactivated| keep-alive缓存的组件停用时调用 |

在Vue生命周期钩子函数中会自动绑定this上下文到实例中，因此可以生命周期钩子函数中访问数据，运行方法。

### 3.生命周期的整体流程

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/Vue生命周期.jpg) 

**具体分析**

**beforeCeated → created**
+ 初始化Vue实例，进行数据观测

**created**
+ 完成数据观测，属性与方法的运算，```watch```、```event```事件回调的配置
+ 可调用```methods```中的方法，访问和修改```data```数据触发响应式渲染dom，可通过```computed```和```watch```完成数据计算
+ 此时```vm.$el```并没有被创建

**created → beforeMount**
+ 判断是否存在```el ```选项，若不存在则停止编译，直到调用```vm.$mount(el)```才会继续编译
+ 优先级：```render``` > ```template``` > ```outerHTML```
+ ```vm.el```获取到的是挂载```DOM```的

**mounted**
+ ```vm.el```已经完成```DOM```的挂载和渲染，此刻打印```vm.$el```，发现之前的挂载点及内容被替换成新的```DOM```

**beforeUpdate**
+ 更新的数据必须是被渲染在模板上的（```el```、```render``` 、 ```template``` 之一）
+ 此时```view```层还未更新
+ 若在```beforeUpdate```中再次修改数据，不会再次触发更新方法

**updated**
+ 完成```view```层的更新
+ 若在```updated```中再次修改数据，会再次触发更新方法（```beforeUpdate```、```updated```）

**beforeDestory**
+ 实例被销毁前调用，此时实例属性与方法仍可访问

**destroyed**
+ 完全销毁一个实例。可清理它与其他实例之间的连接，解绑它的全部指令及事件监听器
+ 并不能清除```DOM```，仅仅销毁实例

**使用场景**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220728235433.png)

### 题外话：数据请求在created还是mounted？
一般来说，在```created```和```mounted```中都可以发送数据请求。但是大部分时候会在```created```中发送请求。
```created```是在组件实例一旦创建完成立即调用，此时页面```dom```节点并未生成。
```mounted```是在页面```dom```节点渲染完毕之后立即执行，此时页面```dom```节点已经生成。

在触发时机上```created```比```mounted```更早，二者都能拿到实例对象的属性和方法。
放在```mounted```中请求数据有可能导致页面闪动（页面```dom```结构已经生成，页面```dom```会进行二次渲染），在```created```就不会出现此类情况。

**建议：放在```created```中去请求数据**
