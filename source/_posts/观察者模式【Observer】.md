---
title: 观察者模式【Observer】
date: 2022-04-14 10:18:02
categories:
- 设计模式
tags:
- 设计模式
---

# 1. 观察者模式是什么？
观察者模式，通常又被称为<b>发布订阅者模式</b>或<b>消息机制</b>。
它定义了对象间的一种一对多的依赖关系，只要当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并自动更新，解决了主体对象与观察者之间功能的耦合，即一个对象改变给其他对象通知的问题。
# 2.观察者模式中二者分工
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220414103356.png)
# 3. 代码实现
```javascript
let observer_id = 0, observed_id = 0;
/*
观察者类 -- 订阅者
实现功能：构造函数、更新自身的函数
*/
class Observer {
  constructor() {
    this.id = observer_id++;
  }

  update (ob) {
    console.log('观察者', this.id, '检测到被观察者', ob.id, '的变化');
  }
}

/*
被观察者类 -- 发布者
实现功能：构造函数、添加观察者方法、删除观察者方法、通知每一个观察者的方法
*/
class Observed {
  constructor() {
    this.observerList = [];
    this.id = observed_id;
  }
  // 添加观察者
  addObserver (ob) {
    this.observerList.push(ob);
  }
  // 删除观察者
  removeObserver (ob) {
    this.observerList = this.observerList.filter(o => {
      return o.id != ob.id;
    })
  }
  // 通知所有观察者
  notify () {
    this.observerList.forEach(observer => {
      observer.update(this)
    })
  }
}

let obs_d = new Observed();
let obs_1 = new Observer(), obs_2 = new Observer();

obs_d.addObserver(obs_1);
obs_d.addObserver(obs_2);

obs_d.notify();
console.log('-----------------')
obs_d.removeObserver(obs_1);
obs_d.notify();
```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220414103913.png)