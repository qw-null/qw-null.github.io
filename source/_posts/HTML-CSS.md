---
title: HTML+CSS学习笔记
date: 2021-07-18 22:20:47
tags:
- HTML
- CSS
---

[【尚硅谷】Web前端零基础入门HTML5+CSS3基础教程丨初学者从入门到精通](https://www.bilibili.com/video/BV1XJ411X7Ud?p=1)

[+ 课程配套资料 +](https://github.com/qw-null/Web-HTML5-CSS3-/tree/master/%E8%AF%BE%E7%A8%8B%E8%B5%84%E6%96%99)

网页分成三部分：
- 结构HTML
- 表现CSS
- 行为JavaScript

# 一、HTML
HTML(Hypertext Markup Language)  超文本标记语言。

## 1.标签
标签一般成对出现，但是存在一些自结束标签（单标签），如：
```html
<img>、<img/>、<input>、<input/>
```
## 2.注释
注释不能嵌套
```html
<!-- 注释内容 -->
```
## 3.标签的属性（P7）
在标签（开始标签或自结束标签）还可以设置属性;
属性和标签中的内容应该使用空格隔开
```html
<font color="red"> 标签属性 </font>
<!-- color为font标签的属性 -->
```

## 4.文档声明 (P8)
- 文档声明用来告诉浏览器当前网页版本
- html5的文档声明 
  ```html
  <!doctype html> / <!DOCTYPE HTML>
  ```

## 5.字符编码（P10）
```html
<!-- 网页字符集通过<meta>设置
<meta>需要放在<head></head>中 -->
<head>
    <meta charset="utf-8">
</head>
```

## 6.实体（转义字符）
```html
实体语法：
- &实体名字;
(空格：&nbsp;
大于号：&gt;
小于号：&lt;
版权符号：&copy;)
```

## 7.meta标签
meta主要用于设置网页中的一些元数据，元数据不是给用户看的
charset - 指定网页的字符集
name - 指定的数据的名称 
content - 指定的数据的内容
```html
<meta name="keywords" content="HTML5,前端,CSS3">
 
keywords表示网站的关键字，可以同时指定多个关键字，关键字之间使用，隔开

description 用于指定网站的描述
```

## 8.语义化标签 + 块元素、行内元素（P16-P18）
块元素：在页面中独占一行的元素（block element），网页中一般通过块元素进行布局

+ p标签表示页面中的一个段落，块元素
+ h1~h6标签，标题，块元素
+ hgroup标签用来为标题分组，可以将一组相关的标题同时放入到hgroup

行内元素：在页面中不会独占一行的元素（inline element），主要用于包裹文字
+ em标签表示语音语调的加重,行内元素
+ strong标签表示强调 

一般情况下，会在块元素中放置行内元素，而不会在行内元素中放置块元素；
块元素中基本什么元素都能放，特殊情况：p元素中不能放置块元素

## 9.列表list（P19）
html中列表一共三种：
+ 有序列表
+ 无序列表
+ 定义列表
```html
<!-- 1.有序列表 -->
<ol>
    <li>结构</li>
    <li>表现</li>
    <li>行为</li>
</ol>
<!-- 2.无序列表 -->
<ul>
    <li>结构</li>
    <li>表现</li>
    <li>行为</li>
</ul>
<!-- 3.定义列表 -->
<dl>
    <dt>结构</dt>
    <dd>结构表示网页的结构，结构用来规定网页中哪里是标题，哪里是段落</dd>
    <dd>结构表示网页的结构，结构用来规定网页中哪里是标题，哪里是段落</dd>
    <dd>结构表示网页的结构，结构用来规定网页中哪里是标题，哪里是段落</dd>
</dl>
```
定义列表效果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211011141506.png)

##  10.超链接（P20）
超链接：可以从一个页面跳转到其他页面，或者当前页面的其他位置
使用a标签定义超链接
  属性：
    href → 指定跳转的目标路径
    target → 用来指定超链接打开的位置
   ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220316192446.png)

超链接，行内元素，在a标签中可以嵌套除自身外的任何元素
```html
<a href="#"></a>的作用和<a href="javascript:;"></a>相同
```
### 相对路径（P21）
当需要跳转到服务器内部的页面是，一般使用相对路径，相对路径都会使用.或..开头，如果不写./或../则相当于写了./
./表示当前文件所在的目录
../表示当前文件所在目录的上一级目录

## 11.图片标签（P23,P24）
img标签是一个自结束标签
属性：
  + src属性指定的是外部图片路径（路径规则和超链接相同）
  + width 图片宽度
  + height 图片高度
    -宽度和高度如果只修改了一个，则另一个会等比例缩放

图片格式：
+ jepg(jpg)-支持的颜色比较丰富，不支持透明效果，不支持动图
+ gif-支持的颜色比较少，支持简单透明，支持动图
+ png-支持的颜色丰富，支持复杂透明，不支持动图
+ webp-这种格式是谷歌新推出的专门用来表示网页中的图片的一种格式，具备其他图片格式的所有优点，而且文件特别小/缺点：兼容性差/
+ base64-将图片使用base64进行编码，这样可以直接通过字符形式引入图片，一般用于需要和网页同时加载的图片才会使用base64

## 12.内联框架（P25）
iframe标签，用于在当前页面中引入其他页面
属性：
- src指定要引入的网页的路径 
- frameborder 指定内联框架的边框

## 13.音视频播放（P26）
音视频文件引入时，默认情况下不允许用户自己控制播放停止。

audio标签 用来向页面中引入外部音频文件
属性：controls 是否允许用户控制播放
autoplay 音频文件是否自动播放
loop 音频文件是否循环播放

```html
<audio src="***" contorls></audio>
```

video标签 引入视频文件
使用方式和audio标签一致

video标签示例：
```html
<video controls width="250">

    <source src="/media/cc0-videos/flower.webm"
            type="video/webm">

    <source src="/media/cc0-videos/flower.mp4"
            type="video/mp4">

    Sorry, your browser doesn't support embedded videos.
</video>
```
当视频的媒体数据加载期间发生错误时执行 JavaScript :
```html
<video onerror="myFunction()">
```

## 14.表格（P96-P97）
网页中通过table标签来创建表格
在table中使用tr表示表格中的一行，有几个tr就有几行，在tr中使用td表示一个单元格，有几个td就有几个单元格
colspan 横向的合并单元格； rowsoan 纵向合并单元格

可以将一个表格分为三部分：
1. 头部 thead
2. 主体 tbody
3. 底部 tfoot

th 表示头部单元格

#### 14.1 表格的样式（P98）
```css
table{
  width: 50%;
  border: 1px solid black;
  margin: 0 auto;

  /* 1.border-spacing 指定边框之间的距离 */
  /* 此时边框的宽度为2px */
  border-spacing：0px; 

  /* 2.border-collapse: collapse; 设置边框的合并 */
   /* 此时边框的宽度为1px */
  border-collapse: collapse;
}

table tr:nth-child(2n){
  /* 调整表格偶数行的背景颜色 */
  background-color: green;
}
```
如果表格中没有使用tbody而直接使用tr，那么浏览器会自动创建一个tbody，并且将所有的tr都放到tbody中，因此tr不再是tbody的子元素
```css
table > tr {
  /* 此时效果无法生效 */
  background-color: red;
}
```

默认情况下元素在td中是垂直居中的，可以通过 vertical-align 来修改

