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
Symbol：代表创建后独一无二且不可变的数据类型【它的出现我认为是为了解决可能出现的全局变量冲突的问题】
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
#### 1.2.1 什么是数据？
● 存储在内存中的代表特定信息的东西，本质上是0101……
● 数据的特点：可传递、可运算
● 一切皆数据
● 内存中所有操作的目标：数据【*算术运算 、 *逻辑运算、 *赋值运算 、*运行函数】

#### 1.2.2 什么是内存？
● 内存条通电之后产生的可存储数据的空间（临时的）；
● 内存的产生和死亡：内存条（电路板） → 通电  → 产生内存空间  → 存储数据  → 处理数据  →  断电  →  内存空间和数据都消失；
● 一块小内存可以保存的2种数据：内部存储的数据 + 地址值
● 内存的分类：
  栈：全局变量和局部变量（空间较小）
  堆：对象（空间较大）
```javascript
var obj = {name:'Tom'};
console.log(obj.name);
```
上述代码在打印时，首先读取的是```obj```，此时读取的是```obj```的内容值，只不过内容值当中存储的是```obj```的地址值。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220228114525.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220228114817.png)

#### 1.2.3 什么是变量？
可变化的量，由变量名和变量值组成；
每个变量都对应一小块内存，变量名用来查找对应的内存，变量值就是内存中保存的数据
#### 1.2.4 内存、数据、变量三者之间的关系？
● 内存：用来存储数据的的空间
● 变量：是内存的标识

#### 1.2.5 相关问题：
##### ⭐ var a = xxx，a内存中到底保存的是什么？

* xxx是基本数据，保存的是这个数据
* xxx是对象，保存的是对象的地址值
* xxx是一个变量，保存的是xxx的内容值（可能是基本数据，也可能是地址值）

##### ⭐ 关于引用变量赋值问题？

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


##### ⭐ 在JS调用函数时传递变量参数，是值传递还是引用传递？

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

##### ⭐ JS引擎如何管理内存？

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
#### 1.3.1 什么是对象？
多个数据的封装体 或者 是用来保存多个数据的容器；
一个对象代表现实中的一个事物
```javascript
var person = {
  name:'Tom',
  age:13
}
```

#### 1.3.2 为什么用对象？
统一管理多个数据

#### 1.3.3 对象的组成
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

#### 1.3.4 如何访问对象内部数据？
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

##### ⭐ 什么情况下必须使用```['属性名']```的方式？

1.属性名包含特殊字符：- 、空格
2.属性名不确定（属性名是个变量）
```javascript
var p = {}
1.给p对象添加一个属性：content-type : text/json
p.content-type = 'text/json'  // 不能用
p['content-type'] = 'text/json' //可以使用

2.属性名不确定（属性名是个变量）
var propName = 'myAge';
var value = 18;
p.propName = value; //不能用
p[propName] = value; //可以使用
```

### 1.4 函数
#### 1.4.1 什么是函数？
实现特定功能的n条语句的封装体
只有函数是可以执行的，其他类型的数据不能执行

#### 1.4.2 为什么要用函数？
提高代码复用、便于阅读交流

#### 1.4.3 如何定义函数？
函数声明
表达式

```javascript
1.函数声明
function fn1(){
  console.log('fn1()');
}

2.表达式
var fn2 = function(){
  console.log('fn2()');
}
```

#### 1.4.4 如何调用（执行）函数？
直接调用：```test()```
通过对象调用：```obj.test()```
new调用：```new test()```
临时让test成为obj的方法进行调用：```test.call(obj) ```、``` test.apply(obj)```

```javascript
举例：
var obj = {};
function test(){
  this.xx = 'Hello';
}

* obj.test(); // 不能直接调用，因为obj中根本没有这个函数

test.call(obj) //可以让一个函数成为指定任意对象的方法进行调用
console.log(obj.xx) // Hello

```
#### 1.4.4 回调函数
##### 什么是回调函数？
3个特点：
1）自己定义的
2）没有调用
3）最终执行了（在某个时刻 或者 某个条件下）
##### 常见的回调函数
* dom事件的回调函数 ➡ this指的是发生事件的dom元素
* 定时器回调函数 ➡ this指的是window
* ajax请求回调函数
* 生命周期回调函数

