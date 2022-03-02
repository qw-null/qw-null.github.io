---
title: 字节青训营-前端与HTML
date: 2022-01-16 11:09:31
categories:
- 字节青训营
tags: 
- HTML
---
## 0.什么是前端？
使用web技术栈，解决多端【PC/移动浏览器、客户端、小程序、VR/AR等】图形用户界面下人机交互问题
前端技术栈：html(内容) + css(样式) + js(行为)

## 1.HTML
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116115240.png)

浏览器会将html文件解析成一棵DOM树
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116120500.png)

### 1.1 HTML 语法
标签属性不区分大小写（推荐：小写）、属性值使用双引号包裹、空标签可以不闭合、某些属性值可以省略（readonly、required）
+ 空标签
  没有闭合标签的标签被称作为空标签。
  常见的空标签：
```html
<br /> 换行标签，通常用于文本格式换行
<input />  用于为基于Web的表单创建交互式控件，以便接受来自用户的数据。
<img /> 代表文档中的一个图像。
<isindex /> 使浏览器显示一个对话框，提示用户输入单行文本。
<area /> 在图片上定义一个热点区域
<base /> 指定用于一个文档中包含的所有相对URL的基本URL。
<basefont /> 用来设置文档的默认字体大小。（目前已废弃 ）
<bgsound /> IE浏览器中设置网页背景音乐的元素。
<col /> 定义表格中的列，并用于定义所有公共单元格上的公共语义。它通常位于`<colgroup>`元素内。
<embed /> 用于表示一个外部应用或交互式内容的集合点，换句话说，就是一个插件。 
<frame /> ，它定义了一个特定区域，另一个 HTML 文档可以在里面展示。(已废弃)
<keygen />  为了方便生成密钥材料和提交作为 [HTML form]的一部分的公钥.这种机制被用于设计基于 Web 的证书管理系统。(已废弃)
<link /> 指定了外部资源与当前文档的关系. 这个元素的使用方法包括为导航定义关系框架.这个元素经常用来链接css文件。
<meta /> 元素表示那些不能由其它HTML元相关元素 (<base>, <link>, <script>, <style> 或 <title>) 之一表示的任何元数据信息.
<nextid />  是一个过时的 HTML 元素, 它使下一个 web 设计工具能够为其定位点生成自动名称标签。它是由该 web 编辑工具自动生成的, 不需要手动调整或输入。这个元素的区别是成为第一个元素, 成为一个 "丢失的标签" 被淘汰的官方公共 DTD 的 HTML 版本。
<param />  定义了 <object>的参数
<plaintext /> 将起始标签后面的任何东西渲染为纯文本，不会解释为 HTML。它没有闭合标签，因为任何后面的东西都会看做纯文本。(已废弃)
<spacer /> 它可以向页面插入间隔。它由 Netscape 设计，用于实现单像素布局图像的相同效果，Web 设计师用它来向页面添加空白，而不需要实际使用图片。（已废弃）
<wbr /> 一个文本中的位置，其中浏览器可以选择来换行，虽然它的换行规则可能不会在这里换行。
```


### 1.2 HTML 标签
1. h1~h6 标题标签
2. 列表
   ```html
   // 1.有序列表
   <ol>
     <li>Aa</li>
     <li>Bb</li>
   </ol>

   // 2.无序列表
   <ul>
     <li>Cc</li>
     <li>Dd</li>
   </ul>

   // 3.自定义列表
   <dl>
     <dt>学校：</dt> <!-- 代表title -->
     <dd>UCAS</dd> <!-- title -> value -->
     <dt>校区</dt>
     <dd>雁栖湖校区</dd>
     <dd>玉泉路校区</dd>
     <dd>...</dd>
   </dl>
   ```
   ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116123525.png)
3. 链接
   ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116132541.png)
4. 视频、音频
   ```html
   <audio></audio>
   <video></video>
   ```
5. 输入
   ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116124307.png)
   ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116124609.png)
   选择框：type="checkbox"
   ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116124631.png)
  <hr/>

   ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116124721.png)
   单选框：type="radio" 、通过name属性的值控制单选
  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116124835.png)
  <hr>

  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116125023.png)
  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116125040.png)
  <hr>

  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116125151.png)
  通过list设置为用户的提示信息，datalist标签指定快捷输入数据
6. 引用
   + blockquote：
   \<blockquote> 标签定义摘自另一个源的块引用。
    浏览器通常会对 \<blockquote> 元素进行缩进。
    属性：cite →指定引用的来源
  + cite:
     \<cite> 标签通常表示它所包含的文本对某个参考文献的引用，比如书籍或者杂志的标题。按照惯例，引用的文本将以斜体显示。
  + q：
    \<q>标签用于定义短引文（行内引文）。浏览器将自动为短引文添加引号。

  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116130237.png)
  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116130434.png)

7. 代码标签
   \<code>\</code>
   引用多行代码时，使用\<pre>包裹\<code>
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116130638.png)

8. 表示强调
   \<strong>\<strong> → 加粗
   \<em>\<em>

9. 页面内容划分
    ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116131124.png)

### 1.3 HTML 语义化
根据内容的结构化（内容语义化），选择合适的标签（代码语义化）便于开发者阅读和写出更优雅的代码，同时让浏览器的爬虫和机器能很好地解析。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220116131858.png)

Q：为什么要语义化？
+ 为了在没有CSS的情况下，页面也能呈现出很好地内容结构、代码结构：<span style="background:yellow;"><b>为了裸奔时好看</b></span>；
+ <span style="background:yellow;"><b>用户体验</b></span>：例如title、alt用于解释名词或解释图片信息、label标签的活用；
+ <span style="background:yellow;"><b>有利于SEO</b></span>：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重；
+ 方便其他设备<span style="background:yellow;"><b>解析</b></span>（如屏幕阅读器、盲人阅读器、移动设备）以意义的方式来渲染网页；
+ <span style="background:yellow;"><b>便于团队开发和维护</b></span>，语义化更具可读性，是下一步吧网页的重要动向，遵循W3C标准的团队都遵循这个标准，可以减少差异化。