---
title: 谈一谈BFC
date: 2022-07-29 15:18:38
tags:
- CSS
---
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220729154722.png)

## 1.何为BFC？
BFC（Block Formating Context）块级格式化上下文，是Web页面的一块独立的渲染区域，具有BFC特性的元素可以看成是一个独立的容器，容器内部的元素在布局上不会影响到外面的元素。

## 2.触发BFC的条件
+ 根元素（```<html>```）
+ ```float```不为```none```的元素
+ ```position```为```fixed```或```absolute```的元素
+ ```overflow```不为```visible```的元素
+ ```display```为```inline-block```、```table-cell```、```flex```等的元素

## 3.同一个BFC内的布局规则
1. box会在垂直方向上一个接一个放置
2. 垂直方向上的距离由```margin```决定，属于同一个BFC的两个相邻的box的```margin```会发生重叠，以最大的```margin```为合并值
3. BFC内部每个元素的```margin-left```与父节点的左边接触（对于从左往右格式化而言，否则反之）
4. BFC区域不会与float元素的区域重叠
5. 计算BFC元素高度时，其浮动子元素也参与计算

## 4.BFC的应用
1. 分属于不同的BFC时，可以放置```margin```重叠
2. 清除内部浮动（给父元素开启BFC）
3. 适应多栏布局


