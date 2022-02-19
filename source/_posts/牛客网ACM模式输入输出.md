---
title: 牛客网ACM模式输入输出
date: 2022-02-14 22:03:09
categories:
- 牛客网
tags:
- 牛客网输入输出
---

> 凡事预则立，不预则废。

## A + B (1)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220214221602.png)

```javascript
while(line = readline()){
    let lines = line.split(' ');
    let a = parseInt(lines[0]);
    let b = parseInt(lines[1]);
    print(a+b);
}
```

## A + B (2)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220214222656.png)

```javascript
let len = readline();
while(len--){
    let lines = readline().split(' ');
    let a = parseInt(lines[0]);
    let b = parseInt(lines[1]);
    print(a+b);
}
```

## A + B (3)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220214224250.png)

```javascript
while(line = readline()){
    let lines = line.split(' ');
    let a = parseInt(lines[0]);
    let b = parseInt(lines[1]);
    if(a===0 && b===0) break;
    print(a+b);
}
```

## A + B (4)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220215231917.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220215232009.png)

```javascript
while(line = readline()){
    let lines = line.split(' ');
    let len = parseInt(lines[0]);
    if(len === 0) break;
    let res = 0;
    for(let i=1;i<=len;++i){
        res += parseInt(lines[i]);
    }
    print(res);
}
```

## A + B (5)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220215232728.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220215232745.png)

```javascript
let len = readline();
while(len--){
    let lines = readline().split(' ');
    let end = parseInt(lines[0]);
    let res = 0;
    for(let i=1;i<=end;++i){
        res += parseInt(lines[i]);
    }
    print(res);
}
```
