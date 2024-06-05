---
title: Vue中的$nextTick
date: 2022-08-11 12:02:01
tags:
- Vue
---
### 1.nextTick 是什么？
nextTick本质上就是执行延迟回调的钩子函数，接收一个回调函数作为参数，在下次DOM更新循环结束之后，执行延迟回调。
在修改数据之后立即调用这个方法，获取更新后的DOM。

### 2.nextTick 的作用是什么？
说到```nextTick```就要谈到```Vue```的异步更新，```Vue```在更新DOM时是异步更新的。
【 见以下第3部分-Vue的异步更新队列 】

> 例如当你设置```vm.someDate = 'new Vue' ```, 该组件不会立即重新渲染。当刷新队列时，组件在下一个事件'tick'中更新。
>但如果你想立即基于vm.someDate更新后的DOM状态来做点什么，这可能就会有些棘手。此时我们就可以先用nextTick(callbacks)在真正DOM渲染之前在数据变化之后立即对其操作。

<b style="background:red;">但实际情况是nextTick是DOM更新之后延迟回调</b>

nextTick 除了 **①让我们可以在DOM更新之后延迟回调** ，还有一个作用是 **② Vue内部使用nextTick，把渲染DOM这个操作放入callbacks**

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220811145832.png)

#### nextTick的源码浅析
Vue.nextTick用于延迟执行一段代码，它接受2个参数（回调函数和执行回调函数的上下文环境），如果没有提供回调函数，那么将返回promise对象。
源码：
```javascript
/**
 * Defer a task to execute it asynchronously.
 */
export const nextTick = (function () {
  const callbacks = [] // 用来存储所有需要执行的回调函数
  let pending = false // 用来标志是否正在执行回调函数
  let timerFunc // 用来触发执行回调函数

  function nextTickHandler () { // 这个函数用来执行callbacks里存储的所有回调函数
    pending = false
    const copies = callbacks.slice(0)
    callbacks.length = 0
    for (let i = 0; i < copies.length; i++) {
      copies[i]()
    }
  }

  // the nextTick behavior leverages the microtask queue, which can be accessed
  // via either native Promise.then or MutationObserver.
  // MutationObserver has wider support, however it is seriously bugged in
  // UIWebView in iOS >= 9.3.3 when triggered in touch event handlers. It
  // completely stops working after triggering a few times... so, if native
  // Promise is available, we will use it:
  /* istanbul ignore if */
  /*
  接下来是将触发方式赋值给timerFunc:
  先判断是否原生支持promise，如果支持，则利用promise来触发执行回调函数；
  否则，如果支持MutationObserver，则实例化一个观察者对象，观察文本节点发生变化时，触发执行所有回调函数。
  如果都不支持，则利用setTimeout设置延时为0。

  */
  if (typeof Promise !== 'undefined' && isNative(Promise)) {
    var p = Promise.resolve()
    var logError = err => { console.error(err) }
    timerFunc = () => {
      p.then(nextTickHandler).catch(logError)
      // in problematic UIWebViews, Promise.then doesn't completely break, but
      // it can get stuck in a weird state where callbacks are pushed into the
      // microtask queue but the queue isn't being flushed, until the browser
      // needs to do some other work, e.g. handle a timer. Therefore we can
      // "force" the microtask queue to be flushed by adding an empty timer.
      if (isIOS) setTimeout(noop)
    }
  } else if (!isIE && typeof MutationObserver !== 'undefined' && (
    isNative(MutationObserver) ||
    // PhantomJS and iOS 7.x
    MutationObserver.toString() === '[object MutationObserverConstructor]'
  )) {
    // use MutationObserver where native Promise is not available,
    // e.g. PhantomJS, iOS7, Android 4.4
    var counter = 1
    var observer = new MutationObserver(nextTickHandler)
    var textNode = document.createTextNode(String(counter))
    observer.observe(textNode, {
      characterData: true
    })
    timerFunc = () => {
      counter = (counter + 1) % 2
      textNode.data = String(counter)
    }
  } else {
    // fallback to setTimeout
    /* istanbul ignore next */
    timerFunc = () => {
      setTimeout(nextTickHandler, 0)
    }
  }
// 最后是queueNextTick函数。
// 因为nextTick是一个即时函数，所以queueNextTick函数是返回的函数，接受用户传入的参数，用来往callbacks里存入回调函数。
  return function queueNextTick (cb?: Function, ctx?: Object) {
    let _resolve
    callbacks.push(() => {
      if (cb) {
        try {
          cb.call(ctx)
        } catch (e) {
          handleError(e, ctx, 'nextTick')
        }
      } else if (_resolve) {
        _resolve(ctx)
      }
    })
    if (!pending) {
      pending = true
      timerFunc()
    }
    if (!cb && typeof Promise !== 'undefined') {
      return new Promise((resolve, reject) => {
        _resolve = resolve
      })
    }
  }
})()'

''
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220811235017.png)
nextTick的源码流程：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220811151252.png)

### 3.Vue的异步更新队列
vue在更新DOM时是异步执行的。<b style="background:yellow;"> 只要侦测到数据变化，vue将开启一个队列，并缓冲在同一个事件循环中所发生的所有数据变更。如果一个watcher被多次触发，只会被推入队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和DOM操作是非常重要的。
然后，在下一个的事件循环‘tick’，vue刷新队列并执行实际工作（已去重）。vue在内部对异步队列尝试使用原生的```Promise.then```、```MutationObserver```和```setImmediate```，如果执行环境不支持，则会采用```setTimeout(fn,0)```代替。</b>

### 4.nextTick的适用场景
①需要数据动态地为页面某些DOM元素添加事件，这就要求dom渲染完毕时去设置，但```created```与```mounted```函数执行时一般DOM并没有渲染完毕，因此需要使用```nextTick```函数
②使用第三方插件时，希望在vue生成的某些dom动态变化更新时应用该插件
```javascript
mounted(){
  this.$nextTick(()=>{
    this._initScroll();
  })
}
```
③数据改变之后立即获取焦点操作

