---
title: ByteDance的CSS手写题
date: 2022-07-24 14:15:23
tags:
- 各种无厘头手写
---

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220724141358.png)

要求：HTML代码不能变动，只能写CSS代码。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220724144459.png)

<iframe width="100%" height="350" src="//jsrun.net/7ZPKp/embedded/all/dark" allowfullscreen="allowfullscreen" frameborder="0"></iframe>


### flex布局知识点
#### 1.容器属性
+ flex-direction:指定容器中元素的排列方式（flex主轴的排列方向）
-row 默认值，弹性元素在容器中水平排列（自左向右）
-row-reverse 弹性元素在容器中反向水平排列（自右向左）
-column 弹性元素纵向排列（自上向下）
-column-reverse 弹性元素反向纵向排列（自下向上）

<br>

+ flex-wrap:设置元素在容器中换行
-nowrap 默认值，元素不会自动换行
-wrap 元素沿着辅轴方向自动换行
-wrap-reverse 元素沿着辅轴反方向换行

<br>

+ flex-flow = flex-direction + flex-wrap

+ justify-content:定义元素在主轴上的排序方式
![justify-content取值](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220117135730.png)

<br>

+ align-items:定义元素在辅轴上的排序方式
![align-items取值](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220117135907.png)

*（元素属性align-self可以用来覆盖当前弹性元素上的align-items，设置单个元素的排列方式）*

#### 2.元素属性
+ order:定义子元素或者子容器的排列顺序。数值越小，排列越靠前，默认为0。使用代码：```.item { order: xxx; }```
![order](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220117140405.png)

<br>

+ flex-grow:定义子元素的或子容器的放大比例，默认值为0（原始尺寸）。使用代码:```.item { flex-grow: xxx;} ```

<br>

+ flex-shrink:定义子元素的或子容器的放大比例，默认值为1（即如果空间不足，该项目将缩小。）。使用代码:```.item { flex-shrink: xxx;} ```

<br>

+ flex-basis:定义了在分配多余空间之前，项目占据的主轴空间（main size）。它的默认值为auto，即项目的本来大小。使用代码:```.item:{ flex-basis:xxx;}``` 它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。

<br>

+ align-self:允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。
![align-self](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220117140115.png)


### 元素透明度设置

+ opacity：元素整体透明度都会设置，包括文字，内容所有内容。

+ rgba：rgba(red, green, blue, alpha)，其中alpha为透明度设置。这个透明度只会影响颜色，不会影响其他元素。




