---
title: JavaScript中with的用法
date: 2022-03-23 09:39:45
categories:
- JavaScript
tags:
- JavaScript
---

> ⚠ 警告：由于with语句影响性能且难于调试其中的代码，通常不推荐在产品代码中使用with语句。

但是这个东东，不是用来做项目的，而是用来做面试题的。

## 1. with简介
with语句的用途是将代码作用域设置为特定的对象，语法是：```with(expression) statement```
使用with语句的主要场景是针对一个对象反复操作，此时将代码的作用域设置为该对象能提供便利。
例如：
```javascript
var person = {
  name:'Tom',
  age:'13',
  school:'ucas'
}
```
如果修改```person```的每一项属性值，一般做法是：
```javascript
person.name = 'Jack';
person.age = 15;
person.school = 'SDAU';
// 上述代码中重复写了3次person
```
使用```with```的写法，会更加简洁
```javascript
with(person){
  name = 'Jack';
  age = 15;
  school = 'SDAU';
}
```
## 2.with弊端
* 数据泄露
* 性能下降

### 2.1 数据泄露
```javascript
function fn (obj) {
  with (obj) {
    a = 2;
  }
}

var obj1 = {
  a: 3
}
var obj2 = {
  b: 4
}

// console.log(a); //ReferenceError: a is not defined

fn(obj1);
console.log(obj1.a); // 2

fn(obj2);
console.log(obj2.a); // undefined

console.log(a);// 2，a被泄漏到全局作用域上
```
上述代码中创建了```obj1``` 、```obj2```两个对象，其中```obj1```中有属性```a```，```obj2```没有属性```a```。```fn(obj) ```函数接受一个 ```obj``` 的形参，该参数是一个对象引用，并对该对象执行了 ```with(obj) {...}```。将```obj1```传递进去，```a = 2```赋值操作找到```obj.a```并将2赋值给它。当```obj2```传递进去时，但是并没有属性```a```，因此不会创建这个属性，所以```obj2.a```的值为```undefined```。
⭐ 为什么对```obj2```的操作会导致数据泄露？
当传递```obj2```给```with```时，```with```所声明的作用域是```obj2```，从这个作用域开始对```a```进行LHS查询：```obj2```的作用域➡```fn()```的作用域➡全局作用域，以上中都没有找到```a```，在<b>非严格模式下</b>，会在全局作用域中创建一个全局变量```a```。在<b>严格模式下</b>，会抛出```ReferenceError``` 异常。

### 2.2 性能下降
```javascript
function func () {
  console.time("func");
  var obj = {
    a: [1, 2, 3]
  };
  for (var i = 0; i < 100000; i++) {
    var v = obj.a[0];
  }
  console.timeEnd("func");
}
func();

function funcWith () {
  console.time("funcWith");
  var obj = {
    a: [1, 2, 3]
  };
  with (obj) {
    for (var i = 0; i < 100000; i++) {
      var v = a[0];
    }
  }
  console.timeEnd("funcWith");
}

funcWith();
```
结果为：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220323113947.png)

原因是什么？
JavaScript 引擎会在编译阶段进行数项的性能优化，其中有些优化依赖于能够根据代码的词法进行静态分析，并预先确定所有变量和函数的定义位置，才能在执行过程中快速找到标识符。

但如果引擎在代码中发现了 with，它只能简单地假设关于标识符位置的判断都是无效的，因为无法知道传递给 with 用来创建新词法作用域的对象的内容到底是什么。

最悲观的情况是如果出现了 with ，所有的优化都可能是无意义的。因此引擎会采取最简单的做法就是完全不做任何优化。如果代码大量使用 with 或者 eval()，那么运行起来一定会变得非常慢。无论引擎多聪明，试图将这些悲观情况的副作用限制在最小范围内，也无法避免如果没有这些优化，代码会运行得更慢的事实。