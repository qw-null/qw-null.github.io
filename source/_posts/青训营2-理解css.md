---
title: 字节青训营-理解css
date: 2022-01-16 15:52:19
categories:
- 字节青训营
tags: 
- CSS
---
## CSS是什么？

css(cascading style sheets):用来定义页面元素的样式。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b0323042c94648ebb71b785f0b3a70d1~tplv-k3u1fbpfcp-zoom-1.image)

## 页面使用css

```html
// 1. 外链形式
<link rel="stylesheet" href="/assets/style.css">

// 2.嵌入形式
<style>
  div{
    margin: 0 auto;
    color: red;
  }
</style>

// 3.内联形式
<span style="color:red;">一级标题</span>
```

## css如何工作？

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7a53824239cb443b8f6ca103b4b99c32~tplv-k3u1fbpfcp-zoom-1.image)

## 选择器Selector

找出页面中元素的位置，以便给其设置样式

选择页面元素的方式：

  + 标签名、类名、id
  + 属性
  + DOM树中的位置

选择器种类：通配选择器、标签选择器、id选择器、类选择器、属性选择器、伪类选择器

```css
// 属性选择器
[disabled] {
  background: blue;
  color: yellow;
}
```

## 选择器的组合使用


| 名称       | 语法  | 说明                              |        示例         |
| :--------- | :---: | :-------------------------------- | :-----------------: |
| 直接组合   |  AB   | 满足A的同时满足B                  |  ```input:focus```  |
| 后代组合   |  A B  | 选中B，如果他是A的子孙            |    ```nav a ```     |
| 亲子组合   | A > B | 选中B，如果他是A的子元素          | ```selection > p``` |
| 兄弟选择器 | A ~ B | 选中B，如果他在A后且和A同级       |    ```h2 ~ p```     |
| 相邻选择器 | A + B | 选中B，如果他<b>紧跟</b>在A的后面 |    ```h2 + p```     |

## 颜色

RGB

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9a228ad26abb4bcaa8d5ea27ba31dc32~tplv-k3u1fbpfcp-zoom-1.image)

HSL

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/392aec0267444896883b6e5f64ef8208~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d4c43482dfad478cac01c596feb73575~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cd845c3faa8a4c748df64b56e37ec421~tplv-k3u1fbpfcp-zoom-1.image)

## 字体

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
  
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ac410a72e0c642e39256fc7f5f534699~tplv-k3u1fbpfcp-zoom-1.image)

```css
@font-face{
  font-family: 'myfont'; /* 指定字体名字 */
  src: url(''); /* 服务器端中的地址 */
}

p{
  font-family: 'myfont';
}
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

```css
  font: style weight size/height family;
