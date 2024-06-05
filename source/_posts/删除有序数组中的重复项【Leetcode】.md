---
title: 【Leetcode】删除有序数组中的重复项
date: 2021-11-18 11:26:53
categories:
- Leetcode
tags:
- 每日一题
---
## 题目：

给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。
不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

说明：

为什么返回数值是整数，但输出的答案是数组呢?
请注意，输入数组是以「引用」方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。
你可以想象内部操作如下:
```javascript
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```
示例：
```
示例 1：
输入：nums = [1,1,2]
输出：2, nums = [1,2]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。

示例 2：
输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。
```

## 解题思路：
数组中的元素在内存地址中是连续的，不能单独删除数组中的某个元素，只能覆盖。
<b>双指针法：</b>通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211118113717.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211118113752.png)
说明：当 nums[fast] === nums[slow] 时，代码中无需进行处理，只需要考虑 nums[fast] != nums[slow] 的情况。
当 nums[fast] != nums[slow] 时，slow前进 --> nums[slow] 需要使用 nums[fast] 覆盖 --> fast前进

## 实现代码
```javascript
var removeDuplicates = function(nums) {
    // 初始化快、慢指针
    let [fast, slow] = [0, 0];
    for (fast = 0; fast < nums.length; ++fast){
        // 处理nums[fast] != nums[slow]的情况
        if (nums[fast] != nums[slow]) {
            slow++;
            nums[slow] = nums[fast];
        } 
    }
    // 最终会打印出数组中 该长度范围内 的所有元素。
    // slow表示的是数组元素下标，长度需要将其+1
    return slow + 1;
};
```