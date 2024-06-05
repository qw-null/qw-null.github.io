---
title: CSS中的三列布局
date: 2022-07-09 17:54:25
categories:
- CSS样式
tags:
- CSS
---

三列布局的要求一般为：
1. 左右两边宽度固定，中间宽度自适应。
2. 中间列的内容可以完整显示。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220712100505.png)

## 1. 定位方式

<iframe width="100%" height="300" src="//jsrun.net/SBzKp/embedded/all/dark" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 2. flex 布局

<iframe width="100%" height="300" src="//jsrun.net/4BzKp/embedded/all/dark" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 3.圣杯布局
圣杯布局的步骤：
1. 首先给```left```、```middle```、```right```设置上```float: left```, 脱离文档流；
2. 给container(父元素)设置上```overflow: hidden``` 可以形成BFC撑开文档
3. ```left```、```right```设置上各自的宽度，```middle```设置```width: 100%```
🌟 接下来比较重要了：
4. 给```left```、```middle```、```right```设置```position: relative```
5. ```left```设置 ```left: -leftWidth```,```margin-left:-100%```，```right```设置 ```right: -rightWidth```,```margin-left:-rightWidth```
6. ```container```设置```padding: 0, rightWidth, 0, leftWidth```

<iframe width="100%" height="400" src="//jsrun.net/uBzKp/embedded/all/dark" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 4.双飞翼布局
双飞翼布局的步骤：
1. 首先给```left```、```main```、```right```设置上```float: left```, 脱离文档流；```main```中增加```main-contain```
2. 给```container```设置上```overflow: hidden``` 可以形成BFC撑开文档
3. ```left```、```right```设置上各自的宽度，```main```设置```width: 100%```
🌟 接下来与圣杯布局不一样的地方：
4. ```left```设置 ```margin-left: -100%```, ```right```设置 ```margin-left: -rightWidth```,```container```不再设置```padding```
5. ```main-content```设置```margin: 0 rightWidth 0 leftWidth```

<iframe width="100%" height="400" src="//jsrun.net/YRzKp/embedded/all/dark" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 5.圣杯布局和双飞翼布局

**✨实现的功能**
要求中间内容优先渲染，左右内容宽度固定，中间主要内容宽度自适应排布

**✨区别**
**1.表现形式上**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220709231825.png)
圣杯布局是中间栏为两边腾开位置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220709231855.png)
双飞翼布局则是中间栏不变，将内容部分为两边腾开位置

**2.代码结构**
双飞翼布局中间层多了一层```div```标签