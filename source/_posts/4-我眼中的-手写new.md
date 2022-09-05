---
title: 【我眼中的】 - 【4】手写new
date: 2022-04-19 19:35:43
categories:
- 我眼中的系列
tags:
- 我眼中的系列
---

## 1. ```new``` 操作符

new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。

使用```new```调用类的构造函数会执行如下操作：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220905114711.png)

```javascript
⭐ 1.构造函数无返回值
function Person(name){
  this.name = name;
}
let obj = new Person('Jack');
console.log(obj);

打印值为{name:'Jack'}

⭐ 2.构造函数返回值为对象
function Person(name){
  this.name = name;
  return { age:18 };
}
let obj = new Person('Jack');
console.log(obj);

打印值为{ age:18 }

⭐ 3.构造函数返回值为非对象
function Person(name){
  this.name = name;
  return 1;
}
let obj = new Person('Jack')
console.log(obj)

打印值为{name:'Jack'}
 ```

 所以说```new```操作符必须要返回一个对象，如果构造函数中没有```return```那就返回```this```，如果有```return```并且```return```的是一个对象，那么就会返回这个对象，否则都返回```this```。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220419204952.png)

> 来自MDN的解释：
当代码 ```new Foo(...)``` 执行时，会发生以下事情：
1.一个继承自 ```Foo.prototype``` 的新对象被创建。
2.使用指定的参数调用构造函数 ```Foo```，并将 ```this``` 绑定到新创建的对象。```new Foo``` 等同于 ```new Foo()```，也就是没有指定参数列表，```Foo``` 不带任何参数调用的情况。
3.由构造函数返回的对象就是 ```new``` 表达式的结果。如果构造函数没有显式返回一个对象，则使用步骤1创建的对象。（一般情况下，构造函数不返回值，但是用户可以选择主动返回对象，来覆盖正常的对象创建步骤）

<b>※ 构造函数为箭头函数</b>

普通函数创建时，引擎会按照特定的规则为这个函数创建一个```prototype```属性（指向原型对象）。默认情况下，所有原型对象自动获得一个名为 ```constructor``` 的属性，指回与之关联的构造函数。

```javascript
function Person(){
    this.age = 18;
}
Person.prototype
/**
{
    constructor: ƒ Foo()
    __proto__: Object
}
**/
```

创建箭头函数时，引擎不会为其创建```prototype```属性，箭头函数没有```constructor```供```new```调用，因此使用new调用箭头函数会报错！

```javascript
const Person = ()=>{}
new Person()//TypeError: Foo is not a constructor
```

也就是说构造函数必须是声明式的，即```function xxx()```，不可以是表达式的。

## 2.手写```new```

依据原理：
 1. 创建一个对象```obj```
 2. 该对象的```__proto__```指向构造函数Fn的原型```prototype```
 3. 执行构造函数```Fn```的代码，往新创建的对象```obj```上添加成员属性和方法
 4. 返回这个新的对象```obj```

 ```javascript
 function _new (constructor, ...args) {
  // 构造函数合法类型判断
  if (typeof constructor !== 'function') {
    throw 'constructor must be a function';
  }

  // 创建一个空对象，并继承构造函数的 prototype 属性
  let obj = Object.create(constructor.prototype);

  // 绑定this
  let result = constructor.apply(obj, args);

  // 如果有返回值且返回值是对象类型，那么就将它作为返回值，否则就返回之前新建的对象
  if ((typeof result === 'object' && result !== null) || typeof result === 'function') {
    return result;
  } else {
    return obj;
  }

}
 ```

### Object.create

作用：创建一个新对象，使用现有的对象来提供新创建的对象的 ```__proto__```

因此，可以得出结论

```javascript
let obj = Object.create(func.prototype)

⭐ 相当于如下语句：
function Con(){}
Con.prototype = func.prototype;
Con.prototype.constructor = func;
let obj = new Con();
```

### 通过```new```的方式创建对象与通过字面量创建的区别
+ ```new Object()``` 方式创建对象本质上是方法调用，涉及到在原型链中查找该方法，当找到该方法后，又会生产方法调用必须的堆栈信息，方法调用结束后，还要释放该堆栈，性能不如字面量的方式
+ 通过对象字面量定义对象时，不会调用```Object```的构造函数

Tips : 使用字面量的方式创建对象 性能上 更好，可读性 更高