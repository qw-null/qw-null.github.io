---
title: 6-我眼中的-this指向问题
date: 2022-05-06 10:21:37
categories:
- 我眼中的系列
tags:
- 我眼中的系列
---
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220506102501.png)
重要的事情说三遍：<b style="color:red">this 永远指向最后调用它的那个对象</b>、<b style="color:red">this 永远指向最后调用它的那个对象</b>、<b style="color:red">this 永远指向最后调用它的那个对象</b>。

例子一：
```javascript
    var name = "windowsName";

    function a() {
        var name = "Cherry";
        console.log(this.name);          // windowsName
        console.log("inner:" + this);    // inner: Window
    }

    a();
    console.log("outer:" + this)         // outer: Window
```
最后调用 ```a``` 的地方 ```a()```;，前面没有调用的对象那么就是全局对象 ```window```，这就相当于是 ```window.a()```；注意，这里我们没有使用严格模式，如果使用严格模式的话，全局对象就是 ```undefined```，那么就会报错 ```Uncaught TypeError: Cannot read property 'name' of undefined```。

例子二：
```javascript
  var name = "windowsName";
  var a = {
      name: "Cherry",
      fn : function () {
          console.log(this.name);      // Cherry
      }
  }
  a.fn();
```
最终，函数```fn```是被对象```a```调用，所以```this```的指向是对象```a```。

例子三：
```javascript
var name = "windowsName";
var a = {
    name: "Cherry",
    fn : function () {
        console.log(this.name);      // Cherry
    }
}
window.a.fn();
```
最终，函数```fn```是被对象```a```调用，所以```this```的指向是对象```a```。

例子四：
```javascript
    var name = "windowsName";
    var a = {
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // undefined
        }
    }
    window.a.fn();
```
最终，```this```的指向是对象```a```，但是对象```a```中没有```name```属性，所以返回值是```undefined```。

例子五：
```javascript
    var name = "windowsName";
    var a = {
        name : null,
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // windowsName
        }
    }

    var f = a.fn;
    f();
```
将对象```a```的```fn```方法赋值给```f```，但是此时并没有调用执行```fn```方法，最终是通过```f()```进行调用，而<b style="color:red">this 永远指向最后调用它的那个对象</b>，所以```this```的指向是对象```window```。

例子六：
```javascript
    var name = "windowsName";

    function fn() {
        var name = 'Cherry';
        innerFunction();
        function innerFunction() {
            console.log(this.name);      // windowsName
        }
    }

    fn()
```

## 改变```this```指向的方法
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


