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

+ 补充点：如何将数字（ASCII码）转换为字符？
使用JS的```fromCharCode```方法
语法：```String.fromCharCode(num1[, ...[, numN]])```
参数```num1, ..., numN```：一系列 UTF-16 代码单元的数字。范围介于 0 到 65535（0xFFFF）之间。大于 0xFFFF 的数字将被截断。不进行有效性检查。
返回值：一个长度为 N 的字符串，由 N 个指定的 UTF-16 代码单元组成。
```javascript
var num = 97;
String.fromCharCode(num);  // 'a'

var num1 = 100;
String.fromCharCode(num1);  // 'd'
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
## 相似题目
给你一个字符串数组，请你将 <b>字母异位词</b> 组合在一起。可以按任意顺序返回结果列表。

<b>字母异位词</b> 是由重新排列源单词的字母得到的一个新单词，所有源单词中的字母通常恰好只用一次。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220301095554.png)

#### 解题思路
统计字符串数组每一项的字母出现的次数，并且以此作为键，具有相同键值的元素作为该键的值，就是字母异位词。
效果如下：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220301100250.png)

#### 解题代码
```javascript
var groupAnagrams = function(strs) {
    let map = {};
    for(let item of strs){
        let count = new Array(26).fill(0);
        for(let i of item){
            count[i.charCodeAt()-'a'.charCodeAt()]++;
        }
        if(map[count]){
            map[count].push(item);
        }else{
            map[count] = [item];
        }
    }
    return Object.values(map);

};
```
## 补充知识
Object.values()方法

返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用for...in循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )。

语法：```Object.values(obj)```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220301101119.png)

```javascript
var obj = { foo: 'bar', baz: 42 };
console.log(Object.values(obj)); // ['bar', 42]

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.values(obj)); // ['a', 'b', 'c']

// 类似于数组的对象，具有随机键排序
// 当我们使用数字键时，按照数字键的顺序返回数值。
var an_obj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.values(an_obj)); // ['b', 'c', 'a']

// getFoo 是不可枚举的属性
var my_obj = Object.create({}, { getFoo: { value: function() { return this.foo; } } });
my_obj.foo = 'bar';
console.log(Object.values(my_obj)); // ['bar']

// 非对象参数将被强制转换为对象
console.log(Object.values('foo')); // ['f', 'o', 'o']
```

