---
title: JavaScript的Map详解
date: 2022-02-19 11:31:45
categories:
- JavaScript
tags:
- JavaScript
---

在ES6之前，JS实现“键/值”式存储可以使用```Object```来高效地完成，也就是使用对象属性作为键，再使用属性来引用值。

ES6中出现Map，```Map```是一种新的集合类型。```Map```对象保存键-值对。任何值(对象或者原始值) 都可以作为一个键或一个值。

## Map和Object的区别
+ ```Map```的键值是有序的（FIFO原则，First Input First Output），而添加到对象中的键则不是
+ ```Map```的键值对个数可以通过```size```属性获取，而```Object```的键值对个数只能手动计算
+ ```Object``` 都有自己的原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。

## Map对象的方法
+ ```set(key, val)``` ：向Map中添加新元素
+ ```get(key)```: 通过键值查找特定的数值并返回
+ ```has(key)```: 判断Map对象中是否有key所对应的值，有返回true,否则返回false
+ ```delete(key)```: 通过键值从Map中移除对应的数据
+ ```clear()```: 将这个Map中的所有元素删除

```javascript
const m1 = new Map([['a',111],['b',222]]);
console.log(m1); // {'a' => 111, 'b' => 222}

m1.get('a')  // 111
m1.has('b') // true
m1.set('c',333) // {'a' => 111, 'b' => 222, 'c' => 333}
m1.delete('b') // true
console.log(m1); // {'a' => 111, 'c' => 333}
```

## Map的遍历方法
+ ```keys()```：返回键名的遍历器
+ ```values()```：返回键值的遍历器
+ ```entries()```：返回键值对的遍历器
+ ```entries()```：返回键值对的遍历器

```javascript
const map = new Map([['a', 1], ['b',  2]])

for (let key of map.keys()) {
  console.log(key)
}
// "a"
// "b"

for (let value of map.values()) {
  console.log(value)
}
// 1
// 2

for (let item of map.entries()) {
  console.log(item)
}
for(let i of map){
  console.log(i)
}
// ["a", 1]
// ["b", 2]

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value)
}
// "a" 1
// "b" 2

// for...of...遍历map等同于使用map.entries()

for (let [key, value] of map) {
  console.log(key, value)
}
// "a" 1
// "b" 2

```

使用Map解题：[数组中第K个独一无二的字符串【Leetcode】](https://qw-null.github.io/2022/02/19/%E6%95%B0%E7%BB%84%E4%B8%AD%E7%AC%ACK%E4%B8%AA%E7%8B%AC%E4%B8%80%E6%97%A0%E4%BA%8C%E7%9A%84%E5%AD%97%E7%AC%A6%E4%B8%B2%E3%80%90Leetcode%E3%80%91/)