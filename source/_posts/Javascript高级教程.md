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
IIFE（ 立即调用函数表达式）是一个在定义时就会立即执行的 JavaScript 函数。
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
原型链的作用：查找对象的属性（方法），只是用来查找，如果是一些赋值操作等，则不会查找原型链。

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
instanceof 作用：```a instanceof b``` → 判断```a```是否是```b```的实例【其中，a是实例对象，b是构造函数】
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

A.prototype = { //这步操作直接改变了原型对象
  n: 2,
  m: 3
}

var c = new A()
console.log(b.n, b.m, c.n, c.m);// 1 undefined  2 3
```

上述代码在内存中的示意图：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220308130123.png)
（*最开始是是红色指示线，后来变为蓝色指示线*）

两种表达方式：
```javascript
var B = function(){}
表达方式1：
B.prototype.n = 1;
表达方式2：
B.prototype = { 
  n: 2,
  m: 3
}
```
其中表达方式1会影响原有的实例对象，因为表达方式1是在原有的实例对象上添加到。
表达方式2不会影响原有的实例对象，因为表达方式2直接改变了实例对象原型。



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
执行上下文个数遵循 ``` n + 1 ```的原则，其中```n```指的是调用函数的次数，```1```指的是```window```。

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
上述代码先执行函数提升，再执行变量提升，但是变量未进行赋值操作，所以是function。
具体参考：[变量提升和函数提升的优先级问题](https://qw-null.github.io/2022/03/09/%E5%8F%98%E9%87%8F%E6%8F%90%E5%8D%87%E5%92%8C%E5%87%BD%E6%95%B0%E6%8F%90%E5%8D%87%E7%9A%84%E4%BC%98%E5%85%88%E7%BA%A7%E9%97%AE%E9%A2%98/)

```javascript
if (!(b in window)) {
  var b = 1;
}
console.log(b); // undefined

```

```javascript
var c = 1;
function c (c) {
  console.log(c);
}
c(2); // TypeError: c is not a function
```
上述代码相当于：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220311102431.png)

### 2.3作用域与作用域链
#### 2.3.1 作用域
1. 理解
作用域就像是一块“地盘”，一个代码所在的区域。它是静态的（相对于 执行上下文对象 而言，因为执行上下文对象只有代码执行时才会产生），在编写代码时就确定了。
2. 分类
全局作用域、函数作用域、块级作用域（ES6出现）
3. 作用
隔离变量，不同作用域下同名变量不会有冲突
4. 作用域个数
作用域个数遵循``` n + 1 ```的原则（与执行上下文相同），其中```n```指的是定义的函数的个数，```1```指的是全局作用域。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220314203108.png)
结果为：
fn() 100 20 300 10
bar() 1000 20 300 400 100
bar() 1000 20 300 400 200

#### 2.3.2 作用域与执行上下文
1. 区别：
⭐全局作用域之外，每个函数都会创建自己的作用域。作用域在函数定义时就已经确定了，而不是在函数调用时。全局执行上下文环境是在全局作用域确定之后，JS代码马上执行之前创建；函数执行上下文环境是在调用函数时，函数代码执行之前创建。
⭐作用域是静态的，只要函数定义好了就一直存在，且不会再变化。执行上下文是动态的，调用函数时创建，函数调用结束时就会自动释放。
2. 联系
执行上下文（对象）是从属于所作用域。全局上下文环境 → 全局作用域；函数上下文环境 → 对应的函数作用域。

#### 2.3.3 作用域链
1. 理解
多个上下级关系的作用域形成的链，它的方向是从下向上（从内到外）；查找变量时就是沿着作用域链来查找的
2. 查找一个变量的查找规则
① 在当前作用域下的执行上下文中查找对应的属性，如果有直接返回，否则进入②；
② 在上一级作用域的执行上下文中查找对应的属性，如果有直接返回，否则进入③；
③ 在执行②的相同操作，直到全局作用域，如果还找不到就排除找不到的异常。

```javascript
var a = 1;
function fn1 () {
  var b = 2;
  function fn2 () {
    var c = 3;
    console.log(c);
    console.log(b);
    console.log(a);
    console.log(d);
  }
  fn2()
}
fn1();
```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220314230130.png)

#### 2.3.4 面试题
⭐
```javascript
var x = 10;
function fn () {
  console.log(x);
}
function show (f) {
  var x = 20;
  f();
}
show(fn); // 10
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220315140359.png)
执行函数```fn()```时，会先在```fn```的作用域中查找```x```，没有找到后会跳到其外部的作用域中继续寻找，```fn```的作用域和```show```的作用域是同级的不会相互查找。
⭐
```javascript
var fn = function () {
  console.log(fn);
}
fn();

var obj = {
  fn2: function () {
    console.log(fn2);
  }
}

obj.fn2();
```
结果为：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220315142215.png)
报错的原因是
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220315142435.png)
在函数作用域中查找不到```fn2```，会到全局作用域中继续查找，在全局作用域中不存在```fn2```。
※ 如果想要调用```fn2```，应该修改代码为
```javascript
var obj = {
  fn2: function () {
    console.log(this.fn2);
  }
}

obj.fn2(); // 结果为打印fn2
```
### 2.4闭包
1. 如何产生闭包？
当一个嵌套的内部（子）函数引用了嵌套的外部（父）函数的变量（函数）时，就产生了闭包。
2. 闭包是什么？
闭包指的是那些引用了另一个函数作用域中变量的函数，通常是在嵌套函数中实现的。
>使用chrome调试查看：
※ 理解一：闭包是嵌套的内部函数（该函数中引用了外部函数的变量）
※ 理解二：包含被引用变量（函数）的对象
<b>注意：</b>闭包存在于嵌套的内部函数中