## 15.表单（P99）
网页中的表单用于将本地的数据提交给远程的服务器，使用form标签来创建一个表单
form的属性：
+ action 表单要提交的服务器的地址
表单项：
```html
<!-- 文本框 -->
<input type="text" name="数据提交给服务器必须指定唯一的名称">

<!-- 提交按钮 -->
<input type="submit" value="指定显示文字">

<!-- 密码框 -->
<input type="password" name="**">

<!-- 单选按钮 -->
<!-- 像这种选择框，必须要指定一个value属性，value属性最终会作为用户的填写的值传递给服务器 -->
<!-- checked可以将单选按钮设置为默认选中 -->
<input type="radio" name="**" value="**" checked>

<!-- 多选框 -->
<input type="checkbox" name="**" value="**" checked>

<!-- 下拉列表 -->
<!-- selected 属性设置默认选中 -->
<select>
  <option value="1" selected>选项1</option>
  <option value="2">选项2</option>
  <option value="3">选项3</option>
</select>

<!-- 重置按钮 -->
<input type="reset">

<!-- 普通按钮 -->
<input type="button" value="按钮">
```
autocomplete="off" 关闭自动补全
readonly 将表单项设置为只读，数据会提交到服务器端
disabled 将表单项设置为禁用，数据不会提交到服务器端
autofocus 设置表单项自动获取焦点

# 二、CSS（P27-P28）

CSS 层叠样式表
网页实际上是一个多层结构，通过CSS可以分别为网页的每一层来设计样式，而最终我们能看到的只是网页的最上边一层
使用方式：

+ 内联样式/行内样式：在标签内部通过style属性来设置元素样式
+ + 问题：样式只对一个标签生效、希望影响多个元素必须在每一个元素中都复制一遍，并且样式发生变化时，我们必须要一个一个的修改，不方便维护
+ + 注意：开发时绝对不要使用内联样式
---
+ 内部样式表：将样式编写到head中的style标签里然后通过CSS选择器来选中元素并为其设置各种样式，可以同时为多个标签设置样式，并且修改时只需要修改一处即可全部应用
+ 内部样式更加方便对样式进行复用
+ + 问题：内部样式表只能对一个网页起作用，内部样式不能跨页面复用
---
+ 外部样式表：将CSS样式编写到外部CSS文件中，然后通过link标签引入外部CSS文件
```html
<link rel="stylesheet" href="****">
```
+ 意味着只要想使用这些样式的网页都可以对其进行引用，使样式可以在不同页面之间进行复用
+ 将样式编写到外部的CSS文件中，可以使用到浏览器的缓存机制，从而加快网页的加载速度，提高用户的体验

## 1.CSS基本语法（P29）
CSS注释 - /* 注释内容 */
CSS的基本语法：
  选择器 声明块
  + 选择器：通过选择器可以选中页面中的指定元素（比如p的作用是选中页面中所有的p元素）
  + 声明块：通过声明块来指定要为元素设置的样式。
  + + 声明块由一个一个的声明组成
  + + 声明是一个名值对结构,一个样式名对应一个样式值，名和值之间以:连接，以;结尾

## 2.常用选择器（P30）
1.元素选择器
+ 作用：根据标签名来选中指定的元素
+ 语法：标签名{}
+ 例子：p{}、 h1{} 、div{}

2.id选择器
+ 作用：根据元素的id属性值选中一个元素
+ 语法：#id属性值{}
+ 例子：#box{}、#red{}

3.类选择器
class是一个标签属性，它和id类似，不同的是class可以重复使用，通过class属性可以为元素分组,可以同时为一个元素指定多个class属性
+ 作用：根据元素class属性值选中一组元素
+ 语法：.class属性值{}

4.通配选择器
+ 作用：选中页面中的所有元素
+ 语法：*{}

## 3.复合选择器（P31）
1.交集选择器
+ 作用：选中同时符合多个条件的元素
+ 语法：选择器1选择器2选择器3……选择器n{}
+ + 注意点：交集选择器中如果有元素选择器，必须使用元素选择器开头（例如：<strong>div.red{}</strong>不能写成<strong>.reddiv{}</strong>）

2.选择器分组（并集选择器）
+ 作用：同时选择多个选择器对应的元素
+ 语法：选择器1,选择器2,选择器3,……,选择器n{}（例如：#b1,.p1,h1,span,div.red{}）

## 4.关系选择器（P32）
父元素--直接包含子元素的元素叫做父元素
子元素--直接被父元素包含的元素是子元素
祖先元素--直接或间接包含后代元素的元素叫做祖先元素、--一个元素的父元素也是它的祖先元素
后代元素--直接或间接被祖先元素包含的元素叫做后代元素
兄弟元素--拥有相同父元素的元素是兄弟元素

1.子元素选择器
+ 作用：选中指定父元素的指定子元素
+ 语法：父元素 > 子元素{}

2.后代元素选择器
+ 作用：选中指定元素内的指定后代元素
+ 语法：祖先 &nbsp; 后代{}（例如：div &nbsp; span {}）

3.兄弟元素选择器
+ 作用：选择下一个兄弟元素
+ 语法：前一个+下一个{}(只有两个元素紧挨着才会生效)
``` html
  <div>1<div>
  <span>紧跟着1<span>
  <div>2</div>
  <p></p>
  <span>非紧挨着2</span>

  div + span{
    color:red;
  }
  <!-- 只有 紧跟着1 颜色是红色 -->
```

4.选择下面所有的兄弟元素
+ 语法：兄 ~ 弟{}

## 5.属性选择器（P33）
[属性名] 选择含有指定属性的元素
[属性名=属性值] 选择含有指定属性和属性值的元素
[属性名^=属性值] 选择属性值以指定值开头的元素
[属性名$=属性值] 选择属性值以指定值结尾的元素
[属性名*=属性值] 选择属性值中含有某值的元素

```css
  p[title]{
    color:red;
  }

  p[title=abc]{
    color:red;
  }

  p[title^=abc]{
    color:red;
  }

  p[title$=abc]{
    color:red;
  }

  p[title*$*=abc]{
    color:red;
  }
```

## 6.伪类选择器
1. 伪类（不存在的类，特殊的类） （P34）
- 伪类用来描述一个元素的特殊状态，比如：第一个子元素、被点击元素、鼠标移入的元素……
- 伪类一般情况下都是使用:开头，如 
- - :first-child 第一个子元素
- - :last-child 最后一个子元素
- - :nth-child(n) 选中第n个子元素(特殊值：<strong>1.</strong> n,第n个，n的范围0到正无穷；<strong>2.</strong>2n或even 表示选中偶数位的元素；<strong>3.</strong>2n+1或odd 表示选中奇数位的元素)

以上这些伪类都是根据所有元素进行排序
---
- - :first-of-type
- - :last-of-type
- - :nth-of-type()

这几个伪类的功能和上述的类似，不同点是它们在同类型元素中进行排序
---
- - :not() 否定伪类：将符合条件的元素从选择器中去除
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220103175630.png)
将所有的p标签背景都换成黄色，除了class为hello的

2.a元素的伪类（P35）
+ :link 用于选取未被访问的链接
+ :visited 用来表示访问过的链接（由于隐私的原因，所以visited这个伪类只能修改链接的颜色）
上述两个伪类只对a元素生效
+ :hover 用来表示鼠标移入的状态
+ :active 用来表示鼠标点击的状态

