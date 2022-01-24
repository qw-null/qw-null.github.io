---
title: Promise学习
date: 2022-01-18 17:13:57
categories:
- Vue学习
tags:
- Promise
---
[尚硅谷Web前端Promise教程从入门到精通（2021抢先版）](https://www.bilibili.com/video/BV1GA411x7z1?share_source=copy_web)

## 1.Promise介绍与基本使用
### 1.1 理解
1.抽象表达
+ Promise是一门新的技术（ES6规范）
+ <span style="background-color:yellow;">Promise是JS中进行异步编程的新解决方案</span>
备注：旧方案是单纯使用回调函数

2.具体表达
+ 从语法上来说：Promise是一个构造函数
+ 从功能上来说：Promise对象用来封装一个异步操作并且可以获取其成功/失败的结果值

异步编程：(使用回调函数方式)
* fs文件操作
```
  require('fs').readFile('./index.html',(err,data)=>{})
```
* 数据库操作
* AJAX
```
  $.get('/server',(data)=>{})
```
* 定时器
```
  setTimeout(()=>{},2000);
```

### 1.2 为什么要用Promise？
#### 1.2.1指定回调函数方式更加灵活
1. 旧的：必须在启动异步任务前指定
2. promise:启动异步任务=>返回promise对象=>给promise对象绑定回调函数（甚至可以在异步任务结束后指定/多个）
#### 1.2.2支持链式调用，可以解决回调地狱问题
<b>1.什么是回调地狱?</b>
回调函数嵌套调用，外部回调函数异步执行的结果是嵌套的回调执行的条件

![回调地狱常见情形](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210703102534.png)

<b>2.回调地狱的缺点？</b>
不便于阅读
不便于异常处理
<b>3.解决方案？</b>
promise链式调用

##### Case 1:Promise初体验

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Promise初体验</title>
</head>

<body>
  <div class="container">
    <h2 class="page-header">Promise初体验</h2>
    <button class="btn btn-primary" id="btn">点击抽奖</button></button>
  </div>

</body>

<script>
  //生成随机数
  function rand(m, n) {
    return Math.ceil(Math.random() * (n - m + 1)) + m - 1;
  }

  // 点击按钮，2s后显示是否中奖（30%概率中奖）
  // 若中奖弹出  恭喜恭喜，奖品为10RMB劳斯莱斯优惠券
  // 若未中奖弹出 再接再厉

  //获取元素对象
  const btn = document.querySelector('#btn');
  //绑定单击事件
  btn.addEventListener('click', function () {
    //1. 常规写法
    // setTimeout(() => {
    //   //30%  1-100
    //   //获取从1-100的一个随机数
    //   let n = rand(1, 100);
    //   //判断
    //   if (n <= 30) {
    //     alert('恭喜恭喜，奖品为10RMB劳斯莱斯优惠券')
    //   } else {
    //     alert('再接再厉')
    //   }
    // }, 1000);

    //2.promise 形式实现
    // resolve 解决 函数类型的数据
    // reject 拒绝 函数类型的数据
    const p = new Promise((resolve, reject) => {
      setTimeout(() => {
        //30%  1-100
        //获取从1-100的一个随机数
        let n = rand(1, 100);
        //判断
        if (n <= 30) {
          //成功
          resolve(n); //将promise对象的状态设置为【成功】
        } else {
          //失败
          reject(n); //将promise对象的状态设置为【失败】
        }
      }, 1000);

    });
    //调用 then 方法
    p.then((value) => {
      //成功时的回调函数
      alert('恭喜恭喜，奖品为10RMB劳斯莱斯优惠券,你的中奖号码为' + value)

    }, (reason) => {
      //失败时的回调函数
      alert('再接再厉，你的号码为' + reason)

    })

  })

</script>

</html>
```

##### Case 2:Promise实践练习-fs模块

```js
  //引入模块
const fs = require('fs')

//1.回调函数形式
// fs.readFile('./content.txt', (err, data) => {
//   //如果出错，则抛出错误
//   if (err) throw err;
//   // 如果正确，则输出文件内容
//   console.log(data.toString());
  
// })

//2.Promise形式
let p = new Promise((resolve, reject) => {
  fs.readFile('content.txt', (err, data) => {
    //如果出错
    if (err) reject(err);
    //如果成功
    resolve(data);
  })
  
});

//调用then方法
p.then(value => {
  console.log(value.toString());
}, reason => {
  console.log(reason);
  
});

```

##### Case 3:Promise实践练习-AJAX请求

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Promise 封装 AJAX</title>

</head>

<body>
  <div class="container">
    <h2 class="page-header">Promise 封装 AJAX操作</h2>
    <button class="btn btn-primary" id="btn">点击发送AJAX</button>
  </div>
  <script src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js"></script>
</body>

<script>
  //接口地址：https://api.apiopen.top/getJoke
  //获取元素对象
  const btn = document.getElementById('btn')
  console.log('btn信息', btn);

  btn.addEventListener('click', function () {
    //创建Promise
    const p = new Promise((resolve, reject) => {
      //1.创建对象
      const xhr = new XMLHttpRequest();
      //2.初始化
      xhr.open('GET', 'https://api.apiopen.top/getJoke');
      //3.发送
      xhr.send();
      //4.处理响应结果
      xhr.onreadystatechange = function () {
        if (xhr.readyState === 4) {
          //判断响应状态码2xx
          if (xhr.status >= 200 && xhr.status < 300) {
            //控制台输出响应体
            resolve(xhr.response);
          } else {
            //控制台输出响应状态码
            reject(xhr.status);
          }
        }
      }

    });
    //调用then方法
    p.then(value => {
      console.log(value);
    }, reason => {
      console.warn(reason);

    })
  });
</script>

</html>
```

##### Case 4:Promise封装练习-fs模块

```javascript
// 封装一个函数 mineReadFile 读取文件内容
// 参数：path 文件路径
// 返回：promise 对象

function mineReadFile(path) {
  return new Promise((resolve, reject) => {
    require('fs').readFile(path, (err, data) => {
      //判断
      if (err) reject(err);
      //成功
      resolve(data);
    })
  })
}

mineReadFile('content.txt').then(value => {
  //输出内容
  console.log(value.toString());
}, reason => {
  console.log(reason);
})

```

##### Case 5:util.promisify方法

```javascript
// util.promisify 方法

//引入util模块
const util = require('util');
//引入fs模块
const fs = require('fs');
//返回一个新的函数
let mineReadFile = util.promisify(fs.readFile);

mineReadFile('content.txt').then(value => {
  console.log(value.toString());
}, reason => {
  console.log(reason);
});
```
### 1.3 Promise的状态

  实例对象中的一个属性 [PromiseState]
  *pending 未决定的
  *resolved / fullfiled 成功
  *rejected 失败
   
#####  1.3.1 Promise的状态的改变
1. pending 变为 resolved
2. pending 变为 rejected
说明：只有这两种状态改变的方式，且一个promise对象只能改变一次
      无论变为成功还是失败，都会有一个结果数据
      成功的结果数据一般称为value，失败的结果数据一般称为reason

### 1.4 Promise的值
实例对象中的另一个属性 [RromiseResult]
保存着异步任务 [成功/失败] 的结果
* resolve函数
* reject函数

### 1.5 Promise的基本流程
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210709104619.png)
  

