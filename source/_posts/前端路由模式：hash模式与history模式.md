---
title: 前端路由模式：hash模式与history模式
date: 2022-07-27 20:26:00
categories:
- JavaScript
tags:
- JavaScript
---
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220830184521.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220830184721.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220830184805.png)
前端会通过使用不同的URL向用户展示不同的页面，使得当我们想要访问一个页面时只要知道其URL地址，再浏览器地址栏中输入便会看到这个页面。但是在平时也会存在输入确定的URL地址却无法访问到页面的情况（出现404页面）。这实际上与前端的路由模式息息相关。
### 0.为什么使用路由？
Ajax异步请求可以实现页面的无缝刷新，导致浏览器地址栏的URL不会发生任何变化，破坏了用户的浏览体验。同时本次浏览的页面内容使用URL访问无法重新复现。
使用路由就可以很好的解决这个问题。单页面应用利用JS动态切换网页内容，避免页面重载；路由则提供浏览器地址的变化。二者结合以后，页面内容发生变化时浏览器地址也发生变化。
> SPA（Single Page Application），即<b>单页面应用</b>。所谓单页 Web 应用，就是只有一张 Web 页面的应用。单页应用程序 (SPA) 是加载单个 HTML 页面并在用户与应用程序交互时动态更新该页面的 Web 应用程序。 浏览器一开始会加载必需的 HTML 、 CSS 和 JavaScript ，所有的操作都在这张页面上完成，都由 JavaScript 来控制。

前端路由有两种模式：hash模式和history模式

### 1.hash模式
hash模式就是一种把前端路由的路径用井号```#```拼接在真实URL后面的模式。当```#```后面的路径发生变化时，浏览器并不会重新发起请求，而是触发```hashchange```事件。

**示例**
新建一个```hash.html```文件，内容为：
```javascript
<a href='#/a'>A页面</a>
<a href='#/b'>B页面</a>
<div id="app"></div> 
<script>
  function render(){
    app.innerHTML = window.location.hash;
  }
  window.addEventListener('hashchange',render);
  render();
</script>
```
在上面的例子中，利用```a```标签设置了两个路由导航，把```app```当作视图渲染容器，当切换路由的时候触发视图容器的更新，这其实就是大多数前端框架哈希路由的实现原理。

>总结一下hash模式的优缺点：
>+ <b>优点：</b>浏览器兼容性较好，连IE8都支持
>+ <b>缺点：</b>路径在```#```后面，比较丑；所有的跳转都在客户端进行操作，这种模式不利于```SEO```优化；

<b style="background:yellow;">hash模式永远不会提交到服务器，只能在前端自生自灭。</b>

### 2. history模式  
history API是H5提供的新特性，允许开发者直接更改前端路由，即更新浏览器URL地址而不重新打起请求。

**示例：**
新建```history.html```，内容为：
```javascript
<a href="javascript:toA();">A页面</a>
<a href="javascript:toB();">B页面</a>
<div id="app"></div>
<script>
  function render(){
    app.innerHTML = window.location.pathname;
  }
  function toA(){
    history.pushState({},null,'/a');
    render();
  }
  function toB(){
    history.pushState({},null,'/b');
    render();
  }
  window.addEventListener('popstate',render);
</script>
```
history API提供了丰富的函数供开发者调用，例如：
```javascript
history.replaceState({},null,'/b') //替换路由
history.pushState({},null,'/a') //路由压栈
history.back() //返回
history.forward() //前进
history.go(-2) //后退2次
```
上面代码监听了```popstate```事件，该事件能监听到：
+ 用户点击浏览器的前进、后退操作
+ 手动调用history的```back```、```forward```和```go```方法

监听不到：
+ history的```pushState```和```replaceState```方法

这也是为什么上面的```toA```和```toB```函数内部需要手动调用```render```方法的原因。

大家可能也注意到```light-server```的命令多了```--historyindex '/history.html'```参数，这是干什么的呢？
浏览器在刷新的时候，会按照路径发送真实的资源请求，如果这个路径是前端通过history API设置的URL，那么在服务器往往不存在这个资源，于是就返回404。上面参数的意思是如果后端资源不存在就返回```history.html```的内容。

因此，线上部署基于history API单页面应用的时候，一定要后端配合支持才行，否则会出现大量的404。

>总结一下 history模式的优缺点：
>+ <b>优点：</b>路径比较正规，没有#
>+ <b>缺点：</b>兼容性不如hash，且需要服务端支持，否则一刷新就出现404