#### 1.4.5 IIFE
Immediately-Invoked Function Expression，立即调用函数表达式
匿名函数自调用 = IIFE
```javascript
// 匿名函数自调用
(function(){
  console.log('Hello');
})()
```
<b> ♥ 作用：</b>

* 隐藏实现
* 不会污染外部（全局）命名空间
* 用它来编写js模块

```javascript
(function () {
  var a = 1;
  function test () {
    console.log(++a);
  }
  window.$ = function () { //向外暴露一个全局函数
    return {
      test: test
    }
  }
})();

$().test() // 1.$是一个函数 2.$执行后返回的是一个对象
```

#### 1.4.6 函数中的this
##### this 是什么？
* 任何函数本质上都是通过某个对象来调用的
* 所有函数内部都有一个变量this
* 它的值是调用函数的当前对象

##### 如何确定this的值？
* ```test()``` ：window
* ```p.test()``` ：p
* ```new test()``` ：新创建的对象
* ```p.call(obj)```：obj

>1. 函数调用时，指向window 
>2. 以方法调用时，指向调用该方法的对象 
>3. 使用new创建一个对象时，指向新创建的对象 
>4. call,apply ,bind可以改变this指向，this指向指定的那个对象
>5. 在全局作用域中this代表window

## 2.函数高级
### 2.1原型与原型链
#### 2.1.1 原型（prototype）
<b>1.函数的```prototype```属性</b>
* 每个函数都有一个```prototype```属性，它默认指向一个```Object```空对象（即称为：原型对象）
> 何为Object空对象？ 没有我们自己定义的属性
* 原型对象中有一个属性```constructor ```，它指向函数对象
```javascript
console.log(Date.prototype.constructor === Date); // true
console.log(fun.prototype.constructor === fun); //true（fun为自定义函数）
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220303112643.png)
⭐ 构造函数和它的原型对象是一个相互引用的关系。
解释相互引用：当前存在两个对象```A```和```B```，```A```当中有一个属性可以找到```B```，```B```当中有一个属性可以找到```A```。

<b>2.给原型对象添加属性（这里的属性一般都是方法）</b>
给原型对象添加属性（一般是方法）➡ 实例对象可以访问

```javascript
function Fun(){

}

//给原型对象添加属性（一般是方法）➡ 实例对象可以访问
Fun.prototype.test = function(){
  console.log('test方法');
}

var fun = new Fun();
fun.test();// test方法
```

#### 2.1.2 显式原型与隐式原型
1. 每个函数```function```都有一个```prototype```，即显式原型（属性）
2. 每个实例对象都有一个```__proto__```，可称为隐式原型（属性）
3. 对象的隐式原型的值为其对应构造函数的显式原型的值
4. 总结：
* 函数的```prototype```属性：在定义函数时自动添加的，默认为一个空Object对象
* 对象的```__proto__```属性：在创建对象时自动添加的，默认值为构造函数的```prototype```属性值
* 程序员能直接操作显式原型，但不能直接操作隐式原型（ES6之前）

```javascript
function Fn () { // 内部语句：this.prototype = {}

}

// 1. 每个函数function都有一个prototype，即显式原型（属性），默认指向一个空Object对象
console.log(Fn.prototype);

// 2.每个实例对象都有一个__proto__，可称为隐式原型（属性）
var fn = new Fn(); // 内部语句：this.__proto__ = Fn.prototype;
console.log(fn.__proto__);

//3.对象的隐式原型的值为其对应构造函数的显式原型的值
console.log(Fn.prototype === fn.__proto__) //true