## 7.伪元素选择器（P36）
伪元素，表示页面中一些特殊的并不真实存在的元素（特殊位置），伪元素使用::开头
+ ::first-letter 表示第一个字母
+ ::first-line 表示第一行
+ ::selection 表示选中的内容
---
+ ::before 元素的开始位置
+ ::after 元素的最后位置

注意：before 和 after 必须结合content属性来使用

#### [练习项目：餐厅选择器]
[餐厅选择器](https://github.com/qw-null/CSS-selector-practice/tree/master)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211015160542.png)

## 8.样式的继承（P38）
样式的继承，我们为一个元素设置的样式同时也会应用到它的后代元素上
```html
<p>
  我是一个P元素
  <span>我是P元素中的span</span>
</p>
<span>我是P元素外的span</span>
```
```css
p{
  color:red;
}
```
效果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211016152351.png)

注意：并不是所有的样式都会被继承（比如：背景相关的，布局相关等的这些样式都不会被继承）

## 9.选择器的权重（P39）
样式的冲突：
+ 当我们通过不同的选择器，选中相同的元素，并且为相同的样式设置不同的值，此时就发生了样式的冲突

发生样式冲突时，应用哪个样式由选择器的权重（优先级）决定
选择器权重：
+ 内联样式--1000
+ id选择器--100
+ 类和伪类选择器--10
+ 元素选择器--1
+ 通配选择器--0
+ 继承的样式--没有优先级

比较优先级时，需要将所有的选择器的优先级进行相加，最后优先级越高，则越优先显示（分组选择器是单独计算的）
选择器的累加不会超过其最大的数量级（类选择器再高也不会超过id选择器）
如果优先级计算相同，此时优先使用靠下的样式
可以在某一个样式后面添加!important，此时该样式会获得最高的优先级，甚至超过内联样式【注意：在开发中，一定要慎用！】

## 10.长度单位
#### 像素和百分比（P40）
+ 像素：屏幕（显示器）实际上是由一个个的小点构成的，不同屏幕的像素大小是不同的，像素越小的屏幕显示效果越清晰，所以同样的200px在不同的设备下显示效果不同
+ 百分比：也可以将属性值设置为相对于其父元素（*这种说法不严谨*）属性的百分比

#### em和rem（P41）
+ em：em是相对于元素的字体大小来计算，1em=1font-size,em会根据字体大小的改变而改变、
+ rem：rem是相对于根元素的字体大小来计算

## 11.文档流（P44）
- 网页是一个多层结构，一层摞着一层，通过CSS可以分别为每一层来设置样式，作为用户只能看到最顶层内容，在这些层中，最底下一层称为文档流，文档流是网页的基础
- 我们所创建的元素默认都是在文档流中进行排列
- 元素主要是有两个状态
- - 在文档流中
- - 不在文档流中（脱离文档流）
  
##### 元素在文档流中的特点：
- 块元素
- - 块元素在文档流中独占一行
- - 默认宽度是父元素的全部（会把父元素撑满）
- - 默认高度是被内容撑开（子元素）
---
- 行内元素
- - 行内元素不会独占页面的一行，只占自身大小
- - 行内元素在页面中由左向右水平排列，如果一行之中不能容纳下所有行内元素，则元素会换到第二行继续自左向右排列
- - 行内元素的默认宽度和高度都是被内容撑开

## 12.盒模型 box model（P45）
CSS将页面中的所有元素都设置为一个矩形的盒子，将元素设置为矩形的盒子后，对页面布局就变成将不同的盒子摆放到不同的位置
每一个盒子都由内容区（content）、边框（border）、内边距（padding）、外边距（margin）  组成
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211018114632.png)
- 内容区（content）：元素中所有的子元素和文本内容都在内容区中排列。
- - 内容区的大小有width和height两个属性来设置
---
- 边框（border）：边框属于盒子边缘。*边框的大小会影响到整个盒子的大小*（P46）
- - 设置边框需要设置三个样式：边框宽度（border-width）、边框颜色（border-color）、边框样式（border-style）
-  边框宽度（border-width），默认值一般都是3个像素，可以用来指定四个方向的边框的宽度
- - <b>四个值：</b>上、右、下、左；<b>三个值：</b>上、左右、下；<b>两个值：</b>上下、左右
- - 除了border-width还有一组border-xxx-width，其中xxx可以是left、right、top、bottom，用来单独指定某一个边的宽度
- border-style指定边框的样式，默认值为none
- - 值：solid 表示实线；dotted表示点状虚线；dashed表示虚线；double表示双线
- border简写属性，通过该属性可以同时设置边框所有的相关样式。例如：
border:10px red solid;

---
- 内边距（padding）(P47)
内容区和边框之间的距离是内边距，一共四个方向的内边距（padding-top、padding-bottom、padding-left、padding-right）
- - 内边距的设置会影响到盒子的大小，背景颜色会延伸到内边距上
- - padding内边距的简写属性，可以同时指定四个方向的内边距，规则和border-width一样 

一个盒子的可见框的大小，由内容区、内边距和边框共同决定，所以在计算盒子大小时，需要将这三个区域加到一起计算

---
- 外边距（margin）(P48)
外边距不会影响盒子可见框的大小，但是外边距会影响盒子的位置，一共四个方向的外边距（margin-top、margin-bottom、margin-left、margin-right）
- - 元素在页面中是按照自左向右顺序排列，所以默认设置左和上外边距则会移动元素，而设置下和右外边距会移动其他元素
- - margin也可以设置为负值，如果是负值则元素会向相反的方向移动
- - margin外边距的简写属性，可以同时指定四个方向的外边距，规则和border-width一样

## 13.盒子模型水平方向的布局（P49）
元素的水平方向的布局：元素在其父元素中水平方向的位置由以下几个属性共同决定：margin-left、border-left、padding-left、width、padding-right、border-right、margin-right

一个元素在其父元素中，水平布局必须满足以下等式：
子元素的margin-left+border-left+padding-left+width+padding-right+border-right+margin-right=其父元素内容区的宽度（必须满足）
- 上述等式必须满足，如果相加结果使等式不成立，则称为过度约束，则等式会自动调整
- 调整情况：如果这七个值中没有auto的情况，则浏览器会自动调整margin-right值以使等式满足
- 这七个值中有三个值可以设置为auto：width、margin-left、margin-right，如果某个值为auto，则会自动调整为auto的那个值以使得等式成立
- 设置元素在其父元素中水平居中
  width:500px; /* 必须指定宽度值 */
  margin: 0 auto;  

## 14.盒子模型垂直方向的布局（P50）
子元素是在父元素的内容区中排列的，如果子元素的大小超过了父元素，则子元素会从父元素中溢出，使用<b>overflow属性</b>来设置父元素如何处理溢出的子元素
overflow属性可选值：
+ visible,默认值，子元素会从父元素中溢出，在父元素外部的位置显示
+ hidden，溢出的内容将会被裁剪不会显示
+ scroll，生成两个滚动条，通过滚动条来查看完整内容
+ auto，根据需要生成滚动条

overflow-x，overflow-y分别处理水平方向和垂直方向子元素溢出问题，取值和overflow相同。

## 15.盒子模型外边距的折叠（P51）
外边距的重叠（折叠）：
- 相邻的垂直方向外边距会发生重叠现象
- 兄弟元素
- - 兄弟元素间的相邻垂直外边距会取两者之间的较大值（正值），特殊情况：如果相邻的外边距一正一负，则取两者的和；如果相邻的外边距都是负值，则取两者中绝对值较大的
- - 兄弟元素之间的外边距的重叠，对于开发是有利的，所以不需要进行处理

