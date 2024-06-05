---
title: 排序算法
date: 2021-09-06 10:28:54
categories:
- 算法基础
---

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220829150312.png)

## 1.快速排序
快速排序（Quicksort），简称快排。在平均状况下，排序n个项目要 O(nlogn) 次比较。在最坏状况下则需要O(n<sup>2</sup>)次比较，但这种状况并不常见。

快速排序使用分治法策略来把一个序列（arr）分为较小和较大的2个子序列，然后递归地排序两个子序列。

快速排序步骤为：
+ 1.挑选基准值：从数列中挑出一个元素，称为“基准”
+ 2.分割：重新排序数列，所有比基准值小的元素摆放在基准前面，所有比基准值大的元素摆在基准后面（与基准值相等的数可以到任何一边）。在这个分割结束之后，对基准值的排序就已经完成
+ 3.递归排序子序列：递归地将小于基准值元素的子序列和大于基准值元素的子序列排序。
  
> 递归到最底部的判断条件是数列的大小是零或一，此时该数列显然已经有序。

【上述内容参考自[维基百科](https://zh.wikipedia.org/wiki/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F)】
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210906102722.jpg)

```javascript
function quickSort(arr) {
    //递归终止条件：数组长度为0或1
    if (arr.length<=1) return arr;
    // 取数组中间位置索引以及元素
    let index = Math.floor(arr.length / 2);
    let pivot = arr[index]; //  直接取数据即可，没必要采用该方式let pivot = arr.splice(index, 1)
    // left：存储小于中间元素的元素集合
    // right：存储大于等于中间元素的元素集合
    let left = [];
    let right = [];
    for(let i=0;i<arr.length;++i){
        if(arr[i]<pivot) left.push(arr[i]);
        else if(arr[i]>pivot) right.push(arr[i]);
    }
    return quickSort(left).concat(pivot,quickSort(right));
}
```

## 2.冒泡排序
冒泡排序是一种最基础的交换排序。之所以叫做冒泡排序，因为每一个元素都可以像小气泡一样，根据自身大小一点一点向数组的一侧移动。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220816212759.png)

```javascript
function bubbleSort( arr ) {
    // 冒泡排序
    for(let i=arr.length-1;i>=1;i--){
        for(let j=1;j<=i;j++){
            if(arr[j-1]>arr[j])
                [arr[j-1],arr[j]] = [arr[j],arr[j-1]];
        }
    }
    return arr;
}
```

## 3.归并排序

```javascript
function merge(l,r){
    let temp = [];
    while(l.length>0&&r.length>0){
        if(l[0]<r[0]) temp.push(l.shift());
        else temp.push(r.shift());
    }
    return temp.concat(l).concat(r);
}

function mergeSort(arr){
    if(arr.length<=1) return arr;
    let mid = arr.length>>1;
    let left = mergeSort(arr.slice(0,mid));
    let right = mergeSort(arr.slice(mid));
    return merge(left,right);
}
```