// 给原型添加方法
Fn.prototype.test = function(){
  console.log('test');
}
// 通过实例对象调用原型的方法
fn.test()
```
上述代码内存结构图：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220304121321.png)

#### 2.1.3 原型链

访问一个对象的属性时：
* 先在自身属性中查找，如果找到则返回
* 如果未找到，再沿着```__proto__```这条链向上查找，找到返回
* 如果最终没有找到，返回```undefined```

原型链的别名是隐式原型链。
原型链的作用：查找对象的属性（方法）

<b style="background:#f8df70">原型链的本质是隐式原型链。</b>
原型链的尽头是```Object的原型对象```。

```javascript
function Fn () {
  this.test1 = function () {
    console.log('test1()');
  }
}

console.log(Fn.prototype);
Fn.prototype.test2 = function () {
  console.log('test2()');
}

var fn = new Fn()

fn.test1(); // test1()
fn.test2(); // test2()

console.log(fn.toString()); // [object Object]
console.log(fn.test3); // undefined
fn.test3(); //  "TypeError: fn.test3 is not a function
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220305205820.png)
原型链的尽头是```Object的原型对象```。


![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220305214943.png)
创建两个实例对象，实例对象有隐式原型属性```__proto__```，这个隐式原型属性指向的是Object的原型对象，（隐式原型属性```__proto__```的值是将```prototype```的值赋给它得到的）。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220306140152.png)
+ 所有函数的```__proto__```都是一样的，都是通过```new Function()```产生的
+ 任何函数都是通过```new Function()```产生的.因此，所有函数对象的隐式原型都指向```Function.prototype```

+ 实例对象的隐式原型属性等于构造函数的显示原型属性

#### 小结：
+ 函数的显式原型指向的对象：默认是空Object实例对象【但是Object不满足】
```javascript
function Fn () {
  this.test1 = function () {
    console.log('test1()');
  }
}

console.log(Fn.prototype instanceof Object); // true
console.log(Object.prototype instanceof Object); //false
console.log(Function.prototype instanceof Object); // true
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220307084657.png)
( 😀 因此，上图中存在绿色部分)

+ 所有函数都是Function的实例，包括Function它自身。
```javascript
console.log(Function.__proto__ === Function.prototype); // true
```
+ Object 的原型对象是原型链的尽头
```javascript
console.log(Object.prototype.__proto__); // null
```

#### 原型链的属性问题
+ 读取对象属性值时：会自动到原型链中查找
+ 设置对象的属性值时：不会查找原型链，如果当前对象中没有此属性，直接添加属性并设置其值
+ 方法一般定义在原型中，属性一般通过构造函数定义在对象本身上

※ <i style = "background:#a7ed3d">原型链是用来查找属性的，当为一个对象添加属性时，不会看原型链</i>

```javascript
function Fn () {

}
Fn.prototype.a = 'AAA';

var fn1 = new Fn();
console.log(fn1.a); // AAA

var fn2 = new Fn();
fn2.a = 'BBB';
console.log(fn1.a, fn2.a); // AAA BBB
```
此时```fn1```的内容为：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220307101504.png)
⭐ 查找```fn1```的```a```属性值时，发现```fn1```本身没有属性```a```，所以会自动到原型链中查找属性```a```。

此时```fn2```的内容为：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220307101627.png)
⭐ 查找```fn2```的```a```属性值时，发现```fn2```本身有属性```a```，所以直接输出该属性值，不再去查找原型链，但是原型链中实际上仍存在属性```a```。

```javascript
function Person (name, age) {
  this.name = name;
  this.age = age;
}
Person.prototype.setName = function (name) {
  this.name = name;
}

