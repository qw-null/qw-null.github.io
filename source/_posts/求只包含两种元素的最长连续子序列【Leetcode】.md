---
title: 【Leetcode】求只包含两种元素的最长连续子序列
date: 2022-01-29 18:27:08
categories:
- Leetcode
tags:
- 每日一题
---
[题目链接](https://leetcode-cn.com/problems/fruit-into-baskets/)

## 题目：

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220129182956.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220129183031.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220129183102.png)

## 解题思路：
题目最终求解的是只包含两种元素的最长的连续子序列。
因此需要明确已下几点：
+ 如何保证水果的种类只有两种？
+ 当子序列中水果种类已经有两种，如何删除旧的水果种类，添加新的水果种类？

对于上述两个问题，答案如下：

设置一个保存水果种类的数组，初始化时填入第一棵树的水果种类。 ``` ftype = [fruits[0]]```
当走到下一棵树时，先判断是否是已经保存的水果种类 ``` ftype.includes(fruits[j] ```,如果没有包含，则存在下面两种情况：
```javascript
// 1. 只有一种水果，将新的水果种类加入种类数组中
if(ftype.length<=1){
    ftype[1] = fruits[j];
}

// 2. 当前水果种类已经达到两种：去掉第一种水果，加入新的水果种类
// 此时就要用到第一种水果的结束位置这一变量
i= firstFruit;//更新开始采摘的位置（滑动窗口左边界）
ftype[0]=fruits[j-1];
ftype[1]=fruits[j];

```
另外，解题时还要注意对于当前种类水果最后一个位置的保存。

## 解题代码：
```javascript
var totalFruit = function(fruits) {
    let res = 0;//最终结果
    let subLen = 0;//子序列长度（个数）
    let i=0;//采摘开始的位置
    let firstFruit = 0;//第一种水果的结束位置
    let ftype = [fruits[i]];//采摘的水果种类

    for(let j=0;j<fruits.length;j++){
        if(!ftype.includes(fruits[j])){//水果种类未包含
            // 两种情况：1.水果种类少于两种，记录
            // 2.水果种类多余两种
            if(ftype.length<=1){
                //水果种类少于两种
                ftype[1] = fruits[j];
            }else{
                //水果种类多余两种:去掉第一种水果，加入新的水果种类
                i= firstFruit;//更新开始采摘的位置（滑动窗口左边界）
                ftype[0]=fruits[j-1];
                ftype[1]=fruits[j];
            }
        }
        if(fruits[j]!==fruits[firstFruit]){
            //更新第一种水果的结束位置
            firstFruit = j;
        }
        subLen=j-i+1;
        res = res>subLen?res:subLen;
    }
    return res;

};

```
## 补充知识：
``` Array.includes() ```方法用来判断一个数组是否包含一个指定的值。
``` arr.includes(searchElement, fromIndex) ```

参数|描述
:----|:----
searchElement|必须。需要查找的元素值。
fromIndex|可选。从该索引处开始查找 searchElement。如果为负值，则按升序从 array.length + fromIndex 的索引开始搜索。默认为 0。

返回值：
包含元素返回true，不包含元素返回false。