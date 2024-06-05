---
title: JS分割取字符串方法
date: 2021-06-15 21:22:00
categories:
- JS学习
tags:
- JavaScript
---
### 1.substr
substr(start,length)表示从start位置开始，截取length长度的字符串。
```javascript
var src="images/off_1.png";
alert(src.substr(7,3));
```
弹出值:off

### 2.substring
substring(start,end)表示从start到end之间的字符串，包括start位置的字符但是不包括end位置的字符。
```javascript
var src="images/off_1.png";
alert(src.substring(7,10));
```
弹出值:off

### 3.indexOf
indexOf() 方法返回某个指定的字符串值在字符串中首次出现的位置（从左向右）。没有匹配的则返回-1，否则返回首次出现位置的字符串的下标值。
```javascript
var src="images/off_1.png";
alert(src.indexOf('t'));
alert(src.indexOf('i'));
alert(src.indexOf('g'));
```
弹出值依次为: -1 &nbsp;  0  &nbsp; 3

### 4.lastIndexOf
lastIndexOf()方法返回从右向左出现某个字符或字符串的首个字符索引值（与indexOf相反）。
```javascript
var src="images/off_1.png";
alert(src.lastIndexOf('/'));
alert(src.lastIndexOf('g'));
```
弹出值依次为：6 &nbsp; 15

### 5.split
将一个字符串分割为子字符串，然后将结果作为字符串数组返回。
```javascript
function SplitDemo(){
  var s, ss;
  var s = "The rain in Spain falls mainly in the plain.";
  // 在每个空格字符处进行分解。
  ss = s.split(" ");
  return(ss);
}
```
结果：["The", "rain", "in", "Spain", "falls", "mainly", "in", "the", "plain."]