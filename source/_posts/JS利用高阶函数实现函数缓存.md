---
title: JS利用高阶函数实现函数缓存【备忘模式】
date: 2022-07-24 15:30:33
tags:
- 各种无厘头手写
---
### 题目描述

实现一个可以缓存结果的高阶函数，如果函数成功执行，缓存结果并返回
function cache (fn){
}

const fn = ()=>{
  console.log('执行了')
  return Math.random();
}

const cachedFn = cache(fn)
console.log(cachedFn()) // 0.2333
console.log(cachedFn()) // 0.2333

*(字节前端面试题)*

### 知识点1:高阶函数
高阶函数就是那种 **输入参数里有一个或多个函数** ，输出也是函数的函数。 

### 知识点2:高阶函数实现缓存(备忘模式)
好比有一个函数：
```javascript
var add = function(a){
  return a+1;
}
```
每次运行```add(1)```都会输出2，若是继续输入1，仍然会计算```1+1```，若是该操作是一个开销很大的操作，每次都重新进行计算会耗费大量的性能。如果能够 **对已经计算的结果进行缓存，当再次计算该数时直接取缓存的内容，只有当新的数字时才进行重新计算** ，这样就会极大降低性能消耗。

实现思路：
在备忘录函数内部创建一个对象cache来存储执行结果，若下次再输入同样的参数，就直接从cache中取出。只有传入新的参数时才会重新计算。

```javascript
function cache (fn) {
  let cache = {}
  return function (...args) {
    let _args = JSON.stringify(args);
    return cache[_args] || (cache[_args] = fn.apply(fn, args));
  }
}

const fn = () => {
  console.log('执行了')
  return Math.random()
}

const cachedFn = cache(fn)
console.log(cachedFn())
console.log(cachedFn())
```
执行结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220724230528.png)

可以发现fn函数实际上只执行了一次，第二次是直接从cache 中取值。

