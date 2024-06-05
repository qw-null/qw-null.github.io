---
title: CSS奇淫技巧
date: 2021-09-22 16:17:12
categories:
- CSS样式
tags:
- CSS
---
CSS奇淫技巧

### 1.提示框
效果图：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210922164322.gif)
代码：
```html
<div class="bruce flex-ct-y" data-title="使用attr()抓取节点属性">
	<a class="hover-tips btn-1" href="https://www.baidu.com" data-msg="Hello World">提示框</a>
</div>
```
```css
.hover-tips {
	position: relative;
	padding: 0 20px;
	border-radius: 10px;
	height: 40px;
	background-color: #66f;
	line-height: 40px;
	color: #fff;
	&.btn-1 {
		&::after {
			position: absolute;
			left: 0;
			top: 0;
			border-radius: 5px;
			width: 100%;
			height: 100%;
			background-color: rgba(#000, 0.5);
			opacity: 0;
			text-align: center;
			font-size: 12px;
			content: attr(data-msg);
			transition: all 300ms;
		}
		&:hover::after {
			left: calc(100% + 20px);
			opacity: 1;
		}
	}
}

```
在按钮上触发悬浮状态:hover时，通过attr()获取节点的data-msg并赋值到::after的content上

### 2.绘制三角形
```html
<div id="triangle"></div>
```
```css
#triangle{
  width:0;
  height:0;
  border-left:50px solid red;
  border-top:50px solid transparent;
}
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220105102020.png)

```css
#triangle{
  width:0;
  height:0;
  border-left:50px solid red;
  border-top:50px solid green;
}
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220105102125.png)
```css
#triangle{
  width:0;
  height:0;
  border-left:50px solid red;
  border-top:50px solid green;
  border-right:50px solid blue;
  border-bottom:50px solid yellow;
}
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220105102312.png)

### 3.实现点击div添加选中样式
[实现点击div添加选中样式](https://qw-null.github.io/2022/01/18/CSS%E5%AE%9E%E7%8E%B0%E7%82%B9%E5%87%BBdiv%E6%B7%BB%E5%8A%A0%E9%80%89%E4%B8%AD%E6%A0%B7%E5%BC%8F/#%E6%83%85%E6%99%AF)