3. 产生闭包的条件？
* 函数嵌套
* 内部函数引用了外部函数的数据（变量 \ 函数）
* 执行了外部函数

#### 2.4.1 常见的闭包
1. 将函数作为另一个函数的返回值
```javascript
function fn1 () {
  var a = 2;
  function fn2 () {
    a++;
    console.log(a);
  }
  return fn2;
}
var f = fn1();
console.log(f);
```
结果为
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220315154451.png)
上述过程中<b>创建了1个闭包</b>。通过代码```fn1()```产生。
再执行下列代码：
```javascript
f(); // 3
f(); // 4
```
2. 将函数作为实参传递给另一个函数调用
```javascript
function showDelay (msg, time) {
  setTimeout(function () {
    console.log(msg)
  }, time)
}

showDelay('延迟输出', 2000)
```
上述代码在执行过程中产生了闭包。因为
嵌套的内部函数引用了外部函数的变量```msg```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220315160727.png)
若将代码修改如下，就不会产生闭包
```javascript
function showDelay (msg, time) {
  setTimeout(function () {
    console.log('无闭包产生')；
  }, time)
}

showDelay('延迟输出', 2000)
```

#### 2.4.2 闭包的作用

1. 使得函数内部的变量在函数执行完后，仍存活在内存中（延长了局部变量的生命周期）

2. 让函数外部可以操作（读写）到函数内部的数据（变量/函数）

问题：
<b>※ 1. 函数执行完后，函数内部声明的局部变量是否还在？</b>
一般是不存在，存在于闭包中的变量才可能存在

<b>※ 2. 在函数外部能直接访问函数内部的局部变量吗？</b>
不能，但是通过闭包可以让外部操作它

#### 2.4.3 闭包的生命周期
1. 产生：在嵌套内部函数定义执行完时就产生了（不是在调用时）
2. 死亡：在嵌套的内部函数成为垃圾对象时 

```javascript
function fn1 () {
  // 此时闭包就已经产生了（函数提升，内部函数对象已经创建了）
  var a = 2;
  function fn2 () {
    a++;
    console.log(a);
  }
  return fn2;
}

var f = fn1();
f(); // 3
f(); // 4
f = null;// 闭包死亡（包含必报的函数对象成为垃圾对象）
```
#### 2.4.3 闭包的应用：定义JS模块
JS模块是具有特定功能的JS文件，将所有的数据和功能都封装在一个函数内部（私有的），只向外暴露一个包含n个方法的对象或者函数，模块的使用者，只需要通过模块暴露的对象调用方法来实现对应的功能。

<b>※ 方式一</b>

```javascript
function myModule () {
  // 私有数据
  var msg = "My module";

  // 操作数据的对象
  function doSomething () {
    console.log('doSomething()', msg.toUpperCase());
  }
  function doOtherthing () {
    console.log('doOtherthing()', msg.toLowerCase());
  }

  // 向外暴露对象（给外部使用的方法）
  return {
    doSomething: doSomething,
    doOtherthing: doOtherthing
  }
}
```
<b>※ 方式二</b>

