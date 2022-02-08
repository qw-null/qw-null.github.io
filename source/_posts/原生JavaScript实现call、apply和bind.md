---
title: 原生JavaScript实现call、apply和bind
date: 2022-02-01 13:13:23
categories:
- JavaScript
tags:
- JavaScript
---
## 1. call方法
#### 1.1 call方法详解：
+ call()可以用来代替另一个对象，来调用它的方法
+ call()本质是<b style="background:#ffffcc">改变this的指向</b>
+ call()真正强大的在于能够扩充目标函数的作用域
+ 所谓的call的多重继承就是把自己的this指向多个目标函数

```javascript
// call方法
var p = {
  n: '你好',
  s: '2022年'
}

var a = {
  fn: function () {
    console.log(this.n + this.s)
  }
}

a.fn.call(p) // 你好2022年

// call方法带参数
var b = {
  fn: function (x) {
    console.log(this.n + this.s + x)
  }
}

b.fn.call(p, '加油向未来')// 你好2022年加油向未来
```
call的多重继承

```javascript
function fn1 () { //---->父类
  this.say = function (n) {
    console.log(n);
  }
}

function fn2 () {//---->子类
  fn1.call(this);
  fn3.call(this);
}

function fn3 () { //---->父类
  this.fn3xx = function () {
    console.log('call简单实现多重继承');

  }
}

var _f = new fn2();
_f.say('你好，未来'); //你好，未来
_f.fn3xx(); //call简单实现多重继承
```

#### 1.2 模拟实现call


