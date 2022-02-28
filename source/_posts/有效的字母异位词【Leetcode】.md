---
title: 有效的字母异位词【Leetcode】
date: 2022-02-28 20:12:46
categories:
- Leetcode
tags:
- 每日一题
---
## 题目
给定两个字符串 ```s``` 和 ```t ```，编写一个函数来判断 ```t ```是否是 ```s``` 的字母异位词。

注意：若``` s ```和 ```t ```中每个字符出现的次数都相同，则称 ```s ```和 ```t ```互为字母异位词。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220228202029.png)

## 解题思路
解体的关键在于：统计出包含元素的种类以及包含元素的个数。
因此，可以维护一个长度为26的数组【原因：```s```和```t```仅包含小写字母】，数组的每一项分别表示```a-z```中的一项，数组每一项的值代表出现的次数。

要点：
+ 如何将字符串的单个字符转换为数字？
借助JavaScript的 ```charCodeAt()``` 方法。
charCodeAt() 方法可返回指定位置的字符的 Unicode 编码。这个返回值是 0 - 65535 之间的整数。
语法：```stringObject.charCodeAt(index)```
<b>注释：</b>字符串中第一个字符的下标是 0。如果 index 是负数，或大于等于字符串的长度，则 charCodeAt() 返回 NaN。

  ```javascript
  let s = 'abcd';
  console.log(s.charCodeAt()); // 97  [ a的ASCII码 ]
  console.log(s.charCodeAt(1)); // 98 [ b的ASCII码 ]
  ```

## 解题代码

```javascript
var isAnagram = function(s, t) {
    if(s.length!==t.length) return false;
    let arr = new Array(26).fill(0);
    const a = 'a'.charCodeAt(); //获取a的ASCII码
    for(let i of s){
        arr[i.charCodeAt()-a]++;
    }
    for(let j of t){
        if(!arr[j.charCodeAt()-a]) return false;
        arr[j.charCodeAt()-a]--;
    }
    return true;
};
```
