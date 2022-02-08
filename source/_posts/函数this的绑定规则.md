---
title: 函数this的绑定规则
date: 2022-02-08 14:43:40
categories:
- JavaScript
tags:
- JavaScript
---

## 1.默认绑定
当函数调用类型为独立函数调用时，函数的this为默认绑定，指向全局变量；在严格模式下，this将绑定到undefined

```javascript
function foo(){
  console.log(this.a)
}

foo();
```
上述代码中，``` foo() ```为独立函数调用，foo()中的this指向全局变量（严格模式下则指向undefined）

## 2.隐式绑定
当函数的调用位置有上下文对象时，或者说函数在被调用时<span style="color:red;"><b>被某个对象拥有或者包含时</b></span>，隐式绑定规则就会把函数调用中的this绑定到这个上下文对象。

如下，foo在调用时this便隐式绑定到obj上
```javascript
function foo(){
  console.log(this.a);
}
const obj = {
  a:2,
  foo
}
obj.foo(); //2
```

知识点：隐式绑定丢失的情况（待补充！！）

## 3.显示绑定
使用call、apply和bind显示的绑定函数调用时的this的指向

## 4.new 
当使用new调用函数时，会发生this的指向绑定，但此处发生的绑定与函数本身无关

