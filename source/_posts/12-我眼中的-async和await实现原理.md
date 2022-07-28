---
title: 【我眼中的】 - 【12】async和await实现原理
date: 2022-07-27 21:42:12
categories:
- 我眼中的系列
tags:
- 我眼中的系列
---
Promise是一种异步编程的解决方案，实际上是利用编程技巧将回调函数的横向加载，改成纵向加载，达到链式调用的效果，避免回调地狱的问题。最大的问题是代码冗余，原来的任务被Promise包装一下，不管什么操作，一眼看去都是一堆的then，原来的语意变得不清楚。

为了解决 ```Promise``` 的问题，async和await在ES7中被提出，是关于异步的终极解决方案。
```javascript
const fs = require('fs')
async function readFile() {
  try {    
    var f1 = await readFileWithPromise('/etc/passwd')
    console.log(f1.toString())
    var f2 = await readFileWithPromise('/etc/profile')
    console.log(f2.toString())
  } catch (err) {
    console.log(err)
  }
}
```
async和await函数写起来和同步函数一样，条件是需要接收Promise或原始类型的值。异步编程的最终目标是转换成人类最容易理解的形式。

### async和await的原理
分析二者原理之前，需要一些前置知识：

#### 1. generator
generator函数是协程在ES6的实现。协程简单来说就是多个线程相互协作，完成异步任务。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220728114039.png)
整个generator函数就是一个封装的异步任务，异步操作需要暂停的地方，都用yield语句注明。generator函数的执行方法如下：
```javascript
function* gen(x){
  console.log('start');
  const y = yield x*2;
  return y;
}
const g = gen(1);
g.next() // start {value:2,done:false}
g.next(4) // {value:4,done:true}
```