```javascript
(function () {
  // 私有数据
  var msg = "My module";

  // 操作数据的对象
  function doSomething () {
    console.log('doSomething()', msg.toUpperCase());
  }
  function doOtherthing () {
    console.log('doOtherthing()', msg.toLowerCase());
  }

  // 向外暴露对象（给外部使用的方法）
  window.myModule = {
    doSomething: doSomething,
    doOtherthing: doOtherthing
  }
})()
```

#### 2.4.4 闭包的缺点及解决
1. 缺点：
■ 函数执行完后，函数内的局部变量没有释放，占用内存时间会变长
■ 容易造成内存泄漏 【内存泄漏：内存占用却不使用】
2. 解决：
■ 能不用闭包就不用
■ 及时释放

```javascript
function fn1 () {
  var arr = new Array[100000];
  function fn2 () {
    console.log(arr.length);
  }
  return fn2;
}

var f = fn1();
f();

f = null;// 让内部函数成为垃圾对象 --> 回收闭包
```
补充知识点：[内存溢出与内存泄漏](https://qw-null.github.io/2022/03/20/%E5%86%85%E5%AD%98%E6%BA%A2%E5%87%BA%E4%B8%8E%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F/)
#### 2.4.5 面试题

<b>⭐ 题目 1</b>

```javascript
var name = 'This Window';
var object = {
  name: 'My Object',
  getNameFunc: function () {
    return function () {
      return this.name;
    }
  }
};
console.log(object.getNameFunc()());
```
输出的结果为```This Window```，原因是函数调用时，```this```指向```window```，因此```this.name```指的是全局变量```name```的值。

```javascript
var name = 'This Window';
var object = {
  name: 'My Object',
  getNameFunc: function () {
    var that = this;
    return function () {
      return that.name;
    }
  }
};
console.log(object.getNameFunc()());
```
输出的结果为```My Object```，原因是```this```是在对象```object```的方法```getNameFunc```中进行调用的，以方法调用时，```this```指向调用该方法的对象，因此```that.name```相当于```object.name```。


<b>⭐ 题目 2</b>

```javascript
function fun (n, o) {
  console.log(o);
  return {
    fun: function (m) {
      return fun(m, n);
    }
  }
}
var a = fun(0); // undefidned
a.fun(1); a.fun(2); a.fun(3);// ? ? ?

var b = fun(0).fun(1).fun(2).fun(3);// undefined ? ? ?

var c = fun(0).fun(1);// undefined ?
c.fun(2); c.fun(3);// ? ?
```
结果为：
```
undefidned 0 0 0
undefidned 0 1 2
undefidned 0 1 1
```




## 3.面向对象高级
### 3.1 对象创建模式
⭐ 方式一：Object构造函数模式
+ 套路：先创建空```Object```对象，再动态添加属性/方法
+ 使用场景：起始时不确定对象内部数据
+ 问题：语句太多

```javascript
// 一个人：name:"Tom",age:12
var p = new Object();
p.name = 'Tom';
p.age = 12;
p.setName = function (name) {
  this.name = name;
}

// 测试
p.setName('Jack');
console.log(p);
```
结果为
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220321112724.png)

⭐ 方式二：对象字面量模式
+ 套路：使用{}创建对象，同时指定属性/方法
+ 使用场景：起始时对象内部数据是确定的
+ 问题：如果创建多个对象，有重复代码

```javascript
// 一个人：name:"Tom",age:12
var p = {
  name: 'Tom',
  age: 12,
  setName: function (name) {
    this.name = name;
  }
}

// 测试
p.setName('Jack');
console.log(p.name, p.age); // Jack 12
```

⭐ 方式三：工厂模式
+ 套路：通过工厂函数动态创建对象并返回
+ 适用场景：需要创建多个对象
+ 问题：对象没有一个具体的类型，都是object类型

※ 何为工厂函数？  —— 返回一个对象的函数成为工厂函数


```javascript
function createPerson (name, age) {
  var obj = {
    name: name,
    age: age,
    setName: function (name) {
      this.name = name;
    }
  }
  return obj;
}

// 创建2个人
var p1 = createPerson('Tom',12);
var p2 = createPerson('Jack',14);
```

⭐ 方式四：自定义构造函数模式
+ 套路：自定义构造函数，通过new创建对象
+ 适用场景：需要创建多个类型确定的对象
+ 问题：每个对象都有相同的数据，浪费内存

