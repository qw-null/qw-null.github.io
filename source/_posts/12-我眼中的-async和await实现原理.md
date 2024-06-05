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
g.next(3) // {value:3,done:true}
```
+ ```gen()```不会立即执行，而是一上来就暂停，返回一个```Iterator```对象
+ 每次```g.next()```都会打破暂停状态去执行，直到遇到下一个```yield```或者```return```
+ 遇到```yield```时，会执行```yield```后面的表达式，并返回执行之后的值，然后再次进入暂停状态，此时```done:false```。
+ ```next```函数可以接受参数，作为上个阶段异步任务的返回结果，被函数体内的变量接收
+ 遇到```return```时，会返回值，执行结束，即```done:true```
+ 每次```g.next()```的返回值永远都是```{value:... , done:...}```的形式

#### 2. thunk函数
JS中的thunk函数（译为转换程序），简单来说就是把带有回调函数的多参数函数转换成只接收回调函数的单参数版本
```javascript
const fs = require('fs');
const thunkify = fn=>(...res)=>callback=>fn(...res,callback);
const thunk = thunkify(fs.readFile);
const readFileThunk = thunk('/etc/passwd','utf-8');
readFileThunk((err,data)=>{
  // ...
})
```
单纯的thunk函数并没有很大的用处，大牛们想到将其与generator结合：
```javascript
function* readFileThunkWithGen(){
  try{
    const content1 = yield readFileThunk('/etc/passwd','utf-8');
    console.log(content1);
    const content2 = yield readFileThunk('/etc/profile','utf-8');
    console.log(content2);
    return 'done';
  } catch (error) {
    console.log(error);
    return 'fail';
  }
}

const g = readFileThunkWithGen();
g.next().value((err,data)=>P{
  if(err){
    return g.throw(err).value
  }
  g.next(data.toString()).value((err,data)=>{
    if(err){
      return g.throw(err).value;
    }
    g.next(data.toString())
  })
})
```
thunk函数的真正作用是统一多参数函数的调用方式，在next调用时把控制权交还给generator，使generator函数可以使用低柜方式自启动流程
```javascript
const run = generator => {
  const g = generator()
  const next = (err, ...rest) => {
    if (err) {
      return g.throw(err).value
    }
    const result = g.next(rest.length > 1 ? rest : rest[0])
    if (result.done) {
      return result.value
    }
    result.value(next)
  }
  next()
}
run(readFileThunkWithGen)
```
有了自启动的加持，generator函数内就可以写“同步”的代码了。

generator 可以暂停执行，很容易让它和异步操作产生联系，因为我们在处理异步操作时，在等待的时候可以暂停当前任务，把程序控制权交还给其他程序，当异步任务有返回时，在回调中再把控制权交还给之前的任务。generator 实际上并没有改变 JavaScript 单线程、使用回调处理异步任务的本质。

#### 3.co函数库
每次执行generator函数时自己写启动器比较麻烦。co函数库是一个generator函数的自启动执行器，使用条件是generator函数的yield命令后面只能说thunk函数或者Promise对象，co函数执行完饭回一个Promise对象。
```javascript
const co = require('co')
co(readFileWithGen).then(res => console.log(res)) // 'done'
co(readFileThunkWithGen).then(res => console.log(res)) // 'done'
```
co函数库的源码实现，其实就是将上面两种情况进行综合：
```javascript
// 做了简化，与源码基本一致
const co = (generator, ...rest) => {
  const ctx = this
  return new Promise((resolve, reject) => {
    const gen = generator.call(ctx, ...rest)
    if (!gen || typeof gen.next !== 'function') {
      return resolve(gen)
    } 

    const onFulfilled = res => {
      let ret
      try {
        ret = gen.next(res)
      } catch (e) {
        return reject(e)
      }
      next(ret)
    }    

    const onRejected = err => {
      let ret
      try {
        ret = gen.throw(err)
      } catch (e) {
        return reject(e)
      }
      next(ret)
    }

    const next = result => {
      if (result.done) {
        return resolve(result.value)
      }
      toPromise(result.value).then(onFulfilled, onRejected)
    }

    onFulfilled()
  })  
}

const toPromise = value => {
  if (isPromise(value)) return value
  if ('function' == typeof value) {
    return new Promise((resolve, reject) => {
      value((err, ...rest) => {
        if (err) {
          return reject(err)
        }
        resolve(rest.length > 1 ? rest : rest[0])
      })
    })
  }
}
```

async、await是co库的官方实现。也可以看作自带启动器的generator函数的语法糖。不同的是，async、await只支持Promise和原始类型的值，不支持thunk函数。

**不管回调函数、Promise、generator或者async/await哪种方式，都没有改变JS单线程、使用回调处理异步任务的本质。**


