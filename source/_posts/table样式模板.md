---
title: <table>样式模板
date: 2021-06-06 20:09:38
categories:
- CSS样式
tags:
- HTML
- CSS
---

你必须非常努力 ，才能显得毫不费劲

### 一、样式 1
<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210606203427.png" style="zoom:175%;" />
##### 1.1源代码：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>table标签样式</title>
</head>

<body>
  <div class="title">
    <h3>Table标签样式</h3>
  </div>
  <table class="table">
    <thead>
      <tr>
        <th>编号</th>
        <th>名称</th>
        <th>信息</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>1</td>
        <td>阿里</td>
        <td>
          <span>阿里巴巴一般指阿里巴巴集团。阿里巴巴集团控股有限公司（简称：阿里巴巴集团）
            是以曾担任英语教师的马云为首的18人于1999年在浙江省杭州市创立的公司。</span>
        </td>
      </tr>
      <tr>
        <td>2</td>
        <td>腾讯</td>
        <td>
          <span>腾讯于1998年11月成立,是一家以互联网为基础的平台公司,
            通过技术丰富互联网用户的生活,助力企业数字化升级。我们的使命是“用户为本 科技向善”。</span>
        </td>
      </tr>
      <tr>
        <td>3</td>
        <td>百度</td>
        <td>
          <span>百度是一家持续创新的,以“用科技让复杂世界更简单”为使命的高科技公司。</span>
        </td>
      </tr>
      <tr>
        <td>4</td>
        <td>字节跳动</td>
        <td>
          <span>字节跳动一般指北京字节跳动科技有限公司。北京字节跳动科技有限公司，
            成立于2012年3月，是最早将人工智能应用于移动互联网场景的科技企业之一，
            是中国北京的一家信息科技公司，地址位于北京市海淀区知春路甲48号。
            公司以建设“全球创作与交流平台”为愿景</span>
        </td>
      </tr>
    </tbody>
  </table>

</body>
<style>
  .title {
    /* div居中显示 */
    margin: 0 auto;
    text-align: center;
  }

  .table {
    border-collapse: collapse;
    margin: 0 auto;
    /*对DIV设置margin:0 auto样式，
    是为了让DIV在浏览器中水平居中。布局居中、水平居中，
    均加入margin:0 auto即可。*/
    text-align: center;
  }

  .table td,
  .table th {
    border: 1px solid #cad9ea;
    color: #666;
    height: 80px;
    width: 350px;
  }

  table thead th {
    background-color: #CCE8EB;
    width: 100px;
  }

  .table tr:nth-child(odd) {
    /* 奇数行 */
    background: #fff;
  }

  .table tr:nth-child(even) {
    /* 偶数行 */
    background: #F5FAFA;
  }

  .table td span {
    font-size: 10px;
  }
</style>

</html>
```



##### 1.2知识点：
（1）CSS3 :nth-child() 选择器
:nth-child(n) 选择器匹配属于其父元素的第 N 个子元素，不论元素的类型。n 可以是数字、关键词或公式。

>用法 1：
>![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210606205553.png)
```html
<!DOCTYPE html>
<html>
<head>
<style> 
p:nth-child(1)
{
background:#ccddff;
}
</style>
</head>
<body>
    
<p>第一个段落。</p>
<p>第二个段落。</p>
<p>第三个段落。</p>
<p>第四个段落。</p>

<p><b>注释：</b>Internet Explorer 不支持 :nth-child() 选择器。</p>

</body>
</html>

```
> --存在的问题--
> ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210606210022.png)
```html
<!DOCTYPE html>
<html>
<head>
<style> 
p:nth-child(1)/*没有起作用，因为页面的第一个子元素是<h3>*/
{
background:green;
}
p:nth-child(2)
{
background:red;
}
</style>
</head>
<body>

<h3>:nth-child()演示</h3>
<p>第一个段落。</p>
<p>第二个段落。</p>
<p>第三个段落。</p>
<p>第四个段落。</p>

<p><b>注释：</b>Internet Explorer 不支持 :nth-child() 选择器。</p>

</body>
</html>
```
> 用法 2：
> Odd 和 even 是可用于匹配下标是奇数或偶数的子元素的关键词（第一个子元素的下标是 1）。
>
> ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210606210628.png)

```html
<!DOCTYPE html>
<html>
<head>
<style> 
p:nth-child(odd)
{
background: green;
}
p:nth-child(even)
{
background: yellow;
}
</style>
</head>
<body>

<h1>这是标题</h1>
<p>第一个段落。</p>
<p>第二个段落。</p>
<p>第三个段落。</p>
<p>第四个段落。</p>

<p><b>注释：</b>Internet Explorer 不支持 :nth-child() 选择器。</p>

</body>
</html>
```

。。。。。。未完待续
