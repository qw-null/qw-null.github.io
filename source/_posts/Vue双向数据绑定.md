---
title: Vue双向数据绑定
date: 2022-04-11 23:02:14
categories:
- Vue学习
tags:
- Vue
---
视频链接：[【黑马程序员】vue双向数据绑定原理](https://www.bilibili.com/video/BV1Dr4y1c7xS?share_source=copy_web)

## 0.前置知识点 - 数组的```reduce()```方法

> 应用场景：下次操作的初始值，依赖于上次操作的返回值

1. 数值的累加操作
数组的```reduce```方法会循环当前数组，侧重于进行“滚雪球”的操作
```Array.reduce(函数, 初始值)```
```Array.reduce((上次计算的结果，当前循环的Item项) => { }, 初始值)```
```javascript
const arr = [3, 8, 9, 11, 2, 3, 4];

let res = arr.reduce((total, item) => {
  return total += item;
}, 0);
// 0是箭头函数的第一个参数的初始值
console.log(res); //40
```
2. 链式获取对象的属性的值

```javascript
const obj = {
  name: 'Jack',
  info: {
    address: {
      location: '北京'
    }
  }
}

const attrs = ['info', 'address', 'location'];
let res = attrs.reduce((newObj, key) => {
  return newObj[key]
}, obj);

console.log(res) // 北京
```
```javascript
const obj = {
  name: 'Jack',
  info: {
    address: {
      location: '北京'
    }
  }
}

const attrStr = 'info.address.location'
const res = attrStr.split('.').reduce((newObj, key) => newObj[key], obj)
console.log(res); // 北京
```
## 1. 发布订阅模式
+ ```Dep```类：负责进行<b>依赖收集</b>
首先，有个数组，专门存放所有的订阅信息；其次，还要提供一个向数组中追加订阅信息的方法；然后，还要提供一个循环，循环触发数组中的每一个订阅信息
+ ```Watcher```类：负责<b>订阅一些事件</b>

```javascript
// 收集依赖/收集订阅
class Dep {
  constructor() {
    // subs数组用来存放所有订阅者信息
    this.subs = []
  }

  // 向subs数组中添加订阅者的信息
  addSub (watcher) {
    this.subs.push(watcher)
  }

  // 发布订阅的方法
  notify () {
    this.subs.forEach((watcher) => watcher.update())

  }

}

//订阅者的类
class Watcher {
  constructor(cb) {
    this.cb = cb
  }

  // 触发回调的方法
  update () {
    this.cb()
  }

}

const w1 = new Watcher(() => {
  console.log('我是第一个订阅者');
})
const w2 = new Watcher(() => {
  console.log('我是第二个订阅者');
})
const dep = new Dep()

dep.addSub(w1)
dep.addSub(w2)

dep.notify()
```
#### Vue中发布订阅模式如何运作？
+ 只要我们为Vue中data数据重新赋值，这个复制动作会被Vue监听到
+ 然后Vue要把数据的变化，通知到每一个订阅者
+ 接下来，订阅者（DOM元素）要根据最新的数据，更新自己的内容 

## 2. 使用```Object.defineProperty()```进行数据劫持
Object.defineProperty()方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。
> 备注：应当直接在 Object 构造器对象上调用此方法，而不是在任意一个 Object 类型的实例上调用。

语法：```Object.defineProperty(obj, prop, descriptor)```
参数：
+ ```obj```：要定义属性的对象
+ ```prop```：要定义或修改的属性的名称或 ```Symbol``` 
+ ```descriptor```：要定义或修改的属性描述符

```javascript
const obj = {
  name: 'Jack',
  age: 20,
}

Object.defineProperty(obj, 'name', {
  enumerable: true,// 当前属性允许被循环
  configurable: true, // 当前属性允许被配置
  get () { // getter
    console.log('获取了obj.name');
    return 'Tom'
  },
  set (newVal) { // setter
    console.log('赋值了');
  }
})

console.log(obj.name);// 获取了obj.name   Tom

obj.name = 'Sam'; //赋值了
```