```
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/368829e3fe43434a99e9a1ddce0a06ea~tplv-k3u1fbpfcp-zoom-1.image)

text-aligh 设置文字对齐样式
可选值：
+ left 左对齐 
+ center 居中对齐 
+ right 右对齐 
+ justify 两端对齐（对最后一行的文字不生效）

letter-spacing:调整文字字母之间的间距
word-spacing:调整单词之间的距离
text-index:设置文字缩进

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
+ pre-wrap 一行内容显示不全会换行
+ pre-line 会保留换行

## 调试CSS
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/15843ca7bdfa44be9daca4ffc9257cb0~tplv-k3u1fbpfcp-zoom-1.image)

## 继承

某些属性会自动继承其父元素的计算值，除非显式指定一个值。
一般与文字相关的样式都会被继承，与宽度、高度、盒模型相关的不会被继承。

```css
// 显示继承
*{
  box-sizing: inherit;
}
html{
  box-sizing: border-box;
}
.some-widget{
  box-sizing: content-box;
}
```

## 初始值

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a93cd90dc08e431abc172a2ca646f94d~tplv-k3u1fbpfcp-zoom-1.image)

## CSS求值过程
如何从一个DOM节点生成一棵渲染树或渲染节点

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/09d977f8411a4823bc1bf64381fa3542~tplv-k3u1fbpfcp-zoom-1.image)

+ 第一步：筛选出节点对应的样式（样式个数：0~多个）
+ 第二步：筛选出样式中优先级最高的样式
+ 第三步：查看样式是否为空，若是空的，则依据继承或者初始值
+ 第四步：将一些相对值转化为绝对值（rem → px、相对路径 →  绝对路径等），这些绝对值是在浏览器未进行实际布局的情况下得到的具体值，其中会包含百分数。
+ 第五步：将计算值进一步计算，即将百分比转化为具体数值等
+ 第六步：上一步的计算值中可能会出现100.2px、超出min-width或max-width的情况，需要进一步将其转化为整数值

## 布局

布局相关技术：常规流、浮动、绝对定位

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e93f05f2ccaf46beb0939b197cfdfb2b~tplv-k3u1fbpfcp-zoom-1.image)

#### width和height

+ 指定content box 的宽度和高度；
+ 取值为长度、百分比、auto【百分比时，只有元素的容器指定width和height的具体值时才会生效】
  
#### padding

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/441bde485d5d4fa38e2f48007a75702c~tplv-k3u1fbpfcp-zoom-1.image)

设置数值为百分数时，是相对于元素容器宽度

#### border

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2a312738c01b4bc5b10a4123a1a94bbd~tplv-k3u1fbpfcp-zoom-1.image)

使用border属性生成三角形 → [详情](https://qw-null.github.io/2021/09/22/CSS%E5%A5%87%E6%B7%AB%E6%8A%80%E5%B7%A7/#2-%E7%BB%98%E5%88%B6%E4%B8%89%E8%A7%92%E5%BD%A2)
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

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ca9e85b6ce224e0abab99b0b32343f34~tplv-k3u1fbpfcp-zoom-1.image)

#### margin

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5950db38bc4b4843a2c889591fb82bb8~tplv-k3u1fbpfcp-zoom-1.image)

<b>margin collapse问题（边距折叠）</b>
仅仅发生在垂直方向上，水平方向上不会存在这个问题

1. 兄弟元素：兄弟元素间的相邻垂直外边距会取两者之间的较大值（正值），特殊情况：如果相邻的外边距一正一负，则取两者的和；如果相邻的外边距都是负值，则取两者中绝对值较大的
2. 父子元素：父子元素间的相邻外边距，子元素的会传递给父元素（上外边距）

#### box-sizing
默认情况下，盒子可见框的大小由内容区、内边距和边框共同决定。
box-sizing 用来设置盒子尺寸的计算方式（设置width和height的作用）
可选值：
+ content-box 默认值，宽度和高度用来设置内容区的大小
+ border-box 宽度和高度用来设置整个盒子可见框的大小（width和height指的是内容区、内边距和边框的总大小）

#### overflow
overflow属性用来设置父元素如何处理溢出的子元素
可选值：
+ visible，默认值，子元素会从父元素中溢出，在父元素外部的位置显示
+ hidden，溢出的内容将会被裁剪不会显示
+ scroll，生成两个滚动条，通过滚动条来查看完整内容
+ auto，根据需要生成滚动条

#### 块级 VS 行级

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5f73e0eda7fd48078e965f79efc442e6~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4b604b67902f46b6b0381e98a4aa3b1c~tplv-k3u1fbpfcp-zoom-1.image)

#### display属性
可选值：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0db732fa41da45fc9f92e79c2739ec85~tplv-k3u1fbpfcp-zoom-1.image)

#### 常规流 Normal Flow (文档流)
+ 根元素、浮动和绝对定位的元素会脱离常规流
+ 其他元素都在常规流之中
+ 常规流中的盒子，在某种排版上下文中参与布局，可通过display属性设置

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8262764db73b437a8090b9a0a1b133ae~tplv-k3u1fbpfcp-zoom-1.image)

##### 行级排版上下文

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6b0f2ac6b711436e9c299a24ee0a034e~tplv-k3u1fbpfcp-zoom-1.image)

##### 块级排版上下文

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/787387e2b30f4a279f0eb9cb47060e19~tplv-k3u1fbpfcp-zoom-1.image)

##### Flex Box

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c4e8bf862b9b47c98a290965e55685f4~tplv-k3u1fbpfcp-zoom-1.image)

<hr/>
flex-direction 指定容器中弹性元素的排列方式（主轴方向）

   可选值：
   + row 默认值，弹性元素在容器中水平排列（自左向右）
   + row-reverse 弹性元素在容器中反向水平排列（自右向左）
   + column 弹性元素纵向排列（自上向下）
   + column-reverse 弹性元素反向纵向排列（自下向上）

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ffb6b591c2b64756af3876ad527a1a66~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/de37ef63f41746a38c172e0b6dae4583~tplv-k3u1fbpfcp-zoom-1.image)

<hr/>
justify-content 如何分配主轴上的空白空间（主轴上的元素如何排列）

   可选值：
   + flex-start 元素沿着主轴的起边排列
   + flex-end 元素沿着主轴的终边排列
   + center 元素居中排列
   + space-around 空白分布到元素的两个
   + space-between 空白均匀分布到元素间
   + space-evenly 空白分布到元素的单侧

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/01cb8f4b015040a58c05231eee8c38e4~tplv-k3u1fbpfcp-zoom-1.image)

<hr/>
align-items 定义flex子项在flex容器的当前行的侧轴（纵轴）方向上的对齐方式（元素间关系）

   可选值：
   
   + stretch 默认值，将元素的长度设置为相同的值
   + flex-start 元素不会拉伸，沿着辅轴起边对齐
   + flex-end 元素不会拉伸，沿着辅轴终边对齐
   + center 居中对齐
   + baseline 基线对齐
  ** align-self 用来覆盖当前弹性元素上的align-items

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5c9fe55c05bc4e3ab2bb914449a2359a~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a3d3694162d942ada4dcceb0ac00ef7a~tplv-k3u1fbpfcp-zoom-1.image)
<hr/>
Flexibility

+ 可以设置子项的弹性：当容器有剩余空间时，会伸展；容器空间不够时，会收缩。
+ flex-grow 有剩余空间时的伸展能力
+ flex-shrink 容器空间不足时收缩的能力
+ flex-basis 没有伸展或收缩时的基础长度

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b61f6dfc2e0b473a95dc952d36b19507~tplv-k3u1fbpfcp-zoom-1.image)

##### Grid 布局

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0af06cb54fc043c28538c5f755b612dc~tplv-k3u1fbpfcp-zoom-1.image)

Flex仅可以设置一个方向的布局，Grid是一个二维布局方式

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a28a40f5fdc04a95aac76a533beca73b~tplv-k3u1fbpfcp-zoom-1.image)

<hr/>
划分网格：

```css
grid-template-columns: 100px 100px 200px;
grid-template-rows: 100px 100px;
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/397276cb11a245fa8eeb1306659b1965~tplv-k3u1fbpfcp-zoom-1.image)