- 父子元素
- - 父子元素间的相邻外边距，子元素的会传递给父元素（上外边距）
- - 父子外边距折叠会影响到页面布局，必须进行处理

## 16.行内元素的盒模型（P52）
行内元素的盒模型：
- 行内元素不支持设置宽度和高度
- 行内元素可以设置padding，但是垂直方向padding不会影响页面的布局
- 行内元素可以设置border，垂直方向的border不会影响页面的布局
- 行内元素可以设置margin，垂直方向的margin不会影响布局

display属性，用来设置元素显示类型
可选值：
+ inline 将元素设置为行内元素
+ block 将元素设置为块元素
+ inline-block 将元素设置为行内块元素（行内块，既可以设置宽度和高度，又不会独占一行）
+ table 将元素设置为一个表格
+ none 元素不在页面显示
  

visibility 用来设置元素的显示状态
可选值：
+ <b>visible</b> 默认值，元素在页面中正常显示;
+ <b>hidden</b>元素在页面中隐藏，不显示，但是依然占据页面的位置

## 17.浏览器的默认样式
浏览器的默认样式：
+ 通常情况下，浏览器都会为元素设置一些默认样式，默认样式的存在会影响到页面的布局，通常情况下编写网页时必须要去除浏览器的默认样式（PC端的页面）
+ 引入css文件清除默认样式
+ + [CSS Tools: Reset CSS](https://meyerweb.com/eric/tools/css/reset/)
+ + [normalize.css](https://github.com/necolas/normalize.css/)

## 练习题（P54-P58）
[京东图片列表，京东左侧导航栏，网易新闻列表](https://github.com/qw-null/Web-HTML5-CSS3-/tree/master/P54%E5%B8%83%E5%B1%80%E7%BB%83%E4%B9%A0)

## 18.盒子的大小（P58）
默认情况下，盒子可见框的大小由内容区、内边距和边框共同决定。
box-sizing 用来设置盒子尺寸的计算方式（设置width和height的作用）
可选值：
+ content-box 默认值，宽度和高度用来设置内容区的大小
+ border-box 宽度和高度用来设置整个盒子可见框的大小（width和height指的是内容区、内边距和边框的总大小）

## 19.轮廓阴影和圆角（P59）
outline 用来设置元素的轮廓线，用法和border相同，【outline和border不同点是outline不会影响可见框的大小】

box-shadow用来设置元素的阴影效果，阴影不会影响页面布局
box-shadow:水平偏移量 &nbsp; 垂直偏移量&nbsp;模糊半径&nbsp; 颜色;
+ 第一个值：水平偏移量，设置阴影的水平位置，正值向右移动，负值向左移动
+ 第二个值：垂直偏移量，设置阴影的垂直位置，正值向下移动，负值向上移动
+ 第三个值：阴影的模糊半径
+ 第四个值：阴影颜色，一般使用rgba保证阴影的透明度

border-radius设置圆角，还可以单独设置border-top-left-radius、border-top-right-radius、border-bottom-left-radius、border-bottom-right-radius
border-radius可以分别指定四个角的圆角
+ 四个值：左上、右上、右下、左下
+ 三个值：左上、右上/左下、右下
+ 两个值：左上/右下、右上/左下

## 20.浮动（P60-P61）
通过浮动可以使一个元素向其父元素的左侧或者右侧移动，通过float属性来设置元素的浮动。
float可选值:
+ none 默认值，元素不浮动
+ left 元素向左浮动
+ right 元素向右浮动

元素设置浮动以后，会完全从文档流中脱离，不再占用文档流中的位置，因此元素下边的还在文档流中的元素会自动向上移动

浮动的特点：
+ 1.浮动元素会完全脱离文档流，不再占据文档流中的位置
+ 2.设置浮动以后元素会向父元素的左侧或右侧移动
+ 3.浮动元素默认不会从父元素中移出
+ 4.浮动元素向左或向右移动时，不会超过它前面的其他浮动元素
+ 5.如果浮动元素的上边是一个没有浮动的块元素，则浮动元素无法上移
+ 6.浮动元素不会超过它上面的浮动的兄弟元素，最多是和其兄弟元素一样高
+ 7.浮动元素不会盖住文字，文字会自动环绕在浮动元素的周围，因此可以利用浮动来实现文字环绕图片效果
+ 8.元素设置浮动以后，将会从文档流中脱离，从文档流中脱离后，元素的一些特点也会发生改变

简单总结：浮动目前来讲主要作用就是让页面中的元素可以水平排列，通过浮动可以制作一些水平方向的布局

<span style="font-size:20px;font-weight:700;">脱离文档流的特点：</span>
+ 块元素：
+ + 1.块元素不再独占页面一行
+ + 2.脱离文档流之后，默认宽度和高度都是被内容撑开
  
+ 行内元素：
+ + 行内元素脱离文档流之后会变成块元素，特点和块元素一样

脱离文档流之后，不需要再区分块元素和行内元素

## 浮动练习--导航条（P62）
[导航条](https://github.com/qw-null/Web-HTML5-CSS3-/tree/master/P62%E5%AF%BC%E8%88%AA%E6%9D%A1%E7%BB%83%E4%B9%A0)
效果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211026103344.png)

## 21.高度塌陷和BFC（P64）
<b>高度塌陷问题：</b>
在浮动布局中，父元素高度默认是被子元素撑开，当子元素浮动后，其会完全脱离文档流，子元素从文档流中脱离，将会无法撑起父元素的高度，导致父元素的高度丢失。
父元素高度丢失以后，其下的元素会自动上移，导致页面的布局混乱，所以高度塌陷是浮动布局中比较常见的一个问题，必须要进行处理。

<b>BFC（Block Formatting Context，块级格式化环境）</b>
BFC是一个CSS中的一个隐含属性，可以为一个元素开启BFC，开启BFC后，该元素会变成一个独立的布局区域

元素开启BFC后的特点：
+ 开启BFC的元素不会被浮动元素所覆盖
+ 开启BFC的元素子元素和父元素外边距不会重叠
+ 开启BFC的元素可以包含浮动的子元素

可以通过一些特殊的方式来开启元素的BFC：
+ 1.设置元素的浮动<b>【不推荐】</b>
+ 2.将元素设置为行内块元素（inline-block）<b>【不推荐】</b>
+ 3.将元素的overflow设置为一个非visible的值
+ + 常用方式：为元素设置overflow:hidden开启其BFC，以使其可以包含浮动元素，<b>overflow:hidden要赋值给不想要被影响的元素</b>

 推荐方式：将overflow设置为hidden是副作用最小的开启BFC的方式。

## 22.解决高度塌陷问题
#### 22.1 clear属性（P66）
例子引入：
```html
<div class="box1">1</div>
<div class="box3">3</div>
```
```css
.box1{
  width:200px;
  height:200px;
  background-color:#bfa;
  float:left;
}
.box3{
  width:200px;
  height:200px;
  background-color:orange;
}
```
效果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211028105107.png)
由于box1的浮动，导致box3位置上移，也就是box3受到了box1浮动的影响，位置发生了改变。
如果我们不希望某个元素因为其他元素浮动的影响而改变位置，可以通过clear属性来清除浮动元素对当前元素产生的影响。


clear属性
- 作用：清除浮动元素对当前元素所产生的影响
- 可选值：
- - left 清除左侧浮动元素对当前元素产生的影响
- - right 清除右侧浮动元素对当前元素的影响
- - both 清除两侧中最大影响的那侧（谁的高度大，就清除谁的）

原理：设置清除浮动以后，浏览器会自动为元素添加一个上边距，以使其位置不受其他元素影响

例子：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211028141359.png)

#### 22.2.使用after伪类解决高度塌陷问题（P67）
使用after伪类解决高度塌陷问题是解决高度塌陷的最终解决方案 
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211028142356.png)