var p1 = new Person('Tom', 12);
console.log(p1);
p1.setName('Bob');
console.log(p1);
```
⭐ 方法一般定义在原型中，属性一般通过构造函数定义在对象本身上

```javascript
var p2 = new Person('Lily', 13)
console.log(p1.__proto__ === p2.__proto__) // true
```
⭐ 实例对象的隐式原型等于构造函数的显式原型

#### 2.1.4 instanceof
instanceof 作用：```a instanceof b``` → 判断```a```是否是```b```的实例
+ instanceof 是如何判断的？
表达式：```A instanceof B```
如果B函数的显式原型对象在A对象的原型链上，返回true，否则返回false

```javascript
function Foo () {

}
var f1 = new Foo();
console.log(f1 instanceof Foo); // true
console.log(f1 instanceof Object); // true
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220307113311.png)

<hr>

```javascript
console.log(Object instanceof Function); // true
console.log(Object instanceof Object); // true
console.log(Function instanceof Function); // true
console.log(Function instanceof Object); // true

function Foo () { }
console.log(Object instanceof Foo); // false
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/无标题.png)

#### 2.1.5 面试题
<b>题目1</b>

```javascript
function A () {

}
A.prototype.n = 1;

var b = new A();

A.prototype = {
  n: 2,
  m: 3
}

var c = new A()
console.log(b.n, b.m, c.n, c.m);// 1 undefined  2 3
```

上述代码在内存中的示意图：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220308130123.png)
（*最开始是是红色指示线，后来变为蓝色指示线*）

对于代码
```javascript
A.prototype = {
  n: 2,
  m: 3
}
``` 
的理解：现在堆内存中创建对象```{n:2,m:3}```，然后改变```A.prototype```的指向。（*蓝色指示线部分*）

<b>题目2</b>

```javascript
var F = function () {
  Object.prototype.a = function () {
    console.log('a()');
  }
  Function.prototype.b = function () {
    console.log('b()');
  }
}

var fn = new F();
fn.a(); // a()
fn.b(); // TypeError: fn.b is not a function
F.a(); // a()
F.b(); // b()
```

#### 原型链一张图

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/原型链.jpg)

<b>实记口诀：</b>

1. 函数都是```Function```构造出来的
2. 一切函数都是对象，只要是函数对象，就会有原型```prototype```和隐式原型```__proto__```两个属性
3. 普通对象身上只有```__proto__```，没有```prototype```
4. 实例化对象的```__proto__```都指向构造函数的```prototype```
5. 所有函数的```prototype```都指向自身的```prototype```
6. 所有的```prototype```的```__proto__```都指向```Object.prototype```（Object除外）
7. 所有函数对象的```__proto__```都指向```Function.prototype```（包括Function自身）
8. 所有对象身上都有```constructor```指向函数自身


### 2.2执行上下文与执行上下文栈
#### 2.2.1 变量提升与函数提升

```javascript
// 面试题：程序最终输出什么？
var a = 3;
function fn () {
  console.log(a);
  var a = 4;
}
fn(); // undefined
```
上述代码的实际执行过程是
```javascript
function fn(){
  var a;
  console.log(a);
  a = 4;
}
```
<hr>

```javascript
console.log(b); //undefined   
fn2(); // fn2

var b = 3;
function fn2 () {
  console.log('fn2');
}
```
上述代码在执行```console.log```时，变量```b```和函数```fn2```均未提前声明，但是仍然可以执行。原因是变量```b```进行了变量提升，函数```fn2```进行了函数提升。

1. 变量（声明）提升
通过```var```定义（声明）的变量，在定义语句之前就可以访问到，值为```undefined```
2. 函数（声明）提升
通过```function```声明的函数，在之前就可以直接调用，值为函数定义。
函数提升必须使用声明的方式。
```javascript
console.log(fn2);

function fn2 () {
  console.log('fn2');
}
```
结果为：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220309091249.png)

```javascript
console.log(fn3); //undefined

var fn3 = function () {
  console.log('fn3');
}

