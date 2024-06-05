---
title: 数组中第K个独一无二的字符串【Leetcode】
date: 2022-02-19 14:12:29
categories:
- Leetcode
tags:
- 每日一题
---

## 题目

<b>独一无二的字符串</b> 指的是在一个数组中只出现过 <b>一次</b> 的字符串。

给你一个字符串数组 ```arr``` 和一个整数 ```k``` ，请你返回 ```arr``` 中第 ```k``` 个 <b>独一无二的字符串</b> 。如果 <b>少于</b> ```k``` 个独一无二的字符串，那么返回 ```空字符串 ""``` 。

注意，按照字符串在原数组中的 <b>顺序</b> 找到第 k 个独一无二字符串。

 ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220219142148.png)

## 解题思路
使用Map统计数组中元素的个数，筛选Map中数量为1的键，将其放入数组中。

## 实现代码
```javascript
var kthDistinct = function(arr, k) {
    let map = new Map();
    let res = [];
    for(const i of arr){
        map.set(i,(map.get(i)|| 0)+1);
        // extra = extra || 0;
        // 如果extra 为undefined或者null,false, extra=0 ;否则为原值 --> 很常见的非空判断。
    }
    console.log(map)
    for(const item of map){
        console.log(item)
        if(item[1] === 1) res.push(item[0]);
    }

    return res[k-1]||'';

};
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220219142709.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220219142653.png)

#### 知识点 -- 常见的非空判断

```extra = extra || 0;```

如果```extra``` 为```undefined```或者```null```,```false```, ```extra=0``` ；否则为原值。