---
title: React学习笔记
date: 2023-04-21 11:12:06
tags:
  - React
---

[视频链接](https://www.bilibili.com/video/BV1bS4y1b7NV/?share_source=copy_web&vd_source=f3ff8a2761a3e07339584c4852cdd504)

课程代码：链接: https://pan.baidu.com/s/16hEN7j4hLDpd7NoFiS8dHw?pwd=4gxv 提取码: 4gxv 复制这段内容后打开百度网盘手机 App，操作更方便哦。

[课程大纲介绍](https://www.lilichao.com/index.php/2022/05/10/react%e8%a7%86%e9%a2%91%e6%95%99%e7%a8%8b%ef%bc%88alpha%e7%89%88%ef%bc%89/)

## 1.第一部分【1-11】

### 1.1 变量声明

结论：<b style="color:red">三种声明方式中，第一优先使用的是 const，如果希望变量被改变则使用 let，至于 var 最好不要在代码中使用</b>

- var - 没有块级作用域
- let - 有块级作用域
- const - 和 let 类似，具有块级作用域，但是它只能赋值一次，使用场景：a.对于一些常量使用 const 声明、b.对于一些对象（函数）也可以使用 const，可以避免对象或者函数被意外修改

```js
const obj = { name: "John" };
obj.age = 18;

// obj = {grade:6} 报错，原因：const变量二次赋值会报错
```

### 1.2 解构赋值
