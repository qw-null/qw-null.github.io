---
title: Baidu的CSS手写题
date: 2022-07-25 09:05:46
tags:
- 各种无厘头手写
---
> 输了就是输了，这说明什么呀，小朋友，还得练。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725090509.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725091814.png)

题目难点在于：中间部分（aside和section的高度未指定，但仍需要占满整个空白区域）
解决办法：```position:fixed```确定中间部分在页面的位置，aside指定宽高，section指定```align-self:stretch;```

<iframe width="100%" height="700" src="//jsrun.net/pgPKp/embedded/all/dark" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Baidu's CSS</title>
</head>
<body>
  <header>header</header>
  <div class="container">
    <div class="aside">aside</div>
    <div class="section">section</div>
  </div>
  <footer>footer</footer>

<style>
  *{
    margin: 0;
    padding: 0;
    width: 100%;
  }
  header{
    height: 100px;
    background-color: red;
  }
  footer{
    position: fixed;
    bottom:0;
    width: 100%;
    height: 100px;
    background-color: yellow;
  }
  
  @media screen and (min-width:500px) {
    .container{
      position: fixed;
      top:110px;
      bottom: 110px;
      display: flex;
    }
    .aside{
      width: 200px;
      height: 200px;
      background-color: blue;
      margin-right: 10px;
    }
    .section{
      flex: 1;
      align-self: stretch;
      background-color: green;
    }
  }

    @media screen and (max-width:500px) {
    .container{
      position: fixed;
      top:110px;
      bottom: 110px;
      display: flex;
      flex-direction: column;
    }
    .aside{
      width: 100%;
      height: 200px;
      background-color: blue;
      /* align-self: flex-start; */
      margin-bottom: 10px;
    }
    .section{
      flex: 1;
      align-self: stretch;
      background-color: green;
    }
  }
</style>
  
</body>
</html>
```