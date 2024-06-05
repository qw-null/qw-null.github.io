---
title: Promise学习
date: 2022-01-18 17:13:57
tags:
- JavaScript
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
<html>

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
  * pending 未决定的
  * resolved / fullfiled 成功
  * rejected 失败
   
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

三种方式：resolve函数、reject函数、throw抛出错误

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
1. 都有可能，正常情况下是先指定回调再改变状态，但也可以先改变状态再指定回调
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

#### 3.7 中断promise链
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
  // 调用回调函数,通过PromiseState决定调用谁
  if (this.promiseState === 'fulfilled') {
    onResolved(this.promiseResult);
  }
  if (this.promiseState === 'rejected') {
    onRejected(this.promiseResult);
  }
}
```
#### 4.7 异步任务回调的执行
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 声明属性 --> 用于then方法中保存回调函数
  this.callback = {}

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

    // 调用成功的回调函数 -- 异步任务
    if (self.callback.onResolved) {
      self.callback.onResolved(data)
    }
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

    // 调用成功的回调函数 -- 异步任务
    if (self.callback.onRejected) {
      self.callback.onRejected(data)
    }
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
  // 调用回调函数,通过PromiseState决定调用谁
  if (this.promiseState === 'fulfilled') {
    onResolved(this.promiseResult);
  }
  if (this.promiseState === 'rejected') {
    onRejected(this.promiseResult);
  }
  // 异步执行时，需要处理pending状态
  if (this.promiseState === 'pending') {
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
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 声明属性 --> 用于then方法中保存回调函数
  this.callbacks = []

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

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })
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
  // 调用回调函数,通过PromiseState决定调用谁
  if (this.promiseState === 'fulfilled') {
    onResolved(this.promiseResult);
  }
  if (this.promiseState === 'rejected') {
    onRejected(this.promiseResult);
  }
  // 异步执行时，需要处理pending状态
  if (this.promiseState === 'pending') {
    // 保存回调函数
    this.callbacks.push({
      onResolved: onResolved,
      onRejected: onRejected
    })
  }
}
```

#### 4.9 同步任务下then方法返回结果的实现

```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 声明属性 --> 用于then方法中保存回调函数
  this.callbacks = []

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

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })
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
  const self = this;
  return new Promise((resolve, reject) => {
    // 调用回调函数,通过PromiseState决定调用谁
    if (this.promiseState === 'fulfilled') {
      try {
        // 获取回调函数的执行结果
        let result = onResolved(this.promiseResult);
        // 判断result是否是promise对象
        if (result instanceof Promise) {
          // 结果是promise对象
          result.then(v => {
            resolve(v);
          }, r => {
            reject(r);
          })

        } else {
          // 结果对象的状态为成功
          resolve(result);
        }

      } catch (e) {
        reject(e);
      }
    }
    if (this.promiseState === 'rejected') {
      onRejected(this.promiseResult);
    }
    // 异步执行时，需要处理pending状态
    if (this.promiseState === 'pending') {
      // 保存回调函数
      this.callbacks.push({
        onResolved: function () {
          try {
            // 执行成功回调函数
            let result = onResolved(self.promiseResult);
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
        },

        onRejected: function () {
          try {
            let result = onRejected(self.promiseResult)
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
      })
    }
  })
}
```

#### 4.10 异步任务下then方法返回结果的实现
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 声明属性 --> 用于then方法中保存回调函数
  this.callbacks = []

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

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })
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
  const self = this;
  return new Promise((resolve, reject) => {
    // 封装函数
    function callback (type) {
      try {
        // 获取回调函数的执行结果
        let result = type(self.promiseResult);
        // 判断result是否是promise对象
        if (result instanceof Promise) {
          // 结果是promise对象
          result.then(v => {
            resolve(v);
          }, r => {
            reject(r);
          })
        } else {
          // 结果对象的状态为成功
          resolve(result);
        }
      } catch (e) {
        reject(e);
      }

    }
    // 调用回调函数,通过PromiseState决定调用谁
    if (this.promiseState === 'fulfilled') {
      callback(onResolved);
    }
    if (this.promiseState === 'rejected') {
      callback(onRejected);
    }
    // 异步执行时，需要处理pending状态
    if (this.promiseState === 'pending') {
      // 保存回调函数
      this.callbacks.push({
        onResolved: function () {
          callback(onResolved);
        },
        onRejected: function () {
          callback(onRejected);
        }
      })
    }
  })
}
```
#### 4.11 then方法的完善与优化

```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 声明属性 --> 用于then方法中保存回调函数
  this.callbacks = []

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

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })
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
  const self = this;
  return new Promise((resolve, reject) => {
    // 封装函数
    function callback (type) {
      try {
        // 获取回调函数的执行结果
        let result = type(self.promiseResult);
        // 判断result是否是promise对象
        if (result instanceof Promise) {
          // 结果是promise对象
          result.then(v => {
            resolve(v);
          }, r => {
            reject(r);
          })
        } else {
          // 结果对象的状态为成功
          resolve(result);
        }
      } catch (e) {
        reject(e);
      }

    }
    // 调用回调函数,通过PromiseState决定调用谁
    if (this.promiseState === 'fulfilled') {
      callback(onResolved);
    }
    if (this.promiseState === 'rejected') {
      callback(onRejected);
    }
    // 异步执行时，需要处理pending状态
    if (this.promiseState === 'pending') {
      // 保存回调函数
      this.callbacks.push({
        onResolved: function () {
          callback(onResolved);
        },
        onRejected: function () {
          callback(onRejected);
        }
      })
    }
  })
}