## 2.Promise API
#### 2.1-Promise 构造函数: Promise (excutor) {}
> (1) executor 函数: 执行器 (resolve, reject) => {}
> (2) resolve 函数: 内部定义成功时我们调用的函数 value => {}
> (3) reject 函数: 内部定义失败时我们调用的函数 reason => {}
> 说明: executor 会在 Promise 内部立即同步调用，异步操作在执行器中执行，换话说Promise支持同步也支持异步操作

#### 2.2-Promise.prototype.then 方法: (onResolved, onRejected) => {}
> (1) onResolved 函数: 成功的回调函数 (value) => {}
> (2) onRejected 函数: 失败的回调函数 (reason) => {}
> 说明: 指定用于得到成功 value 的成功回调和用于得到失败 reason 的失败回调 返回一个新的 promise 对象

#### 2.3-Promise.prototype.catch 方法: (onRejected) => {}
> (1) onRejected 函数: 失败的回调函数 (reason) => {}
> 说明: then()的语法糖, 相当于: then(undefined, onRejected)

#### 2.4-Promise.resolve 方法: (value) => {}
> （1）value: 成功的数据或 promise 对象
> 说明：返回一个成功/失败的promise对象，直接改变promise状态

```javascript
    let p1 = Promise.resolve(521);
    // * 如果传入的参数为非Promise类型对象，则返回的结果为成功Promise对象
    // * 如果传入的参数为Promise对象，则参数的结果决定了resolve的结果
    // 即：传入的promise对象的状态决定了p2的状态（成功 or 失败）
    let p2 = Promise.resolve(new Promise((resolve, reject) => {
      // resolve('ok');
      reject('Error');

    }))
    // console.log(p2);
    p2.catch(reason => {
      console.log(reason);
    })
```
#### 2.5-Promise.reject 方法: (reason) => {}
> （1）reason: 失败的原因
> 说明：返回一个失败的promise对象(不论传入什么，返回结果的状态都是失败，即使传入的为成功的promise对象，状态也是失败)
```javascript
    let p1 = Promise.reject(521);
    let p2 = Promise.reject('iloveyou');
    let p3 = Promise.reject(new Promise((resolve, reject) => {
      resolve('ok');
    }))
    console.log('p1结果', p1);
    console.log('p2结果', p2);
    console.log('p3结果', p3);
```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210709151507.png)