#### 补充知识：解决外边距重叠问题（P68）
问题情景：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211028142844.png)
解决方案：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211028143400.png)


clearfix这个样式可以同时解决高度塌陷和外边距重叠的问题，当遇到这些问题时，可以直接使用clearfix进行解决
```css
.clearfix::before,
.clearfix::after{
  content:'';
  display:table;
  clear:both;
}
```
## 23.position定位
定位(position)，是一种更加高级的布局手段，通过定位可以将元素摆放在页面的任意位置，使用position属性来设置定位
可选值：
+ static 默认值，元素是静止的没有开启定位
+ relative 开启元素相对定位
+ absolute 开启元素绝对定位
+ fixed 开启元素的固定定位
+ sticky 开启元素的粘滞定位

#### 23.1 相对定位relative（P69）
position：relative;开启元素的相对定位。
特点：
+ 1.元素开启相对定位，如果不设置偏移量，元素不会发生任何的变化
+ + 偏移量（offset）：当元素开启了定位之后，可以通过偏移量来设置元素的位置。
+ + 包含的值：
+ + top———定位元素和定位位置上边的距离、bottom———定位元素和定位位置下边的距离【定位元素垂直方向的位置由top、bottom两个属性控制，通常情况下只会使用其一】
+ + left———定位元素和定位位置左边的距离、right———定位元素和定位位置右边的距离【定位元素水平方向的位置由left、right两个属性控制，通常情况下只会使用其一】
+ 2.相对定位是参照于元素在文档流中的位置进行定位
+ 3.相对定位会提升元素的层级
+ 4.相对定位不会使元素脱离文档流
+ 5.相对定位不会改变的的元素的性质，块元素还是块元素，行内元素还是行内元素

#### 23.2 绝对定位absolute（P70）
position：absolute;开启元素的绝对定位。
特点：
+ 1.元素开启绝对定位，如果不设置偏移量，<b><u>元素的位置</u></b>不会发生任何的变化
+ 2.开启绝对定位后，元素会从文档流中脱离
+ 3.绝对定位会改变元素的性质，行内变成块，块的高度被内容撑开
+ 4.绝对定位会使得元素提升一个层级
+ 5.绝对定位是相对于其包含块进行定位的
+ + 包含块（containing block）
+ + 正常情况下包含块就是离当前元素最近的<b>祖先<u>块元素</u></b>
+ + 绝对定位的包含块：包含块就是离它最近的开启了定位的祖先元素，如果所有祖先元素都没有开启定位，则根元素就是它的包含块
+ - html（根元素、初始包含快）

#### 23.3 固定定位fixed（P71）
position:fixed;开启元素的固定定位。固定定位也是一种绝对定位，因此固定定位的大部分特点和绝对定位一样，唯一不同的是固定定位永远参照于浏览器的视口进行定位。
固定定位的元素不会随网页的滚动条滚动。

#### 23.4 粘滞定位sticky（P72）
position:sticky;开启元素的粘滞定位。粘滞定位和相对定位的特点基本一致，不同的是粘滞定位可以在元素到达某个位置时将其固定
**兼容性很差,了解即可**

#### 补充知识：绝对定位（P73）
当开启绝对定位后：水平方向的布局等式就需要添加left和right两个值。 
left + margin-left + border-left + padding-left + width + padding-right + border-right + margin-right + right = 包含块内容区的宽度

开启绝对定位后，水平方向的布局等式就需要添加left和right两个值，此时的规则和之前一样只是多添加了两个值。
当发生过度约束，如果9个值中没有auto，则自动调整right的值以使等式成立。
可以设置为auto的值：margin、width、left、right

垂直方向布局的等式也必须要满足：top + margin-top/bottom + padding-top/bottom + border-top/bottom + height = 包含块的高度

##### 使得元素水平垂直居中：
```html
  <div class="box1">
    <div class="box2"></div>
  </div>
```
```css
  .box1 {
      position: relative;
      width: 400px;
      height: 400px;
      background-color: aqua;
    }

    .box2 {
      position: absolute;
      width: 100px;
      height: 100px;
      background-color: orange;
      margin: auto;
      left: 0;
      right: 0;
      top: 0;
      bottom: 0;
    }
```
效果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211101104011.png)
关键代码：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211101104122.png)

## 24.元素的层级（P74）
对于开启了定位的元素，可以通过z-index属性来指定元素的层级，z-index需要一个整数作为参数，值越大元素的层级越高，元素的层级越高越优先显示。
如果元素层级一样，则优先显示靠下的元素。
**祖先元素的层级再高也不会盖住后代元素**

## 京东轮播图布局练习
[京东轮播图布局](https://github.com/qw-null/Web-HTML5-CSS3-/tree/master/P75%E4%BA%AC%E4%B8%9C%E8%BD%AE%E6%92%AD%E5%9B%BE%E5%B8%83%E5%B1%80)

## 25.字体相关的样式（P76）
color：用来设置字体颜色
font-size:字体大小
+ 和font-size相关的单位：
+ em 相当于当前元素的一个font-size
+ rem相当于根元素的一个font-size

font-family:字体族（字体的格式），可选值：（字体分类）
+ serif 衬线字体
+ sans-serif 非衬线字体
+ monospave 等宽字体

指定字体类别，浏览器会自动使用该类别下的字体，font-family可以同时指定多个字体，多个字体间使用,隔开，字体生效时优先使用第一个，第一个无法使用则使用第二个，以此类推

@font-face可以将服务器中的字体直接提供给用户去使用。
存在问题：
+ 1.加载速度
+ 2.版权
+ 3.字体格式
  
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211101144230.png)
```css
@font-face{
  font-family: 'myfont'; /* 指定字体名字 */
  src: url(''); /* 服务器端中的地址 */
}

p{
  font-family: 'myfont';
}
```

#### 25.1 图标字体iconfont（P77-P78）
在网页中经常需要使用一些图标，可以通过图片来引入图标，但是图片大小本身比较大，并且非常不灵活。所以在使用图标时，我们还可以将图标直接设置为字体，然后通过font-face的形式来对字体进行引入，这样我们就可以通过使用字体的形式来使用图标。

font awesome使用步骤：
+ 1.[下载](https://fontawesome.com/)
+ 2.解压
+ 3.将css和webfonts移动到项目中
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211101150302.png)
+ 4.将all.css引入到网页中
```html
<link rel="stylesheet" href="(项目中的地址)**/css/all.css">
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211101150532.png)
+ 5.使用图标字体
+ + 直接通过类名来使用图标字体
```html
<head>
  <link href="/your-path-to-fontawesome/css/all.css" rel="stylesheet"> <!--load all styles -->
</head>
<body>
  <i class="fas fa-user"></i> <!-- uses solid style -->
  <i class="far fa-user"></i> <!-- uses regular style -->
  <i class="fal fa-user"></i> <!-- uses light style -->
  <!--brand icon-->
  <i class="fab fa-github-square"></i> <!-- uses brands style -->