```

#### 4.11 catch方法、异常穿透、值传递
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 声明属性 --> 用于then方法中保存回调函数
  this.callbacks = []

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

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })
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
  const self = this;
  // 判断回调参数 -- catch方法使用
  if (typeof onRejected !== 'function') {
    onRejected = reason => {
      throw reason;
    }
  }
  if (typeof onResolved !== 'function') {
    onResolved = value => value;

  }

  return new Promise((resolve, reject) => {
    // 封装函数
    function callback (type) {
      try {
        // 获取回调函数的执行结果
        let result = type(self.promiseResult);
        // 判断result是否是promise对象
        if (result instanceof Promise) {
          // 结果是promise对象
          result.then(v => {
            resolve(v);
          }, r => {
            reject(r);
          })
        } else {
          // 结果对象的状态为成功
          resolve(result);
        }
      } catch (e) {
        reject(e);
      }

    }
    // 调用回调函数,通过PromiseState决定调用谁
    if (this.promiseState === 'fulfilled') {
      callback(onResolved);
    }
    if (this.promiseState === 'rejected') {
      callback(onRejected);
    }
    // 异步执行时，需要处理pending状态
    if (this.promiseState === 'pending') {
      // 保存回调函数
      this.callbacks.push({
        onResolved: function () {
          callback(onResolved);
        },
        onRejected: function () {
          callback(onRejected);
        }
      })
    }
  })
}

// 添加 catch 方法
Promise.prototype.catch = function (onRejected) {
  return this.then(undefined, onRejected)
}
```
#### 4.12 Promise.resolve()的封装
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 声明属性 --> 用于then方法中保存回调函数
  this.callbacks = []

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

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })
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
  const self = this;
  // 判断回调参数 -- catch方法使用
  if (typeof onRejected !== 'function') {
    onRejected = reason => {
      throw reason;
    }
  }
  if (typeof onResolved !== 'function') {
    onResolved = value => value;

  }

  return new Promise((resolve, reject) => {
    // 封装函数
    function callback (type) {
      try {
        // 获取回调函数的执行结果
        let result = type(self.promiseResult);
        // 判断result是否是promise对象
        if (result instanceof Promise) {
          // 结果是promise对象
          result.then(v => {
            resolve(v);
          }, r => {
            reject(r);
          })
        } else {
          // 结果对象的状态为成功
          resolve(result);
        }
      } catch (e) {
        reject(e);
      }

    }
    // 调用回调函数,通过PromiseState决定调用谁
    if (this.promiseState === 'fulfilled') {
      callback(onResolved);
    }
    if (this.promiseState === 'rejected') {
      callback(onRejected);
    }
    // 异步执行时，需要处理pending状态
    if (this.promiseState === 'pending') {
      // 保存回调函数
      this.callbacks.push({
        onResolved: function () {
          callback(onResolved);
        },
        onRejected: function () {
          callback(onRejected);
        }
      })
    }
  })
}

// 添加 catch 方法
Promise.prototype.catch = function (onRejected) {
  return this.then(undefined, onRejected)
}

// 添加 resolve  方法
Promise.resolve = function (value) {
  // 返回Promise对象
  return new Promise((resolve, reject) => {
    if (value instanceof Promise) {
      value.then(v => {
        resolve(v);
      }, r => {
        reject(r);
      })

    } else {
      //状态设置为成功
      resolve(value);
    }

  })

}