#### 2.6-Promise.all 方法: (promises) => {}
> promises:包含n个promise的数组
> 说明：返回一个新的promise，只有所有的promise都成功才成功，只要有一个失败，就直接失败

```javascript
    //所有的promise都成功
    let p1 = new Promise((resolve, reject) => {
        resolve('ok');
      });
    let p2 = Promise.resolve('success');
    let p3 = Promise.resolve('Yeah');
    const result = Promise.all([p1, p2, p3])
    console.log(result);
```
结果：

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210709153134.png)

```javascript
    // promise中有一个失败
    let p1 = new Promise((resolve, reject) => {
        resolve('ok');
      })
    let p2 = Promise.resolve('success');
    let p3 = Promise.reject('Error');
    const result = Promise.all([p1, p2, p3])
    console.log(result);

```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210709153226.png)

#### 2.7 Promise.race 方法: (promises) => {}
> promises:包含n个promise的数组
> 说明：返回一个新的promise,第一个完成的promise结果状态就是最终的结果状态

```javascript
  let p1 = new Promise((resolve, reject) => {
    resolve('OK');
  })
  let p2 = Promise.resolve('Success');
  let p3 = Promise.resolve('Yeah');

  //调用
  const result = Promise.race([p1, p2, p3]);
  console.log(result);
```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220119150233.png)
状态为p1的状态，结果为p1的结果

```javascript
  let p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('OK');
    }, 1000)
  })
  let p2 = Promise.resolve('Success');
  let p3 = Promise.resolve('Yeah');

  //调用
  const result = Promise.race([p1, p2, p3]);
  console.log(result);

```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220119150538.png)
因为p1中有定时器，因此p2是第一个执行的，所以状态和结果都与p2的一致。

## 3.Promise关键问题

#### 3.1 如何修改对象的状态

三种方式：resolve函数、rejected函数、throw抛出错误

```javascript
// 1.resolve 函数
let p = new Promise((resolve, reject) => {
  resolve('ok'); //状态：pending => filfilled (resolved)
})

// 2.rejected 函数
let p = new Promise((resolve, reject) => {
  reject('error'); //状态：pending => rejected
})

// 3.抛出错误
let p = new Promise((resolve, reject) => {
  throw '出问题了';
})

```
#### 3.2 能否执行多个回调
Q: 一个promise指定多个成功/失败回调函数，都会调用吗？
A：当promise改变为对应状态时都会调用。
```javascript
  let p = new Promise((resolve, reject) => {
    resolve('OK');
  })

  //指定回调 - 1
  p.then(value => {
    console.log(value);
  })

  //指定回调 - 2
  p.then(value => {
    alert(value);
  })
```

#### 3.3 改变状态与指定回调的顺序问题
改变promise状态和指定回调函数谁先谁后？
1. 都有可能，正常情况下是现指定回调再改变状态，但也可以先改变状态再指定回调
2. 如何先改变状态再指定回调？
   ① 在执行器中直接调用resolve() / reject()
   ② 延迟更长时间才调用then()
3. 什么时候才能得到数据？
  ① 如果先指定的回调，那当状态发生改变时，回调函数就会调用，得到数据
  ② 如果先改变的状态，那当指定回调时，回调函数就会调用，得到数据

```javascript
  // 先改变状态，再执行回调
  let p = new Promise((resolve, reject) => {
    resolve('OK');
  })

  p.then(value => {
    console.log(value);
  })

  // 先执行回调，再改变状态
  let p = new Promise((resolve, reject) => {
    setTimeout(()=>{
      resolve('OK');
    },1000)
  })

  p.then(value => {
    console.log(value);
  })
```

