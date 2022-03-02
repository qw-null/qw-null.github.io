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

+ undefined 代表定义未赋值
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
数据的类型：*基本类型  *对象类型
变量类型（变量内存值的类型）： 
*基本类型：保存基本类型的数据   
*引用类型：保存地址值

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220228110010.png)

对于上图中的变量```a```和```b```，因为其存储的是基本类型，所以数据直接保存在栈内存中，而对于变量```c```而言，其代表对象，因此在栈内存中只存储该对象在堆内存中的地址。


### 1.2 数据，变量与内存
1. 什么是数据？
● 存储在内存中的代表特定信息的东西，本质上是0101……
● 数据的特点：可传递、可运算
● 一切皆数据
● 内存中所有操作的目标：数据【*算术运算 、 *逻辑运算、 *赋值运算 、*运行函数】

2. 什么是内存？
● 内存条通电之后产生的可存储数据的空间（临时的）；
● 内存的产生和死亡：内存条（电路板） → 通电  → 产生内存空间  → 存储数据  → 处理数据  →  断电  →  内存空间和数据都消失；
● 一块小内存可以保存的2种数据：内部存储的数据 + 地址值
● 内存的分类：
  栈：全局变量和局部变量
  堆：对象

```javascript
var obj = {name:'Tom'};
console.log(obj.name);
```
上述代码在打印时，首先读取的是```obj```，此时读取的是```obj```的内容值，只不过内容值当中存储的是```obj```的地址值。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220228114525.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220228114817.png)

3. 什么是变量？
可变化的量，由变量名和变量值组成；
每个变量都对应一小块内存，变量名用来查找对应的内存，变量值就是内存中保存的数据

4. 内存、数据、变量三者之间的关系？
● 内存：用来存储数据的的空间
● 变量：是内存的标识

5. 相关问题：

<b>⭐ var a = xxx，a内存中到底保存的是什么？</b>

* xxx是基本数据，保存的是这个数据
* xxx是对象，保存的是对象的地址值
* xxx是一个变量，保存的是xxx的内容值（可能是基本数据，也可能是地址值）

<b>⭐ 关于引用变量赋值问题？</b>

● n个引用变量指向同一个对象，通过一个变量修改对象内部的数据，其他所有变量看到的是修改之后的数据。

● 2个引用对象指向同一个对象，让其中一个引用变量指向另一个对象，另一个引用变量依然指向前一个对象

```javascript
var obj1 = { name:'Tom' };
var obj2 = obj1;
```
上述代码中，实现了将```obj1```的内容保存的```obj2```【```obj1```中保存的是地址值】

```javascript
var obj1 = { name: 'Tom' };
var obj2 = obj1;
obj1.name = 'Jack';
console.log(obj2.name) // Jack

function fn(obj){
  obj.name = 'Bob';
}
fn(obj1);
console.log(obj2.name); // Bob

-----------

var a = {age:12};
var b = a;
a = {name:'Bob',age:13};
console.log(b.age,a.name,a.age); // 12 Bob 13

function fn2(obj){
  obj = {age:15};
}
fn2(a);
console.log(a.age); // 13
```
上述第二部分代码图示：

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220302084101.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220228154520.png)

代码执行完毕之后，```age:15```会在哪里？
变量```obj```是函数的局部变量，在函数执行完毕之后，局部变量会被释放，一旦局部变量```obj```被释放，那么其指向的```age:15```就会被当作垃圾对象回收。

```javascript
function fn3(obj){
  obj.name = 'Lily';
}
fn3(a);
console.log(a.name); // Lily
```
实际上只要是没有切断变量与堆内存中存储内容之间的执行，修改其中一个，其他都会跟着变。


<b>⭐ 在JS调用函数时传递变量参数，是值传递还是引用传递？</b>

答案：
+ 理解1：都是值传递，值分为两种：基本值和地址值。
+ 理解2：可能是值传递，也可能是引用传递（地址值）。

```javascript
var a = 3;
function fn (a) {
  a = a + 1;
}
fn(a);
console.log(a); // 3
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220302091425.png)

<b>⭐ JS引擎如何管理内存？</b>

1.内存生命周期
* 分配小内存空间，得到它的使用权
* 存储数据，可以反复进行操作
* 释放小内存空间

2.释放内存
* 局部变量：函数执行完自动释放
* 对象：成为垃圾对象，由垃圾回收器回收

```javascript
var a = 3;
var obj = {};
```
上述代码中共使用几个内存空间？
3个内存空间。一个是栈内存a，存储数据3；一个是栈内存obj，存储数据{}（堆内存）的地址；另一个是堆内存中存储的{}。

<hr>

```javascript
var a = 3;
var obj = {};
obj = null;
```
上述代码中共使用几个内存空间？
2个内存空间。一个是栈内存a，存储数据3；一个是栈内存obj，存储值为null。堆内存中存储的{}被回收。

<hr>

```javascript
function fn(){
  var b = {};
}

fn();
```
上述代码中的b在函数```fn()```执行完毕之后会自动释放，而b指向的对象是在后面的某个时刻由垃圾回收器回收。

### 1.3 对象
1. 什么是对象？
多个数据的封装体 或者 是用来保存多个数据的容器；
一个对象代表现实中的一个事物
```javascript
var person = {
  name:'Tom',
  age:13
}
```

2. 为什么用对象？
统一管理多个数据

3. 对象的组成
属性 + 方法
♥ 属性：属性名（字符串）和属性值（任意类型）组成
♥ 方法：是一种特别的属性（属性值是函数）

```javascript
var person = {
  name: 'Tom',
  age: 13,
  setName: function (name) {
    this.name = name;
  },
  setAge: function (age) {
    this.age = age;
  }
}

console.log(person.name, person.setName)
```
输出结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220302110213.png)

4. 如何访问对象内部数据？
※ 第一种方式：```.属性名```，编码简单，有时不能用
```javascript
p.setName('Bob');
p.name;
```
※ 第二种方式：```['属性名']```，编码麻烦，能通用
```javascript
person['setAge'](23);
p['age'];
```

<b>⭐ 什么情况下必须使用```['属性名']```的方式？</b>

1.属性名包含特殊字符：- 、空格
2.属性名不确定
```javascript
var p = {}
1.给p对象添加一个属性：content-type : text/json
p.content-type = 'text/json'  // 不能用
p['content-type'] = 'text/json' //可以使用

2.属性名不确定
var propName = 'myAge';
var value = 18;
p.propName = value; //不能用
p[propName] = value; //可以使用
```

### 1.4 函数
1. 什么是函数？

2. 为什么要用函数？

3. 如何定义函数？


4. 如何调用（执行）函数？


## 2.函数高级


## 3.面向对象高级


## 4.线程机制与事件机制