</body>
```
通过伪元素来设置图标字体
+ 1.找到要设置图标的 元素通过before或after选中
+ 2.在content中设置字体编码
+ 3.设置字体的样式
```css
/* 在文件all.css中查找 */
/* fab */
font-family:'Font Awesome 5 Brands';

/* fas */
font-family:'Font Awesome 5 Free';
font-weight: 900; 
```

#### 25.2 Iconfont阿里图标库（P79）
使用：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211101155211.png)
使用到的下载文件：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211101155317.png)
通过伪类的方法使用：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211101155445.png)

#### 25.3 字体的简写属性（P81）
font可以设置字体相关的所有属性，语法：
```css
font: 字体大小/行高  字体族
行高可以省略不写，如果不写会使用默认值
```
font-weight 字重，字体加粗
可选值：
+ normal 默认值，不加粗
+ blod 加粗
+ 100-900 九个级别（没什么用）

font-style 字体风格
可选值：
+ normal 正常的
+ italic 斜体

## 26.行高（P80）
行高（line height）: 
+ 指的是文字占有的实际高度
+ 可以通过line-height来设置行高，可以直接指定一个大小（px,em）,也可以直接为行高设置一个整数【行高为整数的话，将会是字体指定的倍数】
+ 行高还经常用来设置文字的行间距【行间距 = 行高 - 字体大小】

字体框：就是字体存在的盒子，设置font-size实际上就是在设置字体框的高度

行高会在字体框的上下平均分配，可以将line-height和height的值设为一致，使得单行文字在一个元素中垂直居中。

## 27.文本的水平和垂直对齐（P82）
text-align 文本的水平对齐
可选值：
+ left 左对齐 
+ center 居中对齐 
+ right 右对齐 
+ justify 两端对齐

vertical-align 设置元素垂直对齐的方式
可选值：
+ baseline 默认值，基线对齐
+ top 顶部对齐
+ bottom 底部对齐
+ middle 居中对齐

补充知识点：
```html
<p>
  <img src="./img/1.jpg" alt="example">
</p>
<style>
  p {
    border:1px red solid;
  }
</style>
```
效果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211104093836.png)
解决方法：
```css
img{
  vertical-align:bottom;
  /* vertical-align:top;vertical-align:middle;也可以实现同样的效果 */
}
```
## 28.其他文本样式（P83）
text-decoration 设置文本修饰
可选值：
+ none 默认值，什么都没有
+ underline 下划线
+ line-through 删除线
+ overline 上划线

white-space 设置网页如何处理空白
可选值：
+ normal 默认值，正常
+ nowrap 不换行
+ pre 保留空白（源代码中文本格式不做处理，保留）

超出文本框的文字设置省略号
```html
<div class="box2">
  Lorem ipsum dolor sit amet consectetuir adipisicim
</div>
<style>
  .box2{
    width:200px;
    white-space:nowrap;
    overflow:hidden;
    text-overflow:ellipsis;
  }
</style>
```
效果：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211104100706.png)

## 练习题（P84-P86）
[京东顶部导航条](https://github.com/qw-null/Web-HTML5-CSS3-/tree/master/P84%E4%BA%AC%E4%B8%9C%E9%A1%B6%E9%83%A8%E5%AF%BC%E8%88%AA%E6%9D%A1)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211105154415.png)

## 29.背景（P88）
&hearts; background-color 设置背景颜色
&hearts; background-image 设置背景图片
+ 可以同时设置背景颜色和背景图片，这样背景颜色将会成为图片的背景色
+ 如果背景的图片小于元素，则背景图片会自动在元素中平铺，将元素铺满
+ 如果背景的图片大于元素，背景图片将无法完全显示，只显示元素大小
+ 如果背景的图片和元素一样大，则会直接正常显示

&hearts; background-repeat 用来设置背景的重复方式
可选值：
repeat 默认值，背景会沿着x轴 y轴双方向重复
repeat-x 沿着x轴方向重复
repeat-y 沿着y轴方向重复
no-repeat 背景图片不重复 

&hearts; background-position 用来设置背景图片的位置
设置方式：通过top,left,right,bottom,center几个表示方位的词来设置背景图片的位置
使用方位词时必须同时指定两个值，如果只写一个值，则第二个值默认是center
通过偏移量来指定背景图片的位置，顺序是水平偏移量+垂直偏移量
```css
background-position:top center;
background-position:-10px 10px;
```

&hearts; background-clip设置背景的范围，可选值：
border-box 默认值，背景会出现在边框的下边
padding-box 背景不会出现在边框，只出现在内容区和内边距
content-box 背景只会出现在内容区

&hearts;background-origin 背景图片的偏移量计算的原点 
可选值：
padding-box 默认值，background-position从内边距处开始计算
content-box 背景图片的偏移量从内容区处计算
border-box 背景图片的偏移量从边框处开始计算

&hearts;background-size 设置背景图片的大小
可选值：
+ 第一个值表示宽度，第二个值表示高度，如果只写一个值，则第二个值默认是auto【background:100% 100%;】
+ cover 图片的比例不变，将元素铺满
+ contain 图片比例不变，将图片在元素中完整显示

&hearts;background 背景相关的简写属性，所有背景相关的样式都可以通过该样式来设置，并且该样式没有顺序要求，也没有哪个属性必须写的
&star; 注意：baxkground-size 必须写在background-position的后边，并且使用/隔开；background-origin，background-clip两个样式，background-origin要在background-clip的前边


## 背景练习题（P92）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211110102651.png)
&spades;  存在问题：
图片属于网页中的外部资源，外部资源都需要浏览器单独发送请求加载，浏览器加载外部资源时是按需加载，用则加载，不用则不加载。
上边的练习，link会首先加载，而hover和active会在指定状态时才会加载

## 30.雪碧图（P92）
解决图片显示问题：
可以将多个小图片同意保存到一个大图片中，然后通过调整background-position来显示图片，这样图片会同时加载到网页中，就可以有效避免出现闪烁问题，这个技术在网页中应用十分广泛，被称为CSS-Sprite，这种图称为雪碧图。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211110113539.png)

&hearts; 雪碧图的使用步骤：
1. 先确定要使用的图标
2. 测量图标的大小
3. 根据测量的结果创建一个元素
4. 将雪碧图设置为元素的背景图片
5. 设置一个偏移量以显示正确图片

雪碧图的特点：
一次性将多个图片加载进页面，降低请求的次数，加快访问速度，提升用户体验

## 31.线性渐变（P93）
#### 31.1 linear-gradient()
通过渐变可以设置一些复杂的背景颜色，可以实现从一个颜色向其他颜色过渡的效果
！！渐变是图片，需要通过 <b>background-image</b> 来设置
线性渐变：颜色沿着一条直线发生变化 
+ linear-gradient(red,yellow) 红色在开头，黄色在结尾，中间是过渡区域
+ 线性渐变的开头，我们可以指定一个渐变的方向
+ linear-gradient(to top , red , yellow)
+ + to left / to right / to bottom / to top
+ linear-gradient(45deg , red , yellow)
+ + deg表示度数
+ + turn 表示圈

渐变可以同时指定多个颜色，多个颜色默认情况下平均分布，
也可以手动指定渐变的分布情况，例如：<b>linear-gradient(red 100px , yellow 50px)</b>

#### 31.2 repeating-linear-gradient()
repeating-linear-gradient() 可以平铺的线性渐变

background-image:repeating-linear-gradient(red 50px , yellow 100px)
效果：（元素高度200px）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211110142040.png)

#### 31.2 径向渐变（P94）
radial-gradient() 径向渐变(放射性的效果)
+ 默认情况下，径向渐变的形状是根据元素的形状来计算的
+ + 正方形 ---> 圆形
+ + 长方形 ---> 椭圆形
+ 也可以手动指定径向渐变的大小
+ + circle - 圆形 / ellipse - 椭圆形
+ 也可以指定渐变的位置

radial-gradient(大小 at 位置 , 颜色 位置 , 颜色 位置 , ……)
- 大小：
- - circle 圆形
- - ellipse 椭圆形
- - closest-side 近边
- - closest-corner 近角
- - farthest-side 远边
- - farthest-corner 远角
- 位置：top / right / left / center / bottom

```css
background-image: radial-gradient(circle , red , yellow)

