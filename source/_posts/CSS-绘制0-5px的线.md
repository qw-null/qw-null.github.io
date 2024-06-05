---
title: CSS绘制0.5px的线
date: 2022-08-11 17:20:20
tags:
categories:
- CSS样式
tags:
- CSS
---
### 需求：
绘制0.5px的线


### 解决方案：
```html
<div class="line">0.5px的直线</div>
```

### 方法一：scale
```CSS
.line{
  width: 300px;
  background-color: #000;
  height: 1px;
  transform: scaleY(0.5);
}
```

### 方法二：boxshadow
```css
.line{
  height:1px;
  background: none;
  box-shadow: 0 0.5px 0 #000;
}
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220811173548.png)

### 方法三： linear-gradient
```css
.line{
  height: 1px;
  background-image: linear-gradient(0deg, #fff, #000);
}
```
linear-gradient(0deg, #fff, #000);表示从下往上从白色向黑色渐变

### 方法四： viewport(移动端)
```html
<meta name="viewport" content="width=device-width,initial-sacle=0.5">
```
其中width=device-width表示将viewport视窗的宽度调整为设备的宽度，这个宽度通常是指物理上宽度。默认的缩放比例为1时，如iphone 6竖屏的宽度为750px，它的dpr=2，用2px表示1px，这样设置之后viewport的宽度就变成375px。但是我们可以改成0.5，viewport的宽度就是原本的750px，所以1个px还是1px，正常画就行，但这样也意味着UI需要按2倍图的出，整体面面的单位都会放大一倍。


