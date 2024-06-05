---
title: N进制数【Leetcode】
date: 2022-03-07 14:45:50
categories:
- Leetcode
tags:
- 每日一题
---
## 需求
给定一个数字，求该数字的N进制表示方法（N<10）。

```javascript
var getN_aryNum = function(n,num){//n表示进制，num表示原数
  if(num === 0) return '0';
  let res = [];
  let flag = false;//原数是正数还是负数的标志
  if(num<0) {
    flag = true;
    num = -num;
  }
  while(num>0){
    res.push(num%n);
    num = Math.floor(num/n);
  }
  if(flag) res.push('-');
  return res.reverse().join('');
}
```
## 题目
[原题链接：504. 七进制数](https://leetcode-cn.com/problems/base-7/)
给定一个整数 ```num```，将其转化为 <b>7 进制</b>，并以字符串形式输出。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220307145705.png)

## 解题代码
```javascript
 var getN_aryNum = function(n,num){
  if(num === 0) return '0';
  let res = [];
  let flag = false;//原数是正数还是负数的标志
  if(num<0) {
    flag = true;
    num = -num;
  }
  while(num>0){
    res.push(num%n);
    num = Math.floor(num/n);
  }
  if(flag) res.push('-');
  return res.reverse().join('');
}
var convertToBase7 = function(num) {
    return getN_aryNum(7,num)

};
```