```javascript
function Person (name, age) {
  this.name = name;
  this.age = age;
  this.setName = function (name) {
    this.name = name;
  }
}

var p1 = new Person('Tom', 12);
p1.setName('Jack');
console.log(p1);
console.log(p1 instanceof Person); // true
```
但是此种方式创建的对象，每个对象中都包含```setName```方法，占用内存。

⭐ 方式五：构造函数+原型的组合模式
+ 套路：自定义构造函数，属性在函数中初始化，方法添加到原型上
+ 适用场景：需要创建多个类型确定的对象
```javascript
function Person (name, age) {
  // 在构造函数中，只初始化一般属性
  this.name = name;
  this.age = age;
}
Person.prototype.setName = function (name) {
  this.name = name;
}

var p1 = new Person('Tom', 12);
var p2 = new Person('Jack', 13);
console.log(p1, p2)
```
结果为：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220321135956.png)


### 3.2 原型链继承
#### 3.2.1 方式一： 原型链继承
过程如下：
1. 定义父类型构造函数
2. 给父类型的原型添加方法
3. 定义子类型的构造函数
4. 创建父类型的对象赋值给子类型的原型
5. 将子类型原型的构造属性设置为子类型
6. 给子类型原型添加方法
7. 创建子类型的对象：可以调用父类型的方法

<b>⭐ 关键点：子类型的原型为父类型的一个实例对象</b>

```javascript
1. 定义父类型构造函数
function Supper () {
  this.supProp = 'Supper Property';
}
2. 给父类型的原型添加方法
Supper.prototype.showSupperProp = function () {
  console.log(this.supProp);
}

3. 定义子类型的构造函数
function Sub () {
  this.subProp = 'Sub Property';
}
// 子类型的原型为父类型的一个实例对象
4. 创建父类型的对象赋值给子类型的原型
Sub.prototype = new Supper();
5. 将子类型原型的构造属性设置为子类型
Sub.prototype.constructor = Sub
6. 给子类型原型添加方法
Sub.prototype.showSubProp = function () {
  console.log(this.subProp);
}

7. 创建子类型的对象：可以调用父类型的方法
var sub = new Sub();
sub.showSupperProp();
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220321171328.png)

#### 3.2.2 方式二： 借用构造函数继承（假的）
过程如下：
1. 定义父类型构造函数
2. 定义子类型构造函数
3. 在子类型构造函数中调用父类型构造

<b>⭐ 关键点：在子类型构造函数中通过```call()```调用父类型构造函数</b>

```javascript
1. 定义父类型构造函数
function Person (name, age) {
  this.name = name;
  this.age = age;
}

2. 定义子类型构造函数
function Student (name, age, price) {
  3. 在子类型构造函数中调用父类型构造
  Person.call(this, name, age); //相当于 this.Person(name,age)
  this.price = price;
}

var s = new Student('Tom', 12, 13000);
console.log(s);
```
结果为：```Student { name: 'Tom', age: 12, price: 13000 }```
#### 3.2.3 方式三： 组合继承（原型链 + 借用构造函数）
过程如下： 
1. 利用原型链实现对父类型对象的方法继承
2. 利用```call()```借用父类型构造函数初始化相同属性

```javascript
function Person (name, age) {
  this.name = name;
  this.age = age;
}
Person.prototype.setName = function (name) {
  this.name = name;
}

function Student (name, age, price) {
  Person.call(this, name, age); // 为了得到属性
  this.price = price;
}
Student.prototype = new Person(); // 为了能看到父类型的方法
Student.prototype.constructor = Student; // 修正constructor属性
Student.prototype.setPrice = function (price) {
  this.price = price;
}