```

#### 4.13 Promise.reject()的封装
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 声明属性 --> 用于then方法中保存回调函数
  this.callbacks = []

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

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })
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
  const self = this;
  // 判断回调参数 -- catch方法使用
  if (typeof onRejected !== 'function') {
    onRejected = reason => {
      throw reason;
    }
  }
  if (typeof onResolved !== 'function') {
    onResolved = value => value;

  }

  return new Promise((resolve, reject) => {
    // 封装函数
    function callback (type) {
      try {
        // 获取回调函数的执行结果
        let result = type(self.promiseResult);
        // 判断result是否是promise对象
        if (result instanceof Promise) {
          // 结果是promise对象
          result.then(v => {
            resolve(v);
          }, r => {
            reject(r);
          })
        } else {
          // 结果对象的状态为成功
          resolve(result);
        }
      } catch (e) {
        reject(e);
      }

    }
    // 调用回调函数,通过PromiseState决定调用谁
    if (this.promiseState === 'fulfilled') {
      callback(onResolved);
    }
    if (this.promiseState === 'rejected') {
      callback(onRejected);
    }
    // 异步执行时，需要处理pending状态
    if (this.promiseState === 'pending') {
      // 保存回调函数
      this.callbacks.push({
        onResolved: function () {
          callback(onResolved);
        },
        onRejected: function () {
          callback(onRejected);
        }
      })
    }
  })
}

// 添加 catch 方法
Promise.prototype.catch = function (onRejected) {
  return this.then(undefined, onRejected)
}

// 添加 resolve  方法
Promise.resolve = function (value) {
  // 返回Promise对象
  return new Promise((resolve, reject) => {
    if (value instanceof Promise) {
      value.then(v => {
        resolve(v);
      }, r => {
        reject(r);
      })

    } else {
      //状态设置为成功
      resolve(value);
    }

  })

}

// 添加 reject 方法
Promise.reject = function (reason) {
  return new Promise((resolve, reject) => {
    reject(reason);
  })
}
```

#### 4.14 Promise.all()的封装
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 声明属性 --> 用于then方法中保存回调函数
  this.callbacks = []

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

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })
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
  const self = this;
  // 判断回调参数 -- catch方法使用
  if (typeof onRejected !== 'function') {
    onRejected = reason => {
      throw reason;
    }
  }
  if (typeof onResolved !== 'function') {
    onResolved = value => value;

  }

  return new Promise((resolve, reject) => {
    // 封装函数
    function callback (type) {
      try {
        // 获取回调函数的执行结果
        let result = type(self.promiseResult);
        // 判断result是否是promise对象
        if (result instanceof Promise) {
          // 结果是promise对象
          result.then(v => {
            resolve(v);
          }, r => {
            reject(r);
          })
        } else {
          // 结果对象的状态为成功
          resolve(result);
        }
      } catch (e) {
        reject(e);
      }

    }
    // 调用回调函数,通过PromiseState决定调用谁
    if (this.promiseState === 'fulfilled') {
      callback(onResolved);
    }
    if (this.promiseState === 'rejected') {
      callback(onRejected);
    }
    // 异步执行时，需要处理pending状态
    if (this.promiseState === 'pending') {
      // 保存回调函数
      this.callbacks.push({
        onResolved: function () {
          callback(onResolved);
        },
        onRejected: function () {
          callback(onRejected);
        }
      })
    }
  })
}

// 添加 catch 方法
Promise.prototype.catch = function (onRejected) {
  return this.then(undefined, onRejected)
}

// 添加 resolve  方法
Promise.resolve = function (value) {
  // 返回Promise对象
  return new Promise((resolve, reject) => {
    if (value instanceof Promise) {
      value.then(v => {
        resolve(v);
      }, r => {
        reject(r);
      })

    } else {
      //状态设置为成功
      resolve(value);
    }

  })

}

// 添加 reject 方法
Promise.reject = function (reason) {
  return new Promise((resolve, reject) => {
    reject(reason);
  })
}