#### 3.4 then方法返回结果由什么决定
promise.then()返回的新的promise的结果状态由什么决定？
1. 简单表达：由then()指定的回调函数执行的结果决定
2. 详细表达：
① 如果抛出异常，新promise变为rejected，reason为抛出的异常
② 如果返回的是非promise的任意值，新promise变为resolved，value为返回值
③ 如果返回的是另一个新promise，此promise的结果就会成为新promise的结果

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220119163613.png)

#### 3.5 串联多个任务
promise如何串联多个操作任务？
1. promise的then()返回一个新的promise，可以看成then()的链式调用
2. 通过then的链式调用串联多个同步/异步任务

```javascript
  let p = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('OK');
    }, 1000)
  })

  p.then(value => {
    return new Promise((resolve, reject) => {
      resolve('success');
    })
  }).then(value1 => {
    console.log('value1:',value1)
  }).then(value2 => {
    console.log('value2:',value2)
  })
```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220119170913.png)
说明：
value1是new Promise的返回值success，value2是value1（promise）的返回值，因其未定义，所以为undefined

#### 3.6 异常穿透
1. 当使用promise的then链式调用时，可以在最后指定失败的回调
2. 前面任何操作出现了异常，都会传到最后失败的回调中处理

```javascript
  let p = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('OK');
    }, 1000)
  })

  p.then(value => {
    return new Promise((resolve, reject) => {
      resolve('success');
    })
  }).then(value1 => {
    // console.log('value1:', value1)
    throw 'Error';
  }).then(value2 => {
    console.log('value2:', value2)
  }).catch(reason => {
    console.log(reason)
  })
```

#### 3.7 终端promise链
1. 当使用promise的then链式调用时，在中间中断，不再调用后面的回调函数
2. 唯一方法：在回调函数中返回一个pending状态的promise对象

```javascript
  let p = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('OK');
    }, 1000)
  })

  p.then(value => {
    return new Promise((resolve, reject) => {
      resolve('success');
    })
  }).then(value1 => {
    console.log('value1:', value1)
    // 有且只有一种方法：返回一个pending状态的promise对象
    return new Promise(() => {})
  }).then(value2 => {
    console.log('value2:', value2)
  }).catch(reason => {
    console.log(reason)
  })
```

## 4.Promise自定义封装(手写Promise)
#### 4.1 初始结构搭建
```javascript
// 声明构造函数
function Promise (executor) {

}
// 添加then方法
Promise.prototype.then = function (onResolved, onRejected) {

}
```

#### 4.2 resolve与reject的结构搭建

```javascript
// 声明构造函数
function Promise (executor) {
  // resolve函数
  function resolve (data) {

  }
  // reject函数
  function reject (data) {

  }
  // 同步调用【执行器函数】
  executor(resolve, reject);

}

// 添加then方法
Promise.prototype.then = function (onResolved, onRejected) {

}
```
#### 4.3 resolve与reject的代码实现

```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.PromiseState = 'pending';
  this.promiseResult = null;

  //保存实例对象的 this 的值
  const self = this;

  // resolve函数
  function resolve (data) {
    // 1.修改对象的状态(promiseState)
    self.PromiseState = 'fulfilled'; // resolved
    // 2.设置对象结果值(promiseResult)
    self.promiseResult = data;

  }
  // reject函数
  function reject (data) {
    // 1.修改对象的状态(promiseState)
    self.PromiseState = 'rejected';
    // 2.设置对象结果值(promiseResult)
    self.promiseResult = data;

  }
  // 同步调用【执行器函数】
  executor(resolve, reject);

}

// 添加then方法
Promise.prototype.then = function (onResolved, onRejected) {

}
```
#### 4.4 throw抛出异常改变状态

```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 保存实例对象的this的值
  const self = this;

  //resolve函数
  function resolve (data) {
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'fulfilled'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

  }
  // reject函数
  function reject (data) {
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

  }
  try {
    // 同步调用【执行器函数】
    executor(resolve, reject);
  } catch (e) {
    reject(e);

  }



}

// 添加 then 方法

Promise.prototype.then = function (onResolved, onRejected) {

}

```
#### 4.5 Promise对象状态只能修改一次
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 保存实例对象的this的值
  const self = this;

  //resolve函数
  function resolve (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'fulfilled'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;
  }
  try {
    // 同步调用【执行器函数】
    executor(resolve, reject);
  } catch (e) {
    reject(e);

  }



}

