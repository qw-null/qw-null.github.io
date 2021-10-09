---
title: JavaScript练习题【牛客网】（持续使用中）
date: 2021-10-09 09:23:05
categories:
- JS学习
tags:
- JavaScript
---

#### 1.JavaScript定义var a="30",var b=8,则执行 a%b 会得到（）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211009092934.png)

【解析】
B
运算中‘+’号，数字隐式转换成字符串。其余的运算符号是字符串隐式转换成数字。

#### 2.下面这段JS程序的执行结果是：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211009093457.png)
A. [1,2,3,4]
B. [1,2,3]
C. [4]
D. [2,3,4]

【解析】
B
JS中slice()方法是选取数组的的一部分，并返回一个新数组。

#### 3.执行以下程序段后，x的值是（ ）。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211009093823.png)
A. 1
B. 2 
C. 3
D. 4

【解析】
C
switch中没有break

#### 4.以下代码执行后， num 的值是？
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211009094034.png)
A. -1
B. 3 
C. 1
D. 2

【解析】
A
```javascript
// 1.匿名函数，需要通过变量引用指向函数的运行结果。
var foo=function(x,y){      //赋值式函数
return x-y;
} 

//2.有名函数，可以单独定义。
function foo(x,y){         //声明式函数
return x+y;
}

//实际调用
var num=foo(1,2); //调用赋值式函数  return x-y  为  -1
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211009094556.png)
知识点：[js变量提升与函数提升的详细过程](https://www.cnblogs.com/lvonve/p/9871226.html)