var s = new Student('Tom', 12, 13000);
s.setName('Jack');
s.setPrice(15000);
console.log(s);
```
结果为```Student { name: 'Jack', age: 12, price: 15000 }```

#### 3.2.4 补充：new一个对象背后做了什么？
+ 创建一个空对象
+ 给对象设置```__proto__```，值为构造函数对象的```prototype```属性值
+ 执行构造函数体（给对象添加属性 / 方法）


## 4.线程机制与事件机制
### 4.1 进程与线程
<b>进程（process）：</b>程序的一次执行，它占有一片独有的内存空间。可以通过window任务管理器查看进程。
<b>线程（thread）：</b>线程是进程内一个独立执行的单元，是程序执行的一个完整流程，是CPU的最小的调度单元。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220322145958.png)
如果一个程序有2个进程，每个进程包含1个线程，则该程序是单线程程序。

⭐ 相关知识：
1. 应用程序必须运行在某个进程的某个线程上
2. 一个进程至少有一个运行的线程：主线程（线程启动后自动创建）
3. 一个进程中也可以同时运行多个线程，我们会说程序是多线程运行的
4. 一个进程内的数据可以供其中的多个线程直接共享
5. 多个进程之间的数据是不能直接共享的
6. 线程池（thread pool）：保存多个线程对象的容器，实现线程对象的反复利用

⭐ 相关问题：
1. 何为多进程与多线程？
多进程运行：一个应用程序可以启动多个实例运行
多线程：在一个进程内，同时有多个线程运行
2. 比较单线程与多线程？
多线程：【优点】能有效提升CPU的利用率 【缺点】创建多线程开销，线程间切换开销，死锁与状态同步问题
单线程：【优点】顺序变成简单易懂 【缺点】效率低
3. JS是单线程还是多线程？
JS是单线程运行的。但使用H5中的Web Workers可以多线程运行。
4. 浏览器运行是单线程还是多线程？
浏览器运行是多线程。
5. 浏览器运行是单进程还是多进程？
有单进程也有多进程。其中，单进程：firefox、老版IE，多进程：chrome、Edge。
### 4.2 浏览器内核

|  浏览器名称   | 内核  |
|  ----  | ----  |
| Chrome、Safari  | webkit |
| firefox  | Gecko |
| IE  | Trident |
| 360，搜狗等国内浏览器  | Trident + webkit |

浏览器内核由很多模块组成，其中包括：
+ js引擎模块：负责js程序的编译与运行
+ html，css文档解析模块：负责页面文本解析
+ DOM/CSS模块：负责dom/css在内存中的相关处理
+ 布局和渲染模块：负责页面的布局和效果的绘制（内存中的对象）

<b>【上述部分运行在主线程】</b>

+ ……
+ 定时器模块：负责定时器的管理
+ DOM事件响应模块：负责事件管理
+ 网络请求模块：负责ajax请求

<b>【上述部分运行在分线程】</b>

### 4.3 定时器引发的思考
相关问题：
1. 定时器真的是定时执行的吗？
定时器并不能保证真正定时执行，一般会延迟一丁点（可以接受），也可能延迟很长时间（不能接收）
```javascript
var start = Date.now();
console.log('启动定时器前……');
setTimeout(function () {
  console.log('定时器执行了', Date.now() - start);
}, 200)
```
多次执行结果为
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324103330.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324103357.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324103437.png)

```javascript
var start = Date.now();
console.log('启动定时器前……');
setTimeout(function () {
  console.log('定时器执行了', Date.now() - start);
}, 200)
for (let i = 0; i < 1000000000; ++i) {

}
```
多次执行结果为
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324110425.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324110514.png)


2. 定时器回调函数是在哪个线程执行的？
在主线程执行（JS是单线程的）

3. 定时器是如何实现的？
事件循环模型（详情见后续内容）

### 4.4 JS是单线程执行的
1. 如何证明JS执行是单线程的？
setTimeout()的回调函数是在主线程执行的
定时器回调函数只有在运行栈中的代码全部执行完后才有可能执行
```javascript
setTimeout(function () {
  console.log('timeout --> 2000');
}, 2000)
setTimeout(function () {
  console.log('timeout --> 1000');
}, 1000)
function fn () {
  console.log('fn函数执行');
}
fn()
```
执行结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324113636.png)

```javascript
setTimeout(function () {
  console.log('timeout --> 2000');
}, 2000)
setTimeout(function () {
  console.log('timeout --> 1000');
}, 1000)
function fn () {
  console.log('fn函数执行');
}
fn()
console.log('alert之前');
alert('---------'); // 暂停当前主线程的执行，同时暂停计时，点击确定之后，恢复程序的执行和计时
console.log('alert之后');
```
执行结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324114233.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324114309.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324114353.png)

2. 为什么JS要用单线程模式，而不用多线程模式？
Javascript的单线程与它的用途有关，作为浏览器脚本语言，Javascript的主要用途是与用户互动以及操作DOM，这决定了它只能是单线程的，负责会带来很复杂的同步问题。

3. 代码分类
* 初始化代码
* 回调代码

4. JS引擎执行代码的基本流程
先执行初始化代码【包含一些特别的代码：设置定时器、绑定监听、发送ajax请求】，后面的某一个时刻才会执行回调代码

> 其中，设置定时器指的是setTimeout(),不包括内部的回调函数，其内部的回调函数需要在初始化代码执行完毕之后再执行
```javascript
setTimeout(function () {
  console.log('timeout --> 2000');
}, 2000)
setTimeout(function () {
  console.log('timeout --> 1000');
}, 1000)
setTimeout(function () {
  console.log('timeout --> 0');
}, 0)
function fn () {
  console.log('fn函数执行');
}
fn()
console.log('alert之前');
alert('---------');
console.log('alert之后');
```
最终结果执行顺序：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324143955.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324144022.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220324144050.png)

### 4.5 事件循环（轮询）模型

#### 4.5.1 模型原型图
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/2356897452565.png)
对于JS代码可以分为两类：
1. 初始化执行代码（同步代码）：包含绑定dom事件监听，设置定时器，发送ajax请求的代码
2. 回调执行代码（异步代码）：处理回调逻辑

<b>JS引擎执行代码的基本流程：</b>初始化代码 ➡ 回调代码

<b>模型的两个重要组成部分：</b>事件管理模块、回调队列

<b>🍎 模型的运转流程：</b>执行初始化代码，将事件回调函数交给对应模块管理 ➡ 当事件发生时，管理模块会将回调函数及其数据添加到回调队列中 ➡ 只有当初始化代码执行完成后（可能需要一段时间），才会遍历读取回调队列中的回调函数执行

```javascript
  function fn1 () {
    console.log('执行fn1()');
  }
  fn1();
  document.getElementById('btn').onclick = function () {
    console.log('点击了btn');
  }
  setTimeout(function () {
    console.log('执行了定时器');
  }, 2000);
  function fn2 () {
    console.log('执行fn2()');
  }
  fn2();
