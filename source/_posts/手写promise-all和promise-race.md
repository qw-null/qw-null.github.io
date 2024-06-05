---
title: 手写promise.all和promise.race
date: 2022-08-11 20:11:25
tags: 
- 各种无厘头手写
---

## 1.promise.all

来MDN的介绍：
> Promise.all() 方法接收一个 promise 的 iterable 类型（注：Array，Map，Set 都属于 ES6 的 iterable 类型）的输入，并且只返回一个Promise实例， 那个输入的所有 promise 的 resolve 回调的结果是一个数组。这个Promise的 resolve 回调执行是在所有输入的 promise 的 resolve 回调都结束，或者输入的 iterable 里没有 promise 了的时候。它的 reject 回调执行是，只要任何一个输入的 promise 的 reject 回调执行或者输入不合法的 promise 就会立即抛出错误，并且 reject 的是第一个抛出的错误信息。

1. Promise.all() 方法接收一个promise的iterable类型（注：Array，Map，Set都属于ES6的iterable类型）的输入。 ➡️  <span style="background:yellow;">说明所传参数都具有Iterable,也就是可遍历。</span>
2. 并且只返回一个Promise实例。➡️ <span style="background:yellow;"> 说明最终返回是一个Promise对象。</span>
3. 那个输入的所有promise的resolve回调的结果是一个数组。➡️ <span style="background:yellow;"> 说明最终返回的结果是个数组，且数组内数据要与传参数据对应。</span>
4. 这个Promise的resolve回调执行是在所有输入的promise的resolve回调都结束，或者输入的iterable里没有promise了的时候。➡️ <span style="background:yellow;"> 说明最终返回时，要包含所有的结果的返回。</span>
5. 它的reject回调执行是，只要任何一个输入的promise的reject回调执行或者输入不合法的promise就会立即抛出错误，并且reject的是第一个抛出的错误信息。➡️ <span style="background:yellow;">  说明只要一个报错，立马调用reject返回错误信息。</span> 

```javascript
function promiseAll(iterator) {
  const promises = Array.from(iterator);
  const len = promises.length;
  let index = 0; //每次执行成功+1，当等于长度时，说明所有数据都返回，可以resolve()
  let data = [];//用来存放返回的数据数组
  return new Promise((resolve,reject)=>{
    promises[i].then((res)=>{
      data[i]=res;
      if(++index === len){
        resolve(data);
      }
    }).catch((err)=>{
      reject(err);
    })
  })
}
```

## 2.promise.race
```javascript
function promiseRace(promises) {
  return new Promise((resolve, reject) => {
    for (let i = 0; i < promises.length; i++) {
      promises[i].then(v => {
        // 修改返回对象的状态为【成功】
        resolve(v);
      }, r => {
        // 修改返回对象的状态为【失败】
        reject(r);
      })
    }
  })
}
```

## 3.Promise.prototype.finally
`finally()`方法用于指定不管 `Promise` 对象最后状态如何，都会执行的操作(无论`promise`被`reslove`或者`reject`，都会执行到`finally`里面去)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220901115757.png)
`finally`方法的回调函数不接受任何参数，这意味着没有办法知道，前面的 `Promise`状态到底是`fulfilled`还是`rejected`。这表明，`finally`方法里面的操作，应该是与状态无关的，不依赖于 `Promise` 的执行结果。



```javascript
Promise.prototype.finally = (callback) => {
    return this.then(
        (value) => {
            return Promise.resolve(callback()).then(() => value)
        },
        (error) => {
            return Promise.reject(callback()).then(() => { throw error })
        }
    )
}
```