// 添加 all 方法
Promise.all = function (promises) {
  // 返回结果为promise对象
  return new Promise((resolve, reject) => {
    // 声明变量-> 保证所有的promise都成功才可以执行resolve()
    let count = 0;
    // 存放成功结果
    let arr = [];
    // 遍历 promises
    for (let i = 0; i < promises.length; i++) {
      promises[i].then(v => {
        // 得知对象的状态是成功
        // 每个promise对象都成功
        count++;
        // 将当前promise对象成功的结果存入到数组中
        arr[i] = v;
        // 判断
        if (count === promises.length) {
          // 修改状态
          resolve(arr);
        }
      }, r => {
        reject(r);

      })
    }
  })
}

```

#### 4.14 Promise.race()的封装
```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 声明属性 --> 用于then方法中保存回调函数
  this.callbacks = []

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

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onResolved(data);
    })
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

    // 调用成功的回调函数 -- 异步任务
    self.callbacks.forEach(item => {
      item.onRejected(data);
    })
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
  const self = this;
  // 判断回调参数 -- catch方法使用
  if (typeof onRejected !== 'function') {
    onRejected = reason => {
      throw reason;
    }
  }
  if (typeof onResolved !== 'function') {
    onResolved = value => value;

  }

  return new Promise((resolve, reject) => {
    // 封装函数
    function callback (type) {
      try {
        // 获取回调函数的执行结果
        let result = type(self.promiseResult);
        // 判断result是否是promise对象
        if (result instanceof Promise) {
          // 结果是promise对象
          result.then(v => {
            resolve(v);
          }, r => {
            reject(r);
          })
        } else {
          // 结果对象的状态为成功
          resolve(result);
        }
      } catch (e) {
        reject(e);
      }

    }
    // 调用回调函数,通过PromiseState决定调用谁
    if (this.promiseState === 'fulfilled') {
      callback(onResolved);
    }
    if (this.promiseState === 'rejected') {
      callback(onRejected);
    }
    // 异步执行时，需要处理pending状态
    if (this.promiseState === 'pending') {
      // 保存回调函数
      this.callbacks.push({
        onResolved: function () {
          callback(onResolved);
        },
        onRejected: function () {
          callback(onRejected);
        }
      })
    }
  })
}

// 添加 catch 方法
Promise.prototype.catch = function (onRejected) {
  return this.then(undefined, onRejected)
}

// 添加 resolve  方法
Promise.resolve = function (value) {
  // 返回Promise对象
  return new Promise((resolve, reject) => {
    if (value instanceof Promise) {
      value.then(v => {
        resolve(v);
      }, r => {
        reject(r);
      })

    } else {
      //状态设置为成功
      resolve(value);
    }

  })

}

// 添加 reject 方法
Promise.reject = function (reason) {
  return new Promise((resolve, reject) => {
    reject(reason);
  })
}

// 添加 all 方法
Promise.all = function (promises) {
  // 返回结果为promise对象
  return new Promise((resolve, reject) => {
    // 声明变量-> 保证所有的promise都成功才可以执行resolve()
    let count = 0;
    // 存放成功结果
    let arr = [];
    // 遍历 promises
    for (let i = 0; i < promises.length; i++) {
      promises[i].then(v => {
        // 得知对象的状态是成功
        // 每个promise对象都成功
        count++;
        // 将当前promise对象成功的结果存入到数组中
        arr[i] = v;
        // 判断
        if (count === promises.length) {
          // 修改状态
          resolve(arr);
        }
      }, r => {
        reject(r);

      })
    }
  })
}

// 添加 race 方法
Promise.race = function (promises) {
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

#### 4.15 then方法回调的异步执行

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220125163632.png)

