---
title: Javascript数组的reduce方法
date: 2022-03-03 16:15:40
categories:
- JavaScript
tags:
- JavaScript
---

## 简介
Javascript数组的```reduce```方法对数组中的每个元素执行一个由您提供的```reducer```函数(升序执行)，将其结果汇总为单个返回值。

基本语法：```arr.reduce(callback,[initialValue])```

参数说明：
callback 执行数组中每个值的函数，包含四个参数：
* ```previousValue```：第一项的值或者上一次叠加的结果值，或者是提供的初始值```initialValue```
* ```currentValue```：数组中当前被处理的元素
* ```index```：当前元素在数组中的索引
* ```array```：数组本身

initialValue（可选）：作为第一次调用```callback```的第一个参数，可以控制返回值的格式

> reduce() 方法可以使用一下这个表达式总结一下：
```
[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)
```


## 基本示例
```javascript
const arr = [2, 4, 6, 8];
let i = 0;
arr.reduce((pre, cur, index, arr) => {
  console.log(`第${i + 1}次执行：pre=${pre},cur=${cur},index=${index}`);
  i++;
  return pre + cur;
}, 10);
```

结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220303165351.png)

代码分析：
+ 数组中的元素依次执行了回调函数。
+ 因为给 initialValue 赋了初始值 10，所以第一次执行时， pre 的值默认从 10 开始。
+ 每次执行时，pre 的值都是 cur 元素前的所有元素之和。
+ 最后返回数组所有元素累加的和。


```javascript
const arr = [2, 4, 6, 8];
let i = 0;
arr.reduce((pre, cur, index, arr) => {
  console.log(`第${i + 1}次执行：pre=${pre},cur=${cur},index=${index}`);
  i++;
  return pre + cur;
});
```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220303165658.png)

可以看到代码只执行了3次，并且是从数组的第二个元素开始执行的，数组的第一位元素默认作为```pre```的值。

## 实际用法

#### 1.计算数组中每个元素出现的次数

```javascript
const arr = ['Bob', 'Lily', 'Sam', 'Sam', 'Jack', 'Lily', 'Jack', 'Jack'];
arr.reduce((pre, cur) => {
  console.log(pre, cur);
  if (cur in pre) {
    pre[cur]++;
  } else {
    pre[cur] = 1;
  }
  return pre;
}, {})
```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220303170846.png)

#### 2.数组去重
```javascript
const arr = ['Bob', 'Lily', 'Sam', 'Sam', 'Jack', 'Lily', 'Jack', 'Jack'];
arr.reduce((pre, cur) => {
  console.log(pre, cur);
  if (!pre.includes(cur)) {
    pre.push(cur);
  }
  return pre;
}, [])
```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220303171115.png)

#### 3.对象属性求和
```javascript
const person = [
    {
      name: 'Jack',
      age: 18
    }, 
    {
        name: 'Bob',
        age: 17
    }, {
        name: 'Lily',
        age: 19
    }
]

person.reduce((a, b) => {
    a = a + b.age;
    return a;
}, 0); // 54
```