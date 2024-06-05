---
title: 手写LRU
date: 2022-07-22 16:21:17
tags: 
- 各种无厘头手写
---

## LRU（Least Recently Used）最近最少使用
LRU是一个非常经典的算法，一般可用如下场景描述：我们访问了很多个页面，使用内存去存储这些页面，随着时间的推移，页面越来越多，但是内存空间是有限的，所以需要删除一些页面，删除页面基于的策略是 **删除最近最久没有使用过的**页面。

手写LUR需要注意如下场景的问题：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220722170756.png)

1. 访问已经存在的页面：先删除该页面，在重新添加
2. 访问新的页面：若未超内存长度，直接放入；若超出内存长度，删除第一个，放入新的


```javascript
class LRUCache{
  constructor(length){
    this.length = length;
    this.data = new Map();
  }

  set(key,value){
    let data = this.data;
    //若存在内存中：删除原来的，重新放入
    if(data.has(key)){
      data.delete(key);
    }
    data.set(key,value);

    //若超出内存长度:删除第一个元素
    if(data.size>this.length){
      let delKey = data.keys().next().value;
      data.delete(delKey);
    }  
  }

  get(key){
    let data = this.data;
    // 未找到
    if(!data.has(key)) return null;
    // 找到：取值，删除，重新写入
    const value = data.get(key);
    data.delete(key);
    data.set(key,value);
  }
}
```