// 添加 then 方法
Promise.prototype.then = function (onResolved, onRejected) {

}
```
#### 4.6 then方法执行回调
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.PromiseState = 'pending';
  this.promiseResult = null;

  //保存实例对象的 this 的值
  const self = this;

  // resolve函数
  function resolve (data) {
    // 判断状态
    if (self.PromiseState !== 'pending') return;
    // 1.修改对象的状态(promiseState)
    this.PromiseState = 'fulfilled'; // resolved
    // 2.设置对象结果值(promiseResult)
    this.promiseResult = data;

  }
  // reject函数
  function reject (data) {
    // 判断状态
    if (self.PromiseState !== 'pending') return;
    // 1.修改对象的状态(promiseState)
    this.PromiseState = 'rejected';
    // 2.设置对象结果值(promiseResult)
    this.promiseResult = data;

  }

  try {
    // 同步调用【执行器函数】
    executor(resolve, reject);
  } catch (e) {
    //修改promise对象状态为【失败】
    reject(e);

  }
}


// 添加then方法
Promise.prototype.then = function (onResolved, onRejected) {
  // 调用回调函数，根据PromiseState
  if (this.PromiseState === 'fulfilled') {
    onResolved(this.promiseResult);
  }
  if (this.PromiseState === 'rejected') {
    onRejected(this.promiseResult);
  }

}
```

#### 4.7 异步任务回调的执行
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.PromiseState = 'pending';
  this.promiseResult = null;
  // 声明属性
  this.callback = {};

  //保存实例对象的 this 的值
  const self = this;

  // resolve函数
  function resolve (data) {
    // 判断状态
    if (self.PromiseState !== 'pending') return;
    // 1.修改对象的状态(promiseState)
    this.PromiseState = 'fulfilled'; // resolved
    // 2.设置对象结果值(promiseResult)
    this.promiseResult = data;
    // 调用成功的回调函数
    if (self.callback.onResolved) {
      self.callback.onResolved(data);
    }

  }
  // reject函数
  function reject (data) {
    // 判断状态
    if (self.PromiseState !== 'pending') return;
    // 1.修改对象的状态(promiseState)
    this.PromiseState = 'rejected';
    // 2.设置对象结果值(promiseResult)
    this.promiseResult = data;
    // 调用失败的回调函数
    if (self.callback.onRejected) {
      self.callback.onResolved(data);
    }

  }

  try {
    // 同步调用【执行器函数】
    executor(resolve, reject);
  } catch (e) {
    //修改promise对象状态为【失败】
    reject(e);

  }
}


// 添加then方法
Promise.prototype.then = function (onResolved, onRejected) {
  // 调用回调函数，根据PromiseState
  if (this.PromiseState === 'fulfilled') {
    onResolved(this.promiseResult);
  }
  if (this.PromiseState === 'rejected') {
    onRejected(this.promiseResult);
  }
  // 判断pending状态
  if (this.PromiseState === 'pending') {
    // 保存回调函数
    this.callback = {
      onResolved: onResolved,
      onRejected: onRejected
    }


  }
}
```
#### 4.8 指定多个回调的实现

```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.PromiseState = 'pending';
  this.promiseResult = null;
  // 声明属性
  this.callbacks = [];

  //保存实例对象的 this 的值
  const self = this;

  // resolve函数
  function resolve (data) {
    // 判断状态
    if (self.PromiseState !== 'pending') return;
    // 1.修改对象的状态(promiseState)
    this.PromiseState = 'fulfilled'; // resolved
    // 2.设置对象结果值(promiseResult)
    this.promiseResult = data;
    // 调用成功的回调函数
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })

  }
  // reject函数
  function reject (data) {
    // 判断状态
    if (self.PromiseState !== 'pending') return;
    // 1.修改对象的状态(promiseState)
    this.PromiseState = 'rejected';
    // 2.设置对象结果值(promiseResult)
    this.promiseResult = data;
    // 调用失败的回调函数
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })


  }

  try {
    // 同步调用【执行器函数】
    executor(resolve, reject);
  } catch (e) {
    //修改promise对象状态为【失败】
    reject(e);

  }
}