```javascript
// 声明构造函数
function Promise (executor) {
  // 添加属性
  this.promiseState = 'pending';
  this.promiseResult = null;

  // 声明属性 --> 用于then方法中保存回调函数
  this.callbacks = []

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

    // 调用成功的回调函数 -- 异步任务
    setTimeout(() => {
      self.callbacks.forEach(item => {
        item.onResolved(data);
      })
    })
  }
  // reject函数
  function reject (data) {
    // 判断
    if (self.promiseState !== 'pending') return;
    // 1. 修改对象状态（promiseState）
    self.promiseState = 'rejected'
    // 2.设置对象结果值（promiseResult）
    self.promiseResult = data;

    // 调用成功的回调函数 -- 异步任务
    setTimeout(() => {
      self.callbacks.forEach(item => {
        item.onRejected(data);
      })
    })
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
  const self = this;
  // 判断回调参数 -- catch方法使用
  if (typeof onRejected !== 'function') {
    onRejected = reason => {
      throw reason;
    }
  }
  if (typeof onResolved !== 'function') {
    onResolved = value => value;

  }

  return new Promise((resolve, reject) => {
    // 封装函数
    function callback (type) {
      try {
        // 获取回调函数的执行结果
        let result = type(self.promiseResult);
        // 判断result是否是promise对象
        if (result instanceof Promise) {
          // 结果是promise对象
          result.then(v => {
            resolve(v);
          }, r => {
            reject(r);
          })
        } else {
          // 结果对象的状态为成功
          resolve(result);
        }
      } catch (e) {
        reject(e);
      }

    }
    // 调用回调函数,通过PromiseState决定调用谁
    if (this.promiseState === 'fulfilled') {
      setTimeout(() => {
        callback(onResolved);
      })
    }
    if (this.promiseState === 'rejected') {
      setTimeout(() => {
        callback(onRejected);
      })
    }
    // 异步执行时，需要处理pending状态
    if (this.promiseState === 'pending') {
      // 保存回调函数
      this.callbacks.push({
        onResolved: function () {
          callback(onResolved);
        },
        onRejected: function () {
          callback(onRejected);
        }
      })
    }
  })
}

// 添加 catch 方法
Promise.prototype.catch = function (onRejected) {
  return this.then(undefined, onRejected)
}

// 添加 resolve  方法
Promise.resolve = function (value) {
  // 返回Promise对象
  return new Promise((resolve, reject) => {
    if (value instanceof Promise) {
      value.then(v => {
        resolve(v);
      }, r => {
        reject(r);
      })

    } else {
      //状态设置为成功
      resolve(value);
    }

  })

}

// 添加 reject 方法
Promise.reject = function (reason) {
  return new Promise((resolve, reject) => {
    reject(reason);
  })
}

// 添加 all 方法
Promise.all = function (promises) {
  // 返回结果为promise对象
  return new Promise((resolve, reject) => {
    // 声明变量-> 保证所有的promise都成功才可以执行resolve()
    let count = 0;
    // 存放成功结果
    let arr = [];
    // 遍历 promises
    for (let i = 0; i < promises.length; i++) {
      promises[i].then(v => {
        // 得知对象的状态是成功
        // 每个promise对象都成功
        count++;
        // 将当前promise对象成功的结果存入到数组中
        arr[i] = v;
        // 判断
        if (count === promises.length) {
          // 修改状态
          resolve(arr);
        }
      }, r => {
        reject(r);

      })
    }
  })
}

// 添加 race 方法
Promise.race = function (promises) {
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
#### 4.16 封装Promise类
```javascript
class Promise {
  //构造方法
  constructor(executor) {
    // 添加属性
    this.promiseState = 'pending';
    this.promiseResult = null;

    // 声明属性 --> 用于then方法中保存回调函数
    this.callbacks = []

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

      // 调用成功的回调函数 -- 异步任务
      setTimeout(() => {
        self.callbacks.forEach(item => {
          item.onResolved(data);
        })
      })
    }
    // reject函数
    function reject (data) {
      // 判断
      if (self.promiseState !== 'pending') return;
      // 1. 修改对象状态（promiseState）
      self.promiseState = 'rejected'
      // 2.设置对象结果值（promiseResult）
      self.promiseResult = data;

      // 调用成功的回调函数 -- 异步任务
      setTimeout(() => {
        self.callbacks.forEach(item => {
          item.onRejected(data);
        })
      })
    }
    try {
      // 同步调用【执行器函数】
      executor(resolve, reject);
    } catch (e) {
      reject(e);
    }

  }

  //then方法
  then (onResolved, onRejected) {
    const self = this;
    // 判断回调参数 -- catch方法使用
    if (typeof onRejected !== 'function') {
      onRejected = reason => {
        throw reason;
      }
    }
    if (typeof onResolved !== 'function') {
      onResolved = value => value;

    }

    return new Promise((resolve, reject) => {
      // 封装函数
      function callback (type) {
        try {
          // 获取回调函数的执行结果
          let result = type(self.promiseResult);
          // 判断result是否是promise对象
          if (result instanceof Promise) {
            // 结果是promise对象
            result.then(v => {
              resolve(v);
            }, r => {
              reject(r);
            })
          } else {
            // 结果对象的状态为成功
            resolve(result);
          }
        } catch (e) {
          reject(e);
        }

      }
      // 调用回调函数,通过PromiseState决定调用谁
      if (this.promiseState === 'fulfilled') {
        setTimeout(() => {
          callback(onResolved);
        })
      }
      if (this.promiseState === 'rejected') {
        setTimeout(() => {
          callback(onRejected);
        })
      }
      // 异步执行时，需要处理pending状态
      if (this.promiseState === 'pending') {
        // 保存回调函数
        this.callbacks.push({
          onResolved: function () {
            callback(onResolved);
          },
          onRejected: function () {
            callback(onRejected);
          }
        })
      }
    })
  }

  //catch方法
  catch (onRejected) {
    return this.then(undefined, onRejected)
  }

  //添加resolve方法
  static resolve (value) {
    // 返回Promise对象
    return new Promise((resolve, reject) => {
      if (value instanceof Promise) {
        value.then(v => {
          resolve(v);
        }, r => {
          reject(r);
        })

      } else {
        //状态设置为成功
        resolve(value);
      }
    })
  }

  // 添加 reject 方法
  static reject (reason) {
    return new Promise((resolve, reject) => {
      reject(reason);
    })
  }

  // 添加 all 方法
  static all (promises) {
    // 返回结果为promise对象
    return new Promise((resolve, reject) => {
      // 声明变量-> 保证所有的promise都成功才可以执行resolve()
      let count = 0;
      // 存放成功结果
      let arr = [];
      // 遍历 promises
      for (let i = 0; i < promises.length; i++) {
        promises[i].then(v => {
          // 得知对象的状态是成功
          // 每个promise对象都成功
          count++;
          // 将当前promise对象成功的结果存入到数组中
          arr[i] = v;
          // 判断
          if (count === promises.length) {
            // 修改状态
            resolve(arr);
          }
        }, r => {
          reject(r);

        })
      }
    })
  }

  // 添加 race 方法
  static race (promises) {
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
}
```
## 5.async 与 await
#### 5.1 async函数
1. 函数的返回值为promise对象
2. promise对象的结果由async函数执行的返回值决定

async函数返回值的规则与then方法的规则是一致的

```javascript
async function main() {
      // 1.如果返回值是一个非Promise类型的数据
      // promiseState为fulfilled，promiseResult为返回的值
      return 521;

      // 2.如果返回的是一个Promise对象
      // promiseState为返回promise对象的状态，promiseResult为返回的promise对象的值
      return new Promise((resolve, reject) => {
      resolve('ok')
      reject('ERR')
      })

      //3.抛出异常
      // promiseState为reject，promiseResult为抛出异常的值
      throw 'error'

    }
