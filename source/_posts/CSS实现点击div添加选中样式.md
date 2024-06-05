---
title: CSS实现点击div添加选中样式
date: 2022-01-18 14:40:33
tags:
- CSS
---
## 情景:
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220118144305.png)
四个按钮，选中其中一个后，为其添加样式且要保持样式，当选择其他的按钮后，之前选中按钮样式取消，选中按钮样式添加。

## 思路
使用input标签和label标签。
+ input标签表示单选按钮，保证只能有一个按钮可以选中（name相同）。隐藏不显示。
+ label标签用于显示文字和设置选中背景颜色。

实现点击之后才进行切换的效果：
将lable指向相应的input标签的id并使用伪类:checked来实现选择切换的效果。

## 代码
```html
<div>
  <input type="radio" name="catebtn" id="news" checked>
  <label for="news">公司新闻</label>
  <input type="radio" name="catebtn" id="dongtai">
  <label for="dongtai">行业动态</label>
  <input type="radio" name="catebtn" id="baodao">
  <label for="baodao">媒体报道</label>
  <input type="radio" name="catebtn" id="zhishi">
  <label for="zhishi">行业知识</label>
</div>
```
```css
  input{
    display: none;
  }
  label{
    display: inline-block;
    font-size: 1.1rem;
    line-height: 2rem;
    margin: 0 2rem;
    padding: 0 2rem;
    cursor: pointer;
  }
  label:hover{
      background-color: #026eb7;
      color: #fff;
    }
  #news:checked+label,
  #dongtai:checked+label,
  #baodao:checked+label,
  #zhishi:checked+label{
    background-color: #026eb7;
    color: #fff;
  }
```