---
title: JS手写:利用 XMLHttpRequest 手写 AJAX 实现
date: 2022-09-06 09:54:34
tags:
- 各种无厘头手写
---
题目：利用 XMLHttpRequest 手写 AJAX 实现

```javascript
function getAjax (url) {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open('GET', url, false);
        xhr.setRequestHeader("Content-Type", "application/json");
        xhr.onreadystatechange = function () {
            if (xhr.readyState !== 4) return;
            if (xhr.status === 200 || xhr.status ===304) {
                resolve(xhr.responseText);
            } else {
                reject(new Error(xhr.responseText));
            }
        };
        xhr.send();
    });
};
```
```javascript
// 测试
getAjax('https://www.baidu.com');
```
结果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220906095813.png)

