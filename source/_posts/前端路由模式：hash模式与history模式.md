---
title: 前端路由模式：hash模式与history模式
date: 2022-03-27 20:26:00
categories:
- JavaScript
tags:
- JavaScript
---
前端会通过使用不同的URL向用户展示不同的页面，使得当我们想要访问一个页面时只要知道其URL地址，再浏览器地址栏中输入便会看到这个页面。但是在平时也会存在输入确定的URL地址却无法访问到页面的情况（出现404页面）。这实际上与前端的路由模式息息相关。

## 🎸 0.为什么使用路由？
Ajax异步请求可以实现页面的无缝刷新，导致浏览器地址栏的URL不会发生任何变化，破坏了用户的浏览体验。同时本次浏览的页面内容使用URL访问无法重新复现。
使用路由就可以很好的解决这个问题。单页面应用利用JS动态切换网页内容，避免页面重载；路由则提供浏览器地址的变化。二者结合以后，页面内容发生变化时浏览器地址也发生变化。
> SPA（Single Page Application），即<b>单页面应用</b>。所谓单页 Web 应用，就是只有一张 Web 页面的应用。单页应用程序 (SPA) 是加载单个 HTML 页面并在用户与应用程序交互时动态更新该页面的 Web 应用程序。 浏览器一开始会加载必需的 HTML 、 CSS 和 JavaScript ，所有的操作都在这张页面上完成，都由 JavaScript 来控制。

## 🎸1.URL地址介绍
URL属性介绍：
| 属性| 含义 |
|:-----|:-----|
| location.protocal|协议|
| location.hostname|主机名|
| location.host|主机|
| location.port|端口号|
| location.patchname|访问页面|
| location.search|搜索内容|
| location.hash|哈希值|

举例：
```javascript
以网址 "http://127.0.0.1:8080/01-hash.html?a=1&b=2#/aaa/bbb" 为例
location.protocal ➡ 'http:'
location.hostname ➡ '127.0.0.1:'
location.host ➡ '127.0.0.1:8080:'
location.port ➡ '8080:'
location.patchname ➡ '01-hash.html:'
location.search ➡ '?a=1&b=2'
location.hash ➡ '#/aaa/bbb'
```


## 🎸 2.前端路由简介
前端路由只有两种模式：```hash```模式和```history```模式。

### 😺2.1 ```hash```模式
hash模式是一种把前端路由的路径使用<b> # </b>拼接在真实URL后面的模式。当<b> # </b>后的路由路径发生改变时，浏览器不会发起新的请求，而是触发```onhashchange```事件。

#### 2.1.1 特点
+ ```hash```变化会触发网页跳转，即浏览器的前进和后退。
+ ```hash```可以改变```URL```，但是不会触发页面重新加载（hash的变化是记录在```window.history```中），即不会刷新页面。因此所有的跳转都是在客户端进行操作，不算是一次 ```http``` 请求，所以这种模式不利于 ```SEO``` 优化。
+ ```hash```通过```window.onhashchange```的方式监听```hash```的改变，由此实现无刷新跳转。
+ <b>```hash```永远不会提交到服务器，只能在前端自生自灭</b>。

### 🐯2.2 ```history```模式
history模式是利用```history API```实现URL地址改变，网页内容随之改变。```history API```是由``` H5```提供的新特性，允许开发者直接改变前端路由，即更新浏览器URL地址而<b>不重新发起请求</b>。

#### 2.2.1 概述
+ ```window.history```属性指向```History```对象，它表示当前窗口的浏览历史。当发生改变时，只会改变页面的路径，不会刷新页面。
+ ```History```对象保存了当前窗口访问过的所有页面网址。
+ 由于安全原因，浏览器不允许脚本读取这些地址，但是允许在地址之间导航。
+ 浏览器工具栏的“前进”和“后退”按钮，其实就是对 ```History``` 对象进行操作。