```
上述代码的执行结果不唯一，首先确定的结果是
```javascript
执行fn1()
执行fn2()
```
剩余的```执行了定时器```和```点击了btn```，需要看点击btn的时机，如果点击btn在前，则执行顺序为```点击了btn```、```执行了定时器```，否则为```执行了定时器```、```点击了btn```。但是无论怎样```执行fn1()```和```执行fn2()```的顺序是不会改变的，因为这个为初始化执行代码。

#### 4.5.2 相关概念
+ 执行栈（execution stack）：所有代码都是在此空间中执行的
+ 浏览器内核（browser core）:js引擎模块（在主线程）、其他模块（在主/分线程处理）
+ 任务队列（task queue）、消息队列（message queue）、事件队列（event queue）：同一个callback queue
+ 事件轮询（event loop）：从任务队列中循环取出回调函数放入执行栈中处理（一个接一个）


### 4.6 H5 Web Workers（多线程）
H5提供了JS分线程的实现，取名为：```Web Workers```，我们可以将一些大计算量的代码交由web Worker 运行而不冻结用户界面。但是子线程完全受主线程控制，且不操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质。

Web Worker 的作用，就是为 JavaScript 创造多线程环境，允许主线程创建 Worker 线程，将一些任务分配给后者运行。在主线程运行的同时，Worker 线程在后台运行，两者互不干扰。等到 Worker 线程完成计算任务，再把结果返回给主线程。这样的好处是，一些计算密集型或高延迟的任务，被 Worker 线程负担了，主线程（通常负责 UI 交互）就会很流畅，不会被阻塞或拖慢。

Worker 线程一旦新建成功，就会始终运行，不会被主线程上的活动（比如用户点击按钮、提交表单）打断。这样有利于随时响应主线程的通信。但是，这也造成了 Worker 比较耗费资源，不应该过度使用，而且一旦使用完毕，就应该关闭。

相关API：
+ ```Worker```：构造函数，加载分线程执行的JS文件
+ ```Worker.prototype.onmessage```：用于接收另一个线程的回调函数
+ ```Worker.prototype.postMessage```：向另一个线程发送消息

不足之处：
1. ```Worker```内代码不能操作DOM（更新UI）
2. 不能跨域加载JS
3. 不是每个浏览器都支持这个新特性
4. 速度慢

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
```javascript
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

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220329142107.png)

主线程中的```this```是```window```，而work线程中的```this```是一个专门为 ```Worker``` 定制的全局对象。
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