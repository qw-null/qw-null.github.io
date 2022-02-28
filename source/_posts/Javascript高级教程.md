---
title: Javascript高级教程
date: 2022-02-25 09:36:15
categories:
- JavaScript
tags:
- JavaScript
---

[尚硅谷JavaScript高级教程(javascript实战进阶)](https://www.bilibili.com/video/BV14s411E7qf?share_source=copy_web)

## 1.基础总结深入
### 1.1数据类型
两大类：1.基本（值）类型  2.对象（引用）类型
1. 基本（值）类型
String：任意字符串
Number：任意数字
Symbol
Boolean：true / false
Undefined：undefined
Null：null

2. 对象（引用）类型
Object：任意对象
Function：一种特别的对象（特别在可以执行）
Array：一种特别的对象（特别在属性为数值下标，内部数据是有序的）

3. 判断数据类型
* typeof  ： 可以判断：undefined / 数值 / 字符串 / 布尔值
* typeof  ： 不可以判断：<b style="background:yellow;">null与object、object与array</b>
* instanceof ：判断对象的具体类型
* \===  ： 可以判断：undefined / null （因为这两者只有一个数值）

```javascript
// 1.基本数据类型
// typeof 返回数据类型的字符串表达

//Undefined
var a;
console.log(a, typeof a, typeof a === 'undefined', a === undefined); // undefined  'undefined'  true  true
console.log(undefined === 'undefined');// false

// Number
a = 3;
console.log(typeof a === 'number'); // true

//String
a = 'angle';
console.log(typeof a === 'string'); // true

// Boolean
a = true;
console.log(typeof a === 'boolean'); // true

// Null
a = null;
console.log(typeof a); // object
console.log(a === null);// true
```

```javascript
// 2.对象数据类型
var b1 = {
  b2: [1, 'abc', console.log],
  b3: function () {
    console.log('b3')
  }
}
console.log(b1 instanceof Object); // true
console.log(b1.b2 instanceof Array, b1.b2 instanceof Object);// true true
console.log(b1.b3 instanceof Function, b1.b3 instanceof Object);// true true

console.log(typeof b1.b3 === 'function'); //true
```

为什么```b1.b3 instanceof Object``` 是 ```true```，这是因为函数是一种特殊的对象。

对于上述定义中的```b2```的第三个元素```console.log```进行类型判断。

```javascript
console.log(typeof b1.b2[2] === 'function');//true
b1.b2[2](4);// 4
```
#### 1.undefined与null的区别是什么？
+ undefiddned 代表定义未赋值
+ null表示定义了变量并且赋值为null
#### 2.什么时候给变量赋值为null?
+ 初始赋值为null，表明将要赋值为对象。
+ 结束前，让对象成为垃圾对象（被垃圾回收器回收）


```javascript
// 起始
var b = null; // 初始赋值为null，表明将要赋值为对象

// 确定对象就要赋值
b = ['xiaoming',12];

// 最后
b = null; // 释放对象：让b指向的对象成为垃圾对象（被垃圾回收器回收）
```
#### 3.严格区别变量类型与数据类型？
数据的类型：



### 1.2 对象，变量与内存

### 1.3 对象

### 1.4 函数

## 2.函数高级


## 3.面向对象高级


## 4.线程机制与事件机制