background-image: radial-gradient(100px 50px at top left , red , yellow)  
//100px 50px指定大小；at top left指定位置

```

---
## 练习项目：小米商城
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211224104600.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211224104907.png)
[项目地址](https://github.com/qw-null/Web-HTML5-CSS3-/tree/master/P101%E5%B0%8F%E7%B1%B3%E5%95%86%E5%9F%8E-%E9%A1%B9%E7%9B%AE%E7%BB%83%E4%B9%A0)

---

## 32.动画效果
#### 32.1 过渡效果（P115）
过渡（transition）：
+ 通过过渡可以指定一个属性发生变化时的切换方式
+ 通过过渡可以创建一些非常好的效果，提升用户体验

+ transition-property
  指定要执行过渡的属性,多个属性之间使用,隔开；
  如果所有属性都需要过渡，则使用all关键字；
  大部分属性都支持过渡效果，注意过渡时必须是从一个有效值向另一个有效值进行过渡；

+ transition-duration
  指定过渡效果持续的时间
  时间单位：s和ms，1s = 1000ms

+ transition-timing-function
  过渡的时序函数
  可选值：
   ease 默认值，慢速开始，先加速，再减速
   linear 匀速运动
   ease-in 加速运动
   ease-out 减速运动
   ease-in-out 先加速，后减速
   cubic-bezier() 来指定时序函数 [cubic-bezier网址](https://cubic-bezier.com/)
   steps() 分步执行过渡效果，可以设置第二个数值【end-在时间结束时执行过渡（默认值）；start-在时间开始时执行过渡,示例：steps(2,end)】

+ transition-delay
  过渡效果的延迟，等待一段时间后再执行过渡
   
使用transition可以同时设置过渡相关的所有属性的，只有一个要求，如果要写延迟，则两个时间中第一个是持续时间，第二个是延迟时间
#### 32.2 过渡练习：米兔（P116）
动画：动画和过渡类似，都是可以实现一些动态的效果，不同的是过渡需要在某个属性发生变化时才会触发，动画可以自动触发动态效果

+ 设置动画效果，必须设置一个关键帧，关键帧设置了动画执行每一个步骤
```css
/* 关键帧 */
@keyframes test {
  /* from表示动画的开始位置,也可以使用0% */
  from{
    margin-left: 0;
  }

/* to表示动画的结束位置,也可以使用100% */
  to{
    margin-left: 700px;
  }
}
```

+ animation-name:指定当前元素生效的关键帧的名字
+ animation-duration:指定动画的执行时间
+ animation-delay:指定动画的延迟时间
+ animation-iteration-count:指定动画执行的次数
  可选值：数字（次数）、infinite 无限次
+ animation-direction:指定动画的运行方向
  可选值：
  normal - 默认值，从from向to运行
  reverse - 从to向from运行
  alternate - 从from到to，再从to到from，再从from到to …… 运行
  alternate-reverse - 从to到from，再从from到to，再从to到from…… 运行
+ animation-play-state:设置动画的执行状态
  可选值：
  running 默认值，动画执行
  paused 动画暂停
+ animation-fill-mode:设置动画的填充模式
  可选值：
  none 默认值，动画执行完毕元素回到原来位置
  forwards 动画执行完毕，元素会停止在动画结束位置
  backwards 动画延时等待时，元素就会处于开始位置
  both 结合了forwards和backwards

## 33.变形（P120-P121）
变形就是通过css来改变元素的形状或者位置，变形不会影响到页面的布局。
transform： 用来设置元素的变形效果
平移：
translateX() - 沿着X轴方向平移
translateY() - 沿着Y轴方向平移
translateZ() - 沿着Z轴方向平移
平移元素时，百分比是相对于自身计算的
#### ★元素居中显示★
```css
/* 1.已知元素大小情况： */
.box {
  width: 100px;
  height: 100px;
  background-color: red;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}

