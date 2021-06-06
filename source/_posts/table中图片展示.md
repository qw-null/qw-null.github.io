---
title: table中图片展示
date: 2021-06-06 21:33:46
categories:
- CSS样式
tags:
- HTML
- CSS
---

在人海里相遇的人终究要还给人海。

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210606213709.png" style="zoom:175%;" />

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Table中横向图片展示</title>
</head>

<body>
  <h4>Table横向多个图片展示</h4>
  <p>每个表格由 table 标签开始。</p>
  <p>每个表格行由 tr 标签开始。</p>
  <p>每个表格数据由 td 标签开始。</p>
  <div>
    <table class="table">
      <thead>
        <tr>
          <th>序号</th>
          <th>名称</th>
          <th>图片展示</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>1</td>
          <td>阿里</td>
          <td>
            <div class="table_img" onmousewheel="handler()">
              <img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/pexels-pixabay-2150.jpg" alt="" class="imgAuto">
              <img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/pexels-scott-webb-1098520.jpg" alt=""
                class="imgAuto">
              <img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/pexels-pixabay-2150.jpg" alt="" class="imgAuto">
              <img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/pexels-scott-webb-1098520.jpg" alt=""
                class="imgAuto">
              <img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/pexels-pixabay-2150.jpg" alt="" class="imgAuto">
              <img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/pexels-scott-webb-1098520.jpg" alt=""
                class="imgAuto">
              <img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/pexels-pixabay-2150.jpg" alt="" class="imgAuto">
              <img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/pexels-scott-webb-1098520.jpg" alt=""
                class="imgAuto">
              <img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/pexels-pixabay-2150.jpg" alt="" class="imgAuto">
              <img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/pexels-scott-webb-1098520.jpg" alt=""
                class="imgAuto">
              <img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/pexels-pixabay-2150.jpg" alt="" class="imgAuto">
            </div>
          </td>
        </tr>
        <tr>
          <td>2</td>
          <td>腾讯</td>
          <td></td>
        </tr>
        <tr>
          <td>3</td>
          <td>百度</td>
          <td></td>
        </tr>
      </tbody>
    </table>
  </div>

</body>
<script type="text/javascript">
  function handler() {
    // console.log('mousewheel信息', event);
    var detail = event.wheelDelta || event.detail;
    var item = event.currentTarget;
    var moveForwardStep = 1;
    var moveBackStep = -1;
    var step = 0;
    if (detail < 0) {
      step = moveForwardStep * 100;
    } else {
      step = moveBackStep * 100;
    }
    item.scrollLeft += step;

  }

</script>
<style>
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


  .imgAuto {
    /* 图片样式 */
    display: inline-block;
    height: 80px;
    max-width: 350px;
  }

  .table_img {
    height: 100px;
    width: 350px;
    overflow-y: auto;
    white-space: nowrap;
  }

  .table_img::-webkit-scrollbar {
    /*滚动条整体样式*/
    width: 3px;
    /*高宽分别对应横竖滚动条的尺寸*/
    height: 10px;
  }

  .table_img::-webkit-scrollbar-thumb {
    /*滚动条里面小方块*/
    border-radius: 10px;
    background-color: rgba(159, 160, 162, 0.8);
  }

  .table_img::-webkit-scrollbar-track {
    /*滚动条里面轨道*/
    border-radius: 10px;
  }
</style>

</html>

```