```

#### 5.2 await表达式
1. await右侧的表达式一般为promise对象，但也可以是其他的值
2. 如果表达式是promise对象，await返回的是promise成功的值
3. 如果表达式是其他值，直接将此值作为await的返回值

<b>注意</b>

+ await必须写在async函数中，但async函数中可以没有await
+ 如果await的promise失败了，就会抛出异常，需要通过try...catch捕获处理

```await 10;```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220126094102.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220126095436.png)

#### 5.3  async和await结合

```javascript
const fs = require('fs');
const util = require('util')
const mineReadFile = util.promisify(fs.readFile)

// 回调函数的方式
fs.readFile('1.txt', (err, data1) => {
  if (err) throw err;
  fs.readFile('2.txt', (err, data2) => {
    if (err) throw err;
    fs.readFile('3.txt', (err, data3) => {
      if (err) throw err;
      console.log(data1 + data2 + data3);

    })
  })
})


// async 与 await
async function main () {
  try {
    // 读取文件内容
    let data1 = await mineReadFile('1.txt')
    let data2 = await mineReadFile('2.txt')
    let data3 = await mineReadFile('3.txt')
    console.log(data1 + data2 + data3);

  } catch (e) {
    console.log(e);
  }
}

main();

```

async 与 await方式非常简洁，且捕获异常也十分简单。

#### 5.4 async 与 await结合发送AJAX
情景：点击获取段子按钮，发送ajax请求，获取数据

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>async与await结合发送AJAX</title>
</head>

<body>
  <button id="btn">获取段子</button>

  <script>
    function sendAJAX(url) {
      return new Promise((resolve, reject) => {
        //1.创建对象
        const xhr = new XMLHttpRequest();
        //2.初始化
        xhr.open('GET', url);
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
      })
    }

    let btn = document.querySelector('#btn');
    btn.addEventListener('click', async function () {
      // 获取段子信息
      let duanzi = await sendAJAX('https://api.apiopen.top/getJoke');
      console.log(duanzi);


    })

  </script>

</body>

</html>

```