```
上述代码遵循的是变量提升。

#### 2.2.2 执行上下文
<b>1.代码分类</b>
全局代码 和 函数（局部）代码

<b>2.全局执行上下文</b>
在执行全局代码前将```window```确定为全局执行上下文。

对全局数据进行预处理:
* ```var```定义的全局变量 → ```undefined```，添加为```window```的属性
* ```function```声明的全局函数 → 赋值（```fun```）,添加为```window```的方法
* ```this``` → 赋值（```var```）
--上述过程是在全局代码执行之前就会进行的操作
* 执行全局代码

<b>3.函数执行上下文</b>
在调用函数时，准备执行函数体之前，创建对应的函数执行上下文对象（虚拟的，存在于栈中）

对局部数据进行预处理：
* 形参变量 → 赋值（实参） → 添加为执行上下文的属性
* ```arguments``` → 赋值（实参列表），添加为执行上下文的属性
* ```var```定义的局部变量 → undefined，添加为执行上下文的属性
* ```function```声明的函数 → 赋值（fun），添加为执行上下文的方法
* ```this``` → 赋值（调用函数的对象）

开始执行函数体代码

#### 2.2.3 执行上下文栈
1. 在全局代码执行前，JS引擎就会创建一个栈来存储管理所有的执行上下文对象
2. 在全局执行上下文（window）确定后，将其添加到栈中（压栈）
3. 在函数执行上下文创建后，将其添加到栈中（压栈）
4. 在当前函数执行完成后，将栈顶的对象移除（出栈）
5. 当所有的代码执行完后，栈中只剩下window

<span style="background-color:#f9c116"> 处于活动状态的执行上下文环境只有一个。</span>
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220309154025.png)

```javascript
// 1.进入全局执行上下文
var a = 10;
var bar = function (x) {
  var b = 5;
  fn(x + b);// 3. 进入foo执行上下文
}

var fn = function (y) {
  var c = 5;
  console.log(a + c + y);
}

bar(10);// 2.进入bar函数执行上下文
```
上述代码结果为30。

执行上下文栈结构图如下所示：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220309153021.png)

栈的底部始终是```window```，因为第一个产生的是```window```，要放入栈中进行管理。

#### 面试题
<b>⭐ 1.依次输出什么？整个过程中产生了几个执行上下文？</b>

```javascript
console.log('global begin:' + i);
var i = 1;
foo(1);
function foo (i) {
  if (i == 4) {
    return;
  }
  console.log('foo begin:' + i);
  foo(i + 1);
  console.log('foo end:' + i);
}
console.log('global end:' + i);
```
依次输出的结果：
>global begin:undefidned
>foo begin:1
>foo begin:2
>foo begin:3
>foo end:3
>foo end:2
>foo end:1
>global end:1

整个过程中产生了5个执行上下文。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220309163434.png)

<b>⭐ 2.函数提升与变量提升顺序</b>

```javascript
function a () { }
var a;
console.log(typeof a); // 'function'
```
上述代码先执行变量提升，再执行函数提升。
具体参考：[变量提升和函数提升的优先级问题]()
<b>⭐ 3.函数提升与变量提升顺序</b>

```javascript
if (!(b in window)) {
  var b = 1;
}
console.log(b); // undefined

```

### 2.3作用域与作用域链

### 2.4闭包



## 3.面向对象高级


## 4.线程机制与事件机制

## 补充问题

#### JS分号问题
* JS一条语句的后面可以不加分号
* 是否加分号是编码风格问题，没有应该不应该，只有开发者喜欢与否
* 下面两种情况不加分号会报错
※ 小括号开头的前一条语句 （匿名函数自调用）
※ 中方括号开头的前一条语句
```javascript
1.小括号开头的前一条语句 （匿名函数自调用）
var a = 3
;(function(){
  console.log('Hello')
})()
错误理解：
var a = 3(function(){
  console.log('Hello')
})()

---

2.中方括号开头的前一条语句
var a = 3
;[3,4].forEach(function(){
  console.log('Hello')
})
错误理解：
var a = 3[3,4].forEach(function(){
  console.log('Hello')
})
```
⭐解决方法：在行首加分号