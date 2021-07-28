---
title: Promise学习
date: 2021-07-02 17:13:57
categories:
- Vue学习
tags:
- Promise
---
[尚硅谷Web前端Promise教程从入门到精通（2021抢先版）](https://www.bilibili.com/video/BV1GA411x7z1?p=2&share_source=copy_web)

## 1.Promise介绍与基本使用
### 1.1 理解
1.抽象表达
+ Promise是一门新的技术（ES6规范）
+ Promise是JS中进行异步编程的新解决方案
> 备注：旧方案是单纯使用回调函数

2.具体表达
+ 从语法上来说：Promise是一个构造函数
+ 从功能上来说：Promise对象用来封装一个异步操作并且可以获取其成功/失败的结果值

异步编程：
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
1.旧的：必须在启动异步任务前指定
2.promise:启动异步任务=>返回promise对象=>给promise对象绑定回调函数（甚至可以在异步任务结束后指定/多个）
#### 1.2.2支持链式调用，可以解决回调地狱问题
1.什么是回调地狱?
回调函数嵌套调用，外部回调函数异步执行的结果是嵌套的回调执行的条件
![回调地狱常见情形](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210703102534.png)
2.回调地狱的缺点？
不便于阅读
不便于异常处理
3.解决方案？
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
  //
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
   
  1.3.1 Promise的状态的改变
  1.pending 变为 resolved
  2.pending 变为 rejected
  说明：只有这两种状态改变的方式，且一个promise对象只能改变一次
        无论变为成功还是失败，都会有一个结果数据
        成功的结果数据一般称为value，失败的结果数据一般称为reason

### 1.4 Promise的值
实例对象中的另一个属性 [RromiseResult]
保存着异步任务[成功/失败]的结果
*resolve
*reject

### 1.5 Promise的基本流程
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210709104619.png)
  

## 2.Promise API
#### 2.1-Promise 构造函数: Promise (excutor) {}
> (1) executor 函数: 执行器 (resolve, reject) => {}
> (2) resolve 函数: 内部定义成功时我们调用的函数 value => {}
> (3) reject 函数: 内部定义失败时我们调用的函数 reason => {}
> 说明: executor 会在 Promise 内部立即同步调用,异步操作在执行器中执行,换话说Promise支持同步也支持异步操作

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
    // 如果传入的参数为非Promise类型对象，则返回的结果为成功Promise对象
    // 如果传入的参数为Promise对象，则参数的结果决定了resolve的结果
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
> 说明：返回一个失败的promise对象(不论传入什么，返回结果的状态都是失败)
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

#### 2.7-Promise.race 方法: (promises) => {}
> promises:包含n个promise的数组
> 说明：返回一个新的promise,第一个完成的promise结果状态就是最终的结果状态 

## 3.Promise关键问题

## 4.Promise自定义封装

## 5.async 与 await

