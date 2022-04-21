---
title: Webpack5学习笔记
date: 2022-04-20 10:46:09
categories:
- Webpack
tags: 
- Webpack教程
---
视频地址：[尚硅谷新版Webpack5实战教程(从入门到精通)](https://www.bilibili.com/video/BV1e7411j7T5?share_source=copy_web)

# 1.Webpack简介
Webpack是一种前端资源构建工具，一个静态资源打包器（module bundler）
在webpack看来，前端所有资源文件（js/jso/css/img/less/...）都会作为模块处理。
它将根据模块的依赖关系进行静态分析，打包生成对应的静态资源（bundle）

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220420115745.png)

# 2.Webpack五个核心概念

## 2.1 Entry
入口（Entry）指示Webpack以哪个文件为入起点开始打包，分构建内部依赖图。

## 2.2 Output
输出（Output）指示Webpack打包后的资源bundles输出到哪里去，以及如何命名。

## 2.3 Loader
Loader让Webpack能够去处理那些非Javascript文件（webpack自身只能理解Javascript）

## 2.4 Plugins
插件（Plugins）可以用于执行范围更广的任务。插件的范围包括 从打包优化和压缩，一直到重新定义环境中的变量等。

## 2.5 Mode
模式（Mode）指示webpack使用相应模式的配置。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220420142019.png)