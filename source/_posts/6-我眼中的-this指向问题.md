---
title: 【我眼中的】 - 【6】this指向问题
date: 2022-05-06 10:21:37
categories:
- 我眼中的系列
tags:
- 我眼中的系列
---
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220506102501.png)
> 重要的事情说三遍：<b style="color:red">this 永远指向最后调用它的那个对象</b>、<b style="color:red">this 永远指向最后调用它的那个对象</b>、<b style="color:red">this 永远指向最后调用它的那个对象</b>。
## 1. this的六道坎
*（偶然间读到一篇超级nice的博客，觉得比我之前写的棒多了，so，毫不犹豫的转载过来。[👉博客原文](https://blog.crimx.com/2016/05/12/understanding-this/)）*
> this is all about context.

说白了``` this ```就是找大佬，找拥有当前上下文（```context```）的对象（```context object```）。
大佬可以分为六层，层数越高权力越大，```this```只会认最大的。
#### 第一层：世界的尽头
权力最小的大佬是作为备胎的存在，在普通情况下就是全局，浏览器里就是````window````；在```use strict```的情况下就是```undefined```。
```javascript
function showThis () {
  console.log(this)
}

function showStrictThis () {
  'use strict'
  console.log(this)
}

showThis() // window
showStrictThis() // undefined
```

#### 第二层：点石成金
第二层大佬说白了就是找这个函数前面的点```.```。

如果用到```this```的那个函数是属于某个 ```context object``` 的，那么这个 ```context object``` 绑定到```this```。

比如下面的例子，```boss```是```returnThis```的 ```context object``` ，或者说```returnThis```属于```boss```。
```javascript
var boss = {
  name: 'boss',
  returnThis () {
    return this
  }
}

boss.returnThis() === boss // true
```
下面这个例子就要小心点咯，能想出答案么？
```javascript
var boss1 = {
  name: 'boss1',
  returnThis () {
    return this
  }
}

var boss2 = {
  name: 'boss2',
  returnThis () {
    return boss1.returnThis()
  }
}

var boss3 = {
  name: 'boss3',
  returnThis () {
    var returnThis = boss1.returnThis
    return returnThis()
  }
}

boss1.returnThis() // boss1
boss2.returnThis() // ?
boss3.returnThis() // ?
```
答案是```boss1```和```window```哦，猜对了吗。

只要看使用```this```的那个函数。

在```boss2.returnThis```里，使用```this```的函数是```boss1.returnThis```，所以```this```绑定到```boss1```；

在```boss3.returnThis```里，使用```this```的函数是```returnThis```，所以```this```绑定到备胎。

要想把```this```绑定到```boss2```怎么做呢？
```javascript
var boss1 = {
  name: 'boss1',
  returnThis () {
    return this
  }
}

var boss2 = {
  name: 'boss2',
  returnThis: boss1.returnThis
}

boss2.returnThis() //boss2
```
没错，只要让使用```this```的函数是属于```boss2```就行。

#### 第三层：指腹为婚
第三层大佬是```Object.prototype.call```和```Object.prototype.apply```，它们可以通过参数指定```this```。（注意```this```是不可以直接赋值的哦，```this = 2```会报```ReferenceError```。）

```javascript
function returnThis () {
  return this
}

var boss1 = { name: 'boss1' }

returnThis() // window
returnThis.call(boss1) // boss1
returnThis.apply(boss1) // boss1
```
#### 第四层：海誓山盟
第四层大佬是```Object.prototype.bind```，他不但通过一个新函数来提供永久的绑定，<b style="color:red">还会覆盖第三层大佬的命令【即：```bind（）```提供永久绑定，会覆盖```call()```和```apply()```】</b>。
```javascript
function returnThis () {
  return this
}

var boss1 = { name: 'boss1'}

var boss1returnThis = returnThis.bind(boss1)

boss1returnThis() // boss1

var boss2 = { name: 'boss2' }
boss1returnThis.call(boss2) // still boss1

```

#### 第五层：内有乾坤
一个比较容易忽略的会绑定```this```的地方就是```new```。当我们```new```一个函数时，就会自动把```this```绑定在新对象上，然后再调用这个函数。<b style="color:red">它会覆盖```bind()```的绑定</b>。
```javascript
function showThis () {
  console.log(this)
}

showThis() // window
new showThis() // showThis

var boss1 = { name: 'boss1' }
showThis.call(boss1) // boss1
new showThis.call(boss1) // TypeError

var boss1showThis = showThis.bind(boss1)
boss1showThis() // boss1
new boss1showThis() // showThis
```

#### 第六层：军令如山
最后一个法力无边的大佬就是 ```ES2015``` 的箭头函数。箭头函数里的```this```不再妖艳，被永远封印到当前词法作用域之中，称作 ```Lexical this``` ，在代码运行前就可以确定。没有其他大佬可以覆盖。

这样的好处就是方便让回调函数的```this```使用当前的作用域，不怕引起混淆。

所以对于箭头函数，只要看它在哪里创建的就行。
```javascript
function callback (cb) {
  cb()
}

callback(() => { console.log(this) }) // window

var boss1 = {
  name: 'boss1',
  callback: callback,
  callback2 () {
    callback(() => { console.log(this) })
  }
}

boss1.callback(() => { console.log(this) }) // still window
boss1.callback2(() => { console.log(this) }) // boss1

```
下面这种奇葩的使用方式就需要注意：
```javascript
var returnThis = () => this

returnThis() // window
new returnThis() // TypeError

var boss1 = {
  name: 'boss1',
  returnThis () {
    var func = () => this
    return func()
  }
}

returnThis.call(boss1) // still window

var boss1returnThis = returnThis.bind(boss1)
boss1returnThis() // still window

boss1.returnThis() // boss1

var boss2 = {
  name: 'boss2',
  returnThis: boss1.returnThis
}

boss2.returnThis() // boss2
```
如果你不知道最后为什么会是 boss2，继续理解<b style="color:red;"> “对于箭头函数，只要看它在哪里创建”</b>这句话。


## 2.改变```this```指向的方法
+ ES6 的箭头函数
+ 函数内部使用 ```_this = this```
+ 使用 ```apply```、```call```、```bind```
+ ```new``` 实例化一个对象

⭐ 箭头函数
<b>箭头函数的 this 始终指向函数定义时的 this，而非执行时。</b>
> 箭头函数需要记着这句话：“箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this，否则，this 为 undefined”。

```javascript
  var name = "windowsName";

  var a = {
      name : "Cherry",

      func1: function () {
          console.log(this.name)     
      },

      func2: function () {
          setTimeout( () => {
              this.func1()
          },100);
      }

  };

  a.func2()     // Cherry
```

⭐ 函数内部使用 ```_this = this```
```javascript
  var name = "windowsName";

  var a = {

      name : "Cherry",

      func1: function () {
          console.log(this.name)     
      },

      func2: function () {
          var _this = this;
          setTimeout( function() {
              _this.func1()
          },100);
      }

  };

  a.func2()       // Cherry
```
这个例子中，在 ```func2``` 中，首先设置 ```var _this = this;```，这里的 ```this``` 是调用 ```func2``` 的对象 ```a```，为了防止在 ```func2``` 中的 ```setTimeout``` 被 ```window``` 调用而导致的在 ```setTimeout``` 中的 ```this``` 为 ```window```。我们将 ```this```(指向变量 a) 赋值给一个变量 ```_this```，这样，在 ```func2``` 中我们使用 ```_this``` 就是指向对象 ```a``` 了。


