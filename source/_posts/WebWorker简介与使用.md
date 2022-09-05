---
title: WebWorker简介与使用
date: 2022-09-05 09:52:47
tags:
- JavaScript
---

### 1.什么是Web Worker？
H5提供了JS分线程的实现，取名为：Web Workers，<mark>**web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能，我们可以将一些大计算量的代码交由web Worker 运行而不冻结用户界面**</mark>。你可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。<b style="background:pink">但是子线程完全受主线程控制，且不操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质</b>。

### 2.Web Worker的作用
`Web Worker` 的作用：就是为 `JavaScript` 创造多线程环境，允许主线程创建 `Worker` 线程，将一些任务分配给后者运行。在主线程运行的同时，`Worker` 线程在后台运行，两者互不干扰。等到 `Worker` 线程完成计算任务，再把结果返回给主线程。这样的好处是，一些计算密集型或高延迟的任务，被 `Worker` 线程负担了，主线程（通常负责 `UI` 交互）就会很流畅，不会被阻塞或拖慢。

<mark>`Worker` 线程一旦新建成功，就会始终运行，不会被主线程上的活动（比如用户点击按钮、提交表单）打断</mark>。这样有利于随时响应主线程的通信。但是，<mark>这也造成了 `Worker` 比较耗费资源，不应该过度使用，而且一旦使用完毕，就应该关闭</mark>。

### 3.WebWorker的缺点
1. `Worker`内代码不能操作`DOM`（更新`UI`）
2. 不能跨域加载`JS`
3. 不是每个浏览器都支持这个新特性
4. 速度慢

### 4.WebWorker的应用
相关API：
+ `Worker`：构造函数，加载分线程执行的JS文件
+ `Worker.prototype.onmessage`：用于**接收**另一个线程的回调函数
+ `Worker.prototype.postMessage`：向另一个线程**发送**消息

```html 
<input type="text" id="number">
<button id="btn">计算</button>
<script>
  var input = document.getElementById('number');
  document.getElementById('btn').onclick = function () {
    var number = input.value;

    // 创建一个Worker对象
    var worker = new Worker('work.js');
    // 绑定接收消息的监听
    worker.onmessage = function (event) {
      console.log('主线程接收分线程返回的数据' + event.data);
    }

    // 向分线程发送消息
    worker.postMessage(number);
    console.log('主线程向分线程发送数据' + number);
  }
</script>
```
```JavaScript
// work.js文件
function fn (n) {
  return n <= 2 ? 1 : fn(n - 1) + fn(n - 2);
}
var onmessage = function (event) {
  console.log('分线程接收主线程发送的数据' + event.data);
  var result = fn(event.data);
  postMessage(result);
  console.log('分线程向主线程发送数据' + result);

}
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220329142438.png)
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220329142359.png)

**流程图：**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220329142107.png)

<mark>主线程中的`this`是`window`，而`work`线程中的`this`是一个专门为 `Worker` 定制的全局对象。</mark>