```css
grid-template-columns: 30% 30% auto;
grid-template-rows: 100px auto;
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fed64fe6060240128f8fefa4c792a760~tplv-k3u1fbpfcp-zoom-1.image)

```css
grid-template-columns: 100px 1fr 1fr;
// fr表示一份
// 含义：除去第一部分100px外，剩余的第二部分、第三部分各占剩余部分的一份。
grid-template-rows: 100px 1fr;
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/56556798271c413885435dfc7fc71057~tplv-k3u1fbpfcp-zoom-1.image)
<hr/>
grid line网格线：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/080fb704459b472699e90be75af5e271~tplv-k3u1fbpfcp-zoom-1.image)

grid area 网格区域：
网格区域使用网格线表示

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dd7708ed23f24aa39f1afcc2f5fdfd77~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fae3b461f78c4000b6d2076ecb9a3809~tplv-k3u1fbpfcp-zoom-1.image)

#### 浮动 Float

通过浮动可以使一个元素向其父元素的左侧或者右侧移动，通过float属性来设置元素的浮动。
float可选值:
+ none 默认值，元素不浮动
+ left 元素向左浮动
+ right 元素向右浮动

#### 绝对定位

定位(position)，是一种更加高级的布局手段，通过定位可以将元素摆放在页面的任意位置，使用position属性来设置定位
可选值：
+ static 默认值，元素是静止的没有开启定位
+ relative 开启元素相对定位
+ absolute 开启元素绝对定位
+ fixed 开启元素的固定定位
+ sticky 开启元素的粘滞定位

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5784d5476cf4490180f20e81e285f002~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/949b3423a1084ffe9fecd71f8a8d2126~tplv-k3u1fbpfcp-zoom-1.image)