#### 2.2.2 特点 & 存在问题
对于 ```history``` 来说，主要有以下特点：
+ 新的 ```url``` 可以是与当前 ```url``` 同源的任意 ```url``` ，也可以是与当前 ```url``` 一样的地址，但是这样会导致的一个问题是，会把重复的这一次操作记录到栈当中。
+ 通过 ```history.state``` ，添加任意类型的数据到记录中。
+ 可以额外设置 ```title``` 属性，以便后续使用。
+ 通过 ```pushState``` 、 ```replaceState``` 来实现无刷新跳转的功能。

对于 ```history``` 来说，确实解决了不少 ```hash``` 存在的问题，但是也带来了新的问题。具体如下：
+ 使用 ```history``` 模式时，在对当前的页面进行刷新时，此时浏览器会重新发起请求。如果 ```nginx``` 没有匹配得到当前的 url ，就会出现 404 的页面。
+ 在使用 ```history``` 模式时，需要<b>通过服务端来允许地址可访问</b>，如果没有设置，就很容易导致出现 404 的局面。

#### 2.2.3 属性
History对象主要有两个属性：```length```和```state```。
+ ```History.length```：当前窗口访问过的网址数量（包括当前网页）
+ ```History.state```：History 堆栈最上层的状态值，通常是 ```undefined```，即未设置。

#### 2.2.4 方法
5个方法：```History.back()```、```History.forward()```、```History.go()```、```History.pushState()```、```History.replaceState()```
+ ```History.back()```：移动到上一个网址，等同于点击浏览器的后退键。对于第一个访问的网址，该方法无效果。
+ ```History.forward()```：移动到下一个网址，等同于点击浏览器的前进键。对于最后一个访问的网址，该方法无效果。
+ ```History.go()```：接受一个整数作为参数，以当前网址为基准，移动到参数指定的网址。如果参数超过实际存在的网址范围，该方法无效果；如果不指定参数，默认参数为0，相当于刷新当前页面。```history.go(-1)```相当于```history.back()```、```history.go(1)```相当于```history.forward()```、```history.go(0)```表示刷新当前页面。
+ ```History.pushState()```：该方法用于在历史记录中添加一条记录。执行该方法时不会触发刷新，只是会导致```History```对象发生变化，地址栏会有变化。
语法：```history.pushState(object, title, url)```
该方法接受三个参数，依次为：
+ + ```object```：是一个对象，通过 pushState 方法可以将该对象内容传递到新页面中。如果不需要这个对象，此处可以填 null。
+ + ```title```：指标题，几乎没有浏览器支持该参数，传一个空字符串比较安全。
+ + ```url```：新的网址，必须与当前页面处在同一个域。不指定的话则为当前的路径，如果设置了一个跨域网址，则会报错。

```javascript
var data = { foo: 'bar' };
history.pushState(data, '', '2.html');
console.log(history.state) // {foo: "bar"}
```
<b>注意：</b>如果 ```pushState``` 的 ```URL``` 参数设置了一个新的锚点值（即 ```hash```），并不会触发 ```hashchange``` 事件。反过来，如果 ```URL``` 的锚点值变了，则会在 ```History``` 对象创建一条浏览记录。
如果 pushState() 方法设置了一个跨域网址，则会报错。

```javascript
// 报错
// 当前网址为 http://example.com
history.pushState(null, '', 'https://baidu.com/hello');
```
上面代码中，```pushState``` 想要插入一个跨域的网址，导致报错。这样设计的目的是，防止恶意代码让用户以为他们是在另一个网站上，因为这个方法不会导致页面跳转。

+ ```History.replaceState()```：该方法用来修改 History 对象的当前记录，用法与 pushState() 方法一样。

```javascript
当前网页网址是 example.com/example.html。

history.pushState({page: 1}, '', '?page=1')
// URL 显示为 http://example.com/example.html?page=1

history.pushState({page: 2}, '', '?page=2');
// URL 显示为 http://example.com/example.html?page=2

history.replaceState({page: 3}, '', '?page=3');
// URL 显示为 http://example.com/example.html?page=3

history.back()
// URL 显示为 http://example.com/example.html?page=1

history.back()
// URL 显示为 http://example.com/example.html

history.go(2)
// URL 显示为 http://example.com/example.html?page=3

```