// 添加then方法
Promise.prototype.then = function (onResolved, onRejected) {
  // 调用回调函数，根据PromiseState
  if (this.PromiseState === 'fulfilled') {
    onResolved(this.promiseResult);
  }
  if (this.PromiseState === 'rejected') {
    onRejected(this.promiseResult);
  }
  // 判断pending状态
  if (this.PromiseState === 'pending') {
    // 保存回调函数
    this.callbacks.push({
      onResolved: onResolved,
      onRejected: onRejected
    });
  }
}
```

#### 4.9 同步任务下then方法返回结果的实现
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.PromiseState = 'pending';
  this.promiseResult = null;
  // 声明属性
  this.callbacks = [];

  //保存实例对象的 this 的值
  const self = this;

  // resolve函数
  function resolve (data) {
    // 判断状态
    if (self.PromiseState !== 'pending') return;
    // 1.修改对象的状态(promiseState)
    this.PromiseState = 'fulfilled'; // resolved
    // 2.设置对象结果值(promiseResult)
    this.promiseResult = data;
    // 调用成功的回调函数
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })

  }
  // reject函数
  function reject (data) {
    // 判断状态
    if (self.PromiseState !== 'pending') return;
    // 1.修改对象的状态(promiseState)
    this.PromiseState = 'rejected';
    // 2.设置对象结果值(promiseResult)
    this.promiseResult = data;
    // 调用失败的回调函数
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })


  }

  try {
    // 同步调用【执行器函数】
    executor(resolve, reject);
  } catch (e) {
    //修改promise对象状态为【失败】
    reject(e);

  }
}


// 添加then方法
Promise.prototype.then = function (onResolved, onRejected) {
  return new Promise((resolve, reject) => {
    // 调用回调函数，根据PromiseState
    if (this.PromiseState === 'fulfilled') {
      try {
        // 获取回调函数的执行结果
        let result = onResolved(this.promiseResult);
        // 判断
        if (result instanceof Promise) {
          // 如果是promise类型的对象
          result.then(v => {
            resolve(v);
          }, r => {
            reject(r);
          })
        } else {
          // 结果的对象状态为成功
          resolve(result);
        }
      } catch (e) {
        resolve(e);
      }
    }
    if (this.PromiseState === 'rejected') {
      onRejected(this.promiseResult);
    }
    // 判断pending状态
    if (this.PromiseState === 'pending') {
      // 保存回调函数
      this.callbacks.push({
        onResolved: onResolved,
        onRejected: onRejected
      });
    }
  })

}
```
#### 4.10 异步任务下then方法返回结果的实现
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.PromiseState = 'pending';
  this.promiseResult = null;
  // 声明属性
  this.callbacks = [];

  //保存实例对象的 this 的值
  const self = this;

  // resolve函数
  function resolve (data) {
    // 判断状态
    if (self.PromiseState !== 'pending') return;
    // 1.修改对象的状态(promiseState)
    this.PromiseState = 'fulfilled'; // resolved
    // 2.设置对象结果值(promiseResult)
    this.promiseResult = data;
    // 调用成功的回调函数
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })

  }
  // reject函数
  function reject (data) {
    // 判断状态
    if (self.PromiseState !== 'pending') return;
    // 1.修改对象的状态(promiseState)
    this.PromiseState = 'rejected';
    // 2.设置对象结果值(promiseResult)
    this.promiseResult = data;
    // 调用失败的回调函数
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })


  }

  try {
    // 同步调用【执行器函数】
    executor(resolve, reject);
  } catch (e) {
    //修改promise对象状态为【失败】
    reject(e);

  }
}


// 添加then方法
Promise.prototype.then = function (onResolved, onRejected) {
  const self = this;
  return new Promise((resolve, reject) => {
    // 调用回调函数，根据PromiseState
    if (this.PromiseState === 'fulfilled') {
      try {
        // 获取回调函数的执行结果
        let result = onResolved(this.promiseResult);
        // 判断
        if (result instanceof Promise) {
          // 如果是promise类型的对象
          result.then(v => {
            resolve(v);
          }, r => {
            reject(r);
          })
        } else {
          // 结果的对象状态为成功
          resolve(result);
        }
      } catch (e) {
        resolve(e);
      }
    }
    if (this.PromiseState === 'rejected') {
      onRejected(this.promiseResult);
    }
    // 判断pending状态
    if (this.PromiseState === 'pending') {
      // 保存回调函数
      this.callbacks.push({
        onResolved: function () {
          // 执行成功回调函数
          let result = onResolved(self.PromiseResult);
          if (result instanceof Promise) {
            result.then(v => {
              resolve(v);
            }, r => {
              reject(r);
            })
          } else {
            resolve(result);

          }
        },
        onRejected: function () {
          try {
            let result = onRejected(self.PromiseResult);
            if (result instanceof Promise) {
              result.then(v => {
                resolve(v);
              }, r => {
                reject(r);
              })
            } else {
              resolve(result);
            }
          } catch (e) {
            reject(e);

          }
        }
      });
    }
  })

}
```
#### 4.11 then方法的完善与优化

## 5.async 与 await

