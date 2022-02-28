---
title: 统计元素出现的次数【Leetcode刷题心得】
date: 2022-02-28 09:25:42
categories:
- Leetcode
tags:
- 刷题技巧
---

## 0.情景模拟

在日常刷题过程中，通常会对字符串或者数组中的元素进行处理，在处理过程中，往往需要统计其中元素出现的次数，因此，本文主要记录日常自己使用的相关方法。

## 1.对字符串的处理
以字符串 ``` s = 'abcabcaadcce' ```为例。
#### 1.1 统计字符串```s```中元素出现的次数

```javascript
let s = 'abcabcaadcce';
let obj = {};

// 1.统计字符出现的次数
for (let i = 0; i < s.length; ++i) {
  let key = s[i];
  if (obj[key]) {
    obj[key]++;
  } else {
    obj[key] = 1;
  }
}
console.log('统计结果：', obj);
```
统计结果： { a: 4, b: 2, c: 4, d: 1, e: 1 }

#### 1.2 遍历统计对象(键->值)

```javascript
for (let i in obj) {
  console.log('键：', i, ' -> 值：', obj[i]);
}
```
键： a  -> 值： 4
键： b  -> 值： 2
键： c  -> 值： 4
键： d  -> 值： 1
键： e  -> 值： 1

#### 1.3 对元素出现的次数进行排序
```javascript
let arrObj = [];
for (let i in obj) {
  arrObj.push([i, obj[i]]);
}
console.log('obj转arrObj:', arrObj)

arrObj.sort((a, b) => {
  return a[1] - b[1];
});
console.log('排序后的结果：', arrObj);
```

obj转arrObj: [ [ 'a', 4 ], [ 'b', 2 ], [ 'c', 4 ], [ 'd', 1 ], [ 'e', 1 ] ]
排序后的结果： [ [ 'd', 1 ], [ 'e', 1 ], [ 'b', 2 ], [ 'a', 4 ], [ 'c', 4 ] ]



#### 1.4 删除满足条件的元素

```javascript
for (let i in obj) {
  if (i == 'a')
    delete obj[i];
}
console.log('删除后的结果', obj)
```

删除后的结果 { b: 2, c: 4, d: 1, e: 1 }

## 2.对数组的处理

以数组 ```arr = [1, 1, 2, 2, 2, 3, 3, 3, 3, 4, 8, 8, 9]``` 为例。

#### 2.1 统计数组中元素出现的次数

```javascript
let arr = [1, 1, 2, 2, 2, 3, 3, 3, 3, 4, 8, 8, 9];
let obj = {};

for (let i = 0; i < arr.length; ++i) {
  let key = arr[i];
  if (obj[key]) {
    obj[key]++;
  } else {
    obj[key] = 1;
  }
}
console.log('统计结果：', obj);
```
统计结果： { '1': 2, '2': 3, '3': 4, '4': 1, '8': 2, '9': 1 }