---
title: Z字形变换【Leetcode】
date: 2022-03-01 14:28:50
categories:
- Leetcode
tags:
- 每日一题
---
传送门：[6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

## 题目
将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220301143027.png)

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。

请你实现这个将字符串进行指定行数变换的函数：```string convert(string s, int numRows);```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220301143141.png)

## 解题思路
> 我愿称之为 ‘ 最强flag ’ 

要对字符串```s```进行Z字形存储，所按照顺序如下所示：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220301145304.png)

通过图示可以发现：遍历字符串```s```时，每个字符在Z字形中对应的行索引先从小到大，再从大到小……如此往复，直到遍历结束。

so,问题的难点就在于，如何控制遍历过程的 <b>行索引</b> 呢？
其实通过一个```flag```变量就可以完美解决这个问题。

<b>算法流程：</b>

初始化变量：```res数组--存放对应索引的字符串```、```flag--改变行索引，初始值为-1```、```i--对应的行索引```

1. ```res[i] += c```：把每个字符```c```填入对应行
2. ```i += flag```：更新当前字符```c```对应的行索引
3. ```flag = -flag```：到达Z字形的转折点时，改变方向

## 解题代码
```javascript
var convert = function(s, numRows) {
    if(numRows < 2) return s;
    let res = new Array(numRows).fill('');
    let result = '';//最终结果
    let i=0,flag=-1;
    for(let c of s){
        res[i] += c;
        if(i===0||i===numRows-1) flag = -flag;
        i += flag;
    }
    // console.log(res)
    for(let i of res){
        // console.log(i)
        result += i;
    }
    return result;

};
```