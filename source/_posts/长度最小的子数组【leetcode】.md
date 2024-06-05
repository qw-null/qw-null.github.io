---
title: 【leetcode】长度最小的子数组
date: 2022-01-28 22:12:31
categories:
- Leetcode
tags:
- 每日一题
---

[题目链接](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)
## 题目：
给定一个含有 n 个正整数的数组和一个正整数 target 。
找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 ```[numsl, numsl+1, ..., numsr-1, numsr] ```，并返回其长度。如果不存在符合条件的子数组，返回 0 。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220128221650.png)

## 解题思路：
1. 方法一：暴力解法
通过两个for循环，找出所有满足和 ≥ target的情况，然后取最小值。时间复杂度为O($ n^2 $)。

2. 方法二：滑动窗口法
所谓的滑动窗口就是不断调节子序列的起始位置和终止位置，从而得出想要的结果。

模式说明：
输入是一个数组或字符串，求解的结果是具有某种特质的子数组或子字符串。这种情况下下，可以使用滑动窗口的方法求解。

应用滑动窗口的<b>注意事项</b>：
+ 可以通过两个指针来标识窗口的边界
+ 窗口的长度可固定也可改变，取决于求解问题的性质
+ 维护一个或一组和窗口相关联的状态变量，能有效降低计算量和算法复杂度

## 实现代码：
方法一：暴力解法
```javascript
var minSubArrayLen = function(target, nums) {
    let res = Number.MAX_VALUE; // 存放最终结果,找最小的值->初始值应为最大的数
    let [sum,subLen] = [0,0];//子序列的和，长度
    for(let i=0;i<nums.length;i++){
        sum = 0;
        for(let j=i;j<nums.length;j++){
            sum += nums[j];
            //当子序列的值>=target
            if(sum>=target){
                subLen = j-i+1;
                res = res<subLen?res:subLen;//取满足要求的最小长度
                break;
            }
        }

    }
    return res===Number.MAX_VALUE?0:res;

};
```
方法二：滑动窗口法
```javascript
var minSubArrayLen = function(target, nums) {
    // 子序列之和，子序列长度，最终结果
    let [sum,subLen,res]=[0,0,Number.MAX_VALUE];
    //*定义子序列起始位置【窗口开始位置】
    let i=0;
    for(let j=0;j<nums.length;j++){
        sum += nums[j];
        while(sum>=target){
            subLen=j-i+1;
            res = res<subLen?res:subLen;
            sum-=nums[i];
            i++;
        }
    }
    return res!==Number.MAX_VALUE?res:0;
};
```