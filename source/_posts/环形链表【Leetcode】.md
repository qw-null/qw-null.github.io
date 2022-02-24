---
title: 环形链表【Leetcode】
date: 2022-02-24 22:17:52
categories:
- Leetcode
tags:
- 每日一题
---
## 题目
给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220224221951.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220224222027.png)

## 解题思路
题目需要解决两个问题：
1. 如何判断链表中有环？
2. 链表存在环，如何查找到入口？

<b>如何判断链表中有环？</b>
可以采用快慢指针法，慢指针一次走一步，快指针一次走两步，如果链表中存在环，两指针一定会在环内相遇。

<b>链表存在环，如何查找到入口？</b>

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/微信图片编辑_20220224221327.jpg)

这表明，<b>从头结点出发一个指针，从相遇结点 也出发一个指针，这两个指针每次只走一个结点， 那么当这两个指针相遇的时候就是 环形入口的结点。</b>

## 实现代码

```javascript
var detectCycle = function(head) {
    let [fast,slow] = [head,head];
    while(fast && fast.next){
        // slow+1，fast+2，一定会相遇，相遇就要抓住相遇点
        slow = slow.next;
        fast = fast.next.next;
        if(slow == fast){
            let meet = fast; // 相遇点
            let start = head; // 开始节点
            while(meet !== start){
                meet = meet.next;// 相遇点指针
                start = start.next; // 头节点
            }
            return meet;
        }
    }
    return null;
};
```
