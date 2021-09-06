---
title: '快速排序[js版]'
date: 2021-09-06 10:28:54
categories:
- 算法基础
tags:
- 每日一题
---

## 介绍
快速排序（Quicksort），简称快排。在平均状况下，排序n个项目要 O(nlogn) 次比较。在最坏状况下则需要O(n<sup>2</sup>)次比较，但这种状况并不常见。

快速排序使用分治法策略来把一个序列（list）分为较小和较大的2个子序列，然后递归地排序两个子序列。

快速排序步骤为：
+ 1.挑选基准值：从数列中挑出一个元素，称为“基准”
+ 2.分割：重新排序数列，所有比基准值小的元素摆放在基准前面，所有比基准值大的元素摆在基准后面（与基准值相等的数可以到任何一边）。在这个分割结束之后，对基准值的排序就已经完成
+ 3.递归排序子序列：递归地将小于基准值元素的子序列和大于基准值元素的子序列排序。
  
> 递归到最底部的判断条件是数列的大小是零或一，此时该数列显然已经有序。

【上述内容参考自[维基百科](https://zh.wikipedia.org/wiki/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F)】

## 实现
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210906102722.jpg)

```javascript

function quickSort(list) {
    //递归终止条件：数组长度为0或1
    if (list.length === 1 || list.length === 0) return list;
    // 取数组中间位置索引以及元素
    let index = Math.floor(list.length / 2);
    let currentItem = list.splice(index, 1);
    // leftList：存储小于中间元素的元素集合
    // rightList：存储大于中间元素的元素集合
    let leftList = [];
    let rightList = [];
    list.forEach(item => {
        if (item <= currentItem) {
            leftList.push(item);
        } else {
            rightList.push(item);
        }
    });
    //打印每一次的流程
    console.log([leftList, currentItem, rightList]);

    return quickSort(leftList).concat(currentItem).concat(quickSort(rightList));
}

let list = [5, 8, 2, 4, 6, 9, 7]
console.log(quickSort(list));
```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210906110756.png)

## JS中的sort排序源码解读