/* 2.不知元素大小情况 */
.box {
  background-color: red;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
```
[实现水平垂直居中[不知自身宽高情况]](https://qw-null.github.io/2021/09/13/%E5%AE%9E%E7%8E%B0%E6%B0%B4%E5%B9%B3%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD/)

变形不会影响到页面的布局,不会导致元素脱离文档流，因此可以用于实现选中元素的浮动效果。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211224172635.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211224172708.png)

translateZ():Z轴平移，调整元素在z轴的位置，正常情况就是调整元素和人眼之间的距离，距离越大，元素离人越近
z轴平移属于立体效果（近大远小），默认情况下网页是不支持透视，如果需要看见效果，必须要设置网页的视距
```css
html{
  /* 设置当前网页的视距为800px,人眼距离网页的距离 */
  perspective: 800px;
}

.box1:hover {
  /* 此时translateZ效果才会生效 */
  transform: translateZ(800px);
}
```

## 34.旋转（P122）
通过旋转可以使元素沿着x、y或z旋转指定的角度
rotateX()
rotateY()
rotateZ()
```css
.box{
  transform: rotateX(45deg);
  /* 45deg->45° ；1turn->旋转一圈 */
}
```
backface-visibility：是否显示元素的背面
可选值：visible - 默认值，可以看到背面；hidden - 无法看到背面

[旋转练习 - 钟表](https://github.com/qw-null/Web-HTML5-CSS3-/tree/master/P123%E9%92%9F%E8%A1%A8)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211225172211.png)

[旋转练习 - 立方体](https://github.com/qw-null/Web-HTML5-CSS3-/tree/master/P124%E5%A4%8D%E4%BB%87%E8%80%85%E8%81%94%E7%9B%9F)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211225171609.png)

## 35.缩放（P125）
对元素进行缩放,当数值>1表示对元素进行放大，当数值<1表示对元素进行缩小。
scaleX() - 对元素的水平方向进行放大或者缩小
scaleY() - 对元素的垂直方向进行放大或者缩小
scale() - 对元素进行放大或缩小 

transform-origin 设置元素变形的远点
可选值： 
center - 默认值，元素中间
0，0 - 元素最左上角

## 36.less (P126)
less是一门css的预处理语言，通过less可以编写更少的代码实现更强大的样式
在less中添加了许多新特性，像对变量的支持、对mixin的支持……，less的语法大体上和css语法一致，但是less中增添了许多对css的拓展，所以浏览器无法直接执行less代码，要执行就必须将less转换为css

```less
1.注释
// less中的单行注释，注释中的内容不会被解析到css中
/*
  css中的注释，内容会被解析到css文件中
*/

2.变量
变量中可以存储一个任意值，并且在需要时可任意的修改变量的值
变量的语法： @变量名
使用变量时，如果是直接使用则以 @变量名 的形式使用即可
作为类名，或者一部分值使用时必须以 @{变量名} 的形式使用
@length:100px
@c:box3;
.@{c}{
  width: @length;
  background-image: url("@{c}/1.jpg")
}

变量重名时，会优先使用比较近的变量
可在声明变量前就使用变量
---
& 表示外层的父元素

.entend() 对当前选择器扩展指定选择器的样式（选择器分组）
示例：.p2.extend(.p1){
        color:red;
      }

mixin 混合
.p3{
  // 直接对指定的样式进行引用，这里就相当于将p1的样式在这里进行了复制
  .p1()
}

使用类选择器时可以在选择器后边添加一个括号，这时实际上就创建了一个mixins
.p4(){
  width: 100px;
  height: 100px;
}

混合函数 在混合函数中可以直接设置变量,使用时按照顺序传递参数
.test(@w){
  width: @w;
  height: 200px;
  border-color: red;
}
div{
  .test(100px);
}

在less中所有数值都可以直接进行运算
width: 100px + 200px;

import用于将其他less文件引入当前less文件中
@import "color.less"
```
★less中的除法计算要放在括号中，不然会失效
```less
box{
  width: (100 / 4 rem);
}
```

## 37.弹性盒（P131）
flex(弹性盒、伸缩盒)：是css中的又一种布局手段，它主要用来代替浮动来完成页面。
flex可以使元素具有弹性，让元素可以跟随页面得到大小的改变而改变

- 弹性容器
  要使用弹性盒，必须先将一个元素设置为弹性容器，通过display来设置弹性容器
  ```css
  display:flex; //设置为块级弹性容器
  display:inline-flex; //设置为行内的弹性容器
  ```
- 弹性元素
  弹性容器的子元素是弹元素（弹性项），一个元素可以同时是弹性容器和弹性元素


主轴：弹性元素的排列方向称为主轴
侧轴：与主轴垂直方向的称为侧轴


<i style="color:green;font-size:18px;">☻</i> 弹性容器的样式：

1. flex-direction 指定容器中弹性元素的排列方式（主轴方向）
   可选值：
    row 默认值，弹性元素在容器中水平排列（自左向右）
    row-reverse 弹性元素在容器中反向水平排列（自右向左）
    column 弹性元素纵向排列（自上向下）
    column-reverse 弹性元素反向纵向排列（自下向上）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220117114131.png)

2. flex-grow 指定弹性元素的伸展系数
   当父元素有多余空间时，子元素如何伸展

3. flex-shrink 指定弹性元素的收缩系数
   当父元素中的空间不足以容纳所有子元素时，如何对子元素进行收缩

4. flex-wrap 设置弹性元素是否在弹性容器中自动换行
   可选值：
   nowrap 默认值，元素不会自动换行
   wrap 元素沿着辅轴方向自动换行
   wrap-reverse 元素沿着辅轴反方向换行

★flex-flow 是 flex-wrap和flex-direction的简写属性,例如：flex-flow: row wrap;

5. justify-content 如何分配主轴上的空白空间（主轴上的元素如何排列）
   可选值：
   flex-start 元素沿着主轴的起边排列
   flex-end 元素沿着主轴的终边排列
   center 元素居中排列
   space-around 空白分布到元素的两个
   space-between 空白均匀分布到元素间
   space-evenly 空白分布到元素的单侧
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220117135730.png)

6. align-items 定义flex子项在flex容器的当前行的侧轴（纵轴）方向上的对齐方式（元素间关系）
   可选值：
   stretch 默认值，将元素的长度设置为相同的值
   flex-start 元素不会拉伸，沿着辅轴起边对齐
   flex-end 元素不会拉伸，沿着辅轴终边对齐
   center 居中对齐
   baseline 基线对齐
  ** align-self 用来覆盖当前弹性元素上的align-items
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220117135907.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220117140115.png)


<i style="color:green;font-size:18px;">☻</i> 弹性元素的样式：

1. 弹性元素的缩减系数：
   缩减系数的计算方式比较复杂，缩减多少是根据缩减系数和元素大小来计算（flex-grow和flex-shrink）。
2. 元素基础长度
   flex-basis 指定的是元素在主轴上的基础长度。如果主轴是横向的，则该值指定的就是元素的宽度，如果主轴是纵向的，则该值指定的就是元素的高度。
   默认值是auto，表示参考元素自身的高度或宽度。如果传递了一个具体的数值，则以该值为准。

★flex可以设置弹性元素所有的三个样式。
格式：flex: 增长 缩减 基础；（例如：flex: 1 1 auto;）

3. order 决定弹性元素的排列顺序（依据order指定的数值）。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220117140405.png)

## 38.像素（P136 - P138）
像素：屏幕是由一个一个发光的小点构成，这一个个的小点就是像素。前端开发中像素要分为两种情况讨论：css像素 和 物理像素。
- 物理像素：上述所说的小点就是物理像素。
- css像素：编写网页时所用的像素是css像素。浏览器在显示网页时，需要将css像素转换为物理像素，然后再呈现。

一个css像素最终由几个物理像素显示，由浏览器决定。默认情况下，在PC端 一个css像素 = 一个物理像素

视口（viewport）:屏幕中用来显示网页的区域。
可以通过查看视口的大小来观察css像素和物理像素的比值

在不同的屏幕，单位像素的大小是不同的，像素越小屏幕会越清晰

可以通过meta标签来设置视口大小
```html
<!-- 设置视口大小 device-width表示设备的宽度（完美视口） -->
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<!-- 将网页的视口设置为完美视口
结论：书写移动端的页面，就把上边这段代码加上 -->
```

完美视口：
每一款移动设备设计时，都会有一个最佳的像素比，一般只需要将像素比设置为该值即可得到一个最佳效果。将像素比设置为最佳像素比的视口大小成为完美视口。

vw表示的是视口的宽度（viewport width）
100vw = 一个视口的宽度
1vw = 1%视口宽度

☻ 网页中字体大小最小是12px，不能设置一个比12px还小的字体，如果设置一个小于12px的字体，则字体自动设置为12px。

## 39.媒体查询（P143）
响应式布局：网页可以根据不同设备或窗口大小呈现出不同的效果，使用响应式布局可以使一个网页适用于所有设备
响应式布局的关键是媒体查询。
媒体查询：通过媒体查询可以为不同的设备或设备不同的状态来分别设置样式。
语法：@media 查询规则{}

媒体类型：
all - 所有设备
print - 打印设备
screen - 带屏幕的设备
speech - 屏幕阅读器
可以使用逗号连接多个媒体类型，这样它们之间就是一个或的关系；
可以在媒体类型前添加一个only，表示只有。only的使用主要是为了兼容一些老版本的浏览器。

媒体特性：
width - 视口的宽度
height - 视口的高度
min-width - 视口的最小宽度（视口大于指定宽度时生效）
max-width - 视口的最大宽度（视口小于指定宽度时生效）

样式切换的分界点，我们称其为断点，也就是网页的样式在这个点会发生变化。 

小于768px - 超小屏幕 [max-width = 768px]
大于768px - 小屏幕 [min-width = 768px]
大于992px - 中型屏幕 [min-width = 992px]
大于1200px - 大屏幕 [min-width = 1200px]

响应式设计原则：
  1.移动端优先
  2.渐进增强（由移动端逐渐向网页过渡）

