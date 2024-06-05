---
title: Echarts快速上手
date: 2021-09-14 09:14:10
categories:
- Echarts
tags:
- Echarts
---
## Echarts使用五部曲
+ 步骤1：下载并引入echarts.js文件
+ 步骤2：准备一个 <mark><b>具备大小</b></mark>的DOM容器
+ 步骤3：初始化echarts实例对象
+ 步骤4：指定配置项和数据（option）
+ 步骤5：将配置项设置给echarts实例对象
  
### 步骤1：下载并引入echarts.js文件
安装方式：
+ 从npm获取
```
npm install echarts --save
```
详见[在项目中引入 Apache ECharts](https://echarts.apache.org/handbook/zh/basics/import/)
+ 从 CDN 获取
从 jsDelivr 引用 [echarts](https://www.jsdelivr.com/package/npm/echarts)
+ 从 GitHub 获取
[apache/echarts](https://github.com/apache/echarts) 项目的 release 页面可以找到各个版本的链接。点击下载页面下方 Assets 中的 Source code，解压后 dist 目录下的 echarts.js 即为包含完整 ECharts 功能的文件

引入文件为：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210914093547.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210914093752.png)

### 步骤2：准备一个 <mark><b>具备大小</b></mark>的DOM容器
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210914100708.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210914100731.png)

### 步骤3：初始化echarts实例对象
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210914100559.png)

### 步骤4：指定配置项和数据（option）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210914101109.png)

### 步骤5：将配置项设置给echarts实例对象
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210914101150.png)

## Echarts的基础配置（option）
> 需要了解的主要配置：series xAxis yAxis grid tooltip title legend color toolbox

+ series 系列图标配置 决定显示那种类型的图表
+ xAxis 设置x轴的相关配置（boundaryGap是否让坐标轴和线条之间有缝隙）
+ yAxis 设置x轴的相关配置
+ grid 网格配置 可以控制线性图、柱状图等图表的大小（containLabel 显示图标刻度标签[true / false]）
+ tooltip 提示框组件（触发方式trigger [axis / item] ）
+ title 标题组件--设置图表的标题
+ legend 图例组件
+ color 调色板配置 设置线条颜色
+ toolbox 工具箱组件 可以另存为图片等功能

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210914104751.png)

### 图表跟随屏幕自动去适应
```javascript
window.addEventListener('resize', function () {
    myChart.resize()
})
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210914145903.png)