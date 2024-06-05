---
title: 阿里iconfont使用方法
date: 2021-07-20 09:53:34
categories:
- 阿里iconfont
tags:
- 阿里iconfont
---

阿里巴巴iconfont的使用方式分为两种：1.本地使用  2.线上引用
<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211013192203.jpg" width="150%" height="150%"/>

[iconfont-阿里巴巴矢量图标库](https://www.iconfont.cn/)

## 0.前期准备
使用时先到网站上选中需要使用的图标添加至项目
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210720101011.png?1000x)
## 1.本地使用
1.下载项目至本地
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210720101230.png)
2.解压到项目中
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210720103442.png)
3.在页面中引入样式
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>iconfont的使用--本地引入</title>
	<style>
		@font-face {
		  font-family: 'iconfont';
		  src: url('./iconfont/iconfont.woff2?t=1626747157081') format('woff2'),
		       url('./iconfont/iconfont.woff?t=1626747157081') format('woff'),
		       url('./iconfont/iconfont.ttf?t=1626747157081') format('truetype');
		}
		.iconfont {
		  font-family: "iconfont" !important;
		  font-size: 100px;
		  font-style: normal;
		  -webkit-font-smoothing: antialiased;
		  -moz-osx-font-smoothing: grayscale;
		}
	</style>
</head>
<body>
	<span class="iconfont">&#xe89a;</span>	
</body>
</html>
```
注意：
+ @font-face中的url需要依据项目修改
+ 使用方式：	\<span class="iconfont">\&#xe89a;\</span>

## 2.线上引用
#### 2.1 unicode引用
将本地使用代码中的@font-face进行替换即可
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210720111233.png)

#### 2.2 Font class形式引入：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210720111005.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>iconfont的使用--线上引用</title>
	<link rel="stylesheet" href="http://at.alicdn.com/t/font_2686372_j1gr8zzfq9.css">
	<style>
		.iconfont {
		  font-family: "iconfont" !important;
		  font-size: 100px;
		  font-style: normal;
		  -webkit-font-smoothing: antialiased;
		  -moz-osx-font-smoothing: grayscale;
		}
	</style>
</head>
<body>
	<i class="iconfont icon-chazuo"></i>
</body>
</html>
```
在Vue中使用方法
```html
<!--App.vue中引入样式文件，用线上链接即可-->
<style>
	@import "xxxxxxx/iconfont.css";
</style>

<!--class方式引用-->
<div class="iconfont icon-qianbao"></div>

```
#### 2.3 symbol引用
+ 支持多色图标了，不再受单色限制。
+ 通过一些技巧，支持像字体那样，通过,来调整样式。
+ 兼容性较差，支持 ie9+及现代浏览器。
+ 浏览器渲染svg的性能一般，还不如png。
> 使用步骤：

1.第一步：拷贝项目下面生成的symbol代码：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210720112720.png)
```html
<script src="http://拷贝的地址"></script>
```
2.第二步：加入通用css代码（引入一次就行）
```css
<style type="text/css">
    .icon {
       width: 1em; height: 1em;
       vertical-align: -0.15em;
       fill: currentColor;
       overflow: hidden;
    }
</style>
```
3.第三步：挑选相应图标并获取类名，应用于页面：
```html
<svg class="icon" aria-hidden="true">
    <use xlink:href="#icon-xxx"></use>
</svg>
```
实例：
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>iconfont的使用--线上引用</title>
	<script src="http://at.alicdn.com/t/font_2686372_j1gr8zzfq9.js"></script>
	<style src="text/css">
		.iconfont {
		  font-family: "iconfont" !important;
		  font-size: 100px;
		  font-style: normal;
		  -webkit-font-smoothing: antialiased;
		  -moz-osx-font-smoothing: grayscale;
		}
	</style>
</head>
<body>
	<svg class="icon" aria-hidden="true">
	    <use xlink:href="#icon-chazuo"></use>
	</svg>
</body>
</html>
```

