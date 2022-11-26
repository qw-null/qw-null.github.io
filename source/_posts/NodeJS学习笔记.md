---
title: NodeJS学习笔记
date: 2022-11-22 20:21:36
tags:
- Node.js
---

## 一、初识Node.js
### 1 Node.js简介
> Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine.
> Node.js是一个基于Chrome V8引擎的JavaScript运行环境。

**Node.js中的JavaScript运行环境**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20221122205249.png)
*⚠️注意点：*
① 浏览器是JavaScript的前端运行环境；
② Node是JavaScript的后端运行环境；
③ Node.js中无法调用DOM和BOM等浏览器内置API；

### 2 Node.js学习路线
JavaScript基础语法 + Node.js内置API模块（fs、path、http等）+ 第三方API模块（express、mysql等）

### 3 Node.js环境安装
安装包见 [Node.js官网](https://nodejs.org)
**LTS版本和Current版本区别：** LTS版本为稳定版、Current版本为测试版
通过查看版本号确定Node.js是否安装成功：`node -v`

## 二、fs文件系统模块
### 1 什么是fs文件系统模块

`fs模块` 是Node.js官方提供的、用来操作文件的模块。它提供了一系列的方法和属性，用来满足用户对文件的操作需求。例如：
+ `fs.readFile()`方法，用来读取指定文件中的内容
+ `fs.writeFile()`方法，用来向指定的文件中写入内容

如果在JavaScript代码中，使用fs模块来操作文件，则需要使用如下的方式先导入它：`const fs = require('fs')`

### 2 fs.readFile()的语法格式
`fs.readFile(path[, options], callback)`

+ path：必选参数，表示文件的路径
+ options：可选参数，表示用什么编码格式来读取文件
+ callback：必选参数，文件读取完成后，通过回调函数拿到读取的结果

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202211222124055.png)

### 3 fs.writeFile()的语法格式
`fs.writeFile(file, data[, options], callback)`

+ file：必选参数，需要指定一个文件路径的字符串，表示文件的存放路径
+ data：必选参数，表示要写入的内容
+ options：可选参数，表示用什么编码格式来写入文件，默认值是utf-8
+ callback：必选参数，文件写入完成后的回调函数

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202211222149089.png)

### 4 fs模块-动态路径拼接错误的问题
在使用fs模块操作文件时，如果提供的操作路径是以`./`或者`../`开头的相对路径，很容易出现路径动态拼接错误的问题。
原因：代码在运行时，会以执行node命令所处的目录，动态拼接出被操作文件的完整路径。

解决方法：
⭕️ 直接提供一个完整的文件存放路径（缺点：移植性差，不利于维护）
⭕️ 使用`__dirname` 【表示当前文件所处的目录】

## 三、path路径模块
### 1 什么是path路径模块

path模块是Node.js官方提供的、用来处理路径的模块。它提供了一系列的方法和属性，用来满足用户对于路径处理的需求。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202211231937336.png)

在JavaScript代码中，使用path模块来处理路径，需要先导入：`const path = require('path')`

### 2 path.join()语法格式
使用path.join()方法可以把多个路径片段拼接为完整的路径字符串：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202211231948158.png)
*⚠️注意：今后涉及路径拼接，使用path.join()方法*

### 3 path.basename()语法格式
使用path.basename()方法，可以从一个文件路径中获取到文件的名称部分
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202211232010996.png)

### 4 path.extname()语法格式
使用path.extname()方法可以获取路径文件中的拓展名部分
语法格式：`path.extname(path)`
参数解读：
+ path：\<string>必选参数，表示一个路径的字符串

返回：\<string>返回得到的拓展名字符串
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202211232127536.png)

## 三、http模块
### 1 什么是http模块
http模块是Node.js官方提供的、用来创建web服务器的模块。通过http模块提供的`http.createServer()`方法，就能方便的把一台普通电脑变成一台Web服务器，从而对外提供Web资源服务。
导入http模块：`const http = require('http')`

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202211241035474.png)

### 2 创建web服务器的基本步骤

#### ① 导入http模块
`const http = require('http');`
#### ② 创建web服务器实例
`const server = http.createServer()`
#### ③ 为服务器实例绑定request事件，监听客户端的请求
```node
// 使用创建服务器实例的.on()方法，为服务器绑定一个request事件

server.on('request',(req,res)=>{
  console.log(' Someone visit our web server.')
})
```
#### ④ 启动服务器
```node
// 调用server.listen(端口号,cb回调)方法，即可启动web服务器
server.listen(80,()=>{
  console.log(' http server running at http://127.0.0.1');
})
```

### 3 根据不同的URL响应不同的html内容
核心实现步骤：
① 获取请求的url地址
② 设置默认的响应内容为404 Not Found
③ 判断用户请求的是否为 `/` 或 `/index.html`首页
④ 判断用户请求是否为`/about.html`关于页面
⑤ 设置Content-Type响应头，防止中文乱码
⑥ 使用`res.end()`把内容响应给客户端
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202211241629075.png)

## 四、模块化
### 1 什么是模块化？
模块化是指解决一个复杂问题时，自顶向下逐层把系统划分成若干模块的过程。对于整个系统来说，模块是可组合、分解和更换的单元。

**代码进行模块化拆分的好处：**
a.提高了代码的复用性；b.提高了代码的可维护性；c.可以实现按需加载；

模块化规范就是对代码进行模块化拆分和组合时，需要遵循的规范。
好处：降低沟通成本，极大方便各个模块之间的相互调用。

**Node.js中的模块化规范**
Node.js中根据模块来源不同，将模块分为3类：
+ 内置模块
+ 自定义模块
+ 第三方模块

**加载模块**
使用`require()`方法进行模块加载
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202211251522118.png)
*⚠️注意：使用`require()`方法加载其他模块时，会执行被加载模块中的代码*

### 2 什么是模块作用域？
和函数作用域类似，在自定义模块中定义的变量、方法等成员，只能在当前模块内被访问，这种模块级别的访问限制，叫做模块作用域。

好处：防止全局变量污染的问题

### 3 module对象
在每个`.js`自定义模块中都有一个模块对象，它存储了和当前模块相关的信息。

`module.exports()`对象：在自定义模块中，可以使用`module.exports`对象，将模块内的成员共享出去，供外界使用。外界使用`require()`方法导入自定义模块时，得到的就是`module.exports`所指向的对象。

### 4 exports对象
由于`module.exports`写起来比较复杂，为了简化向外共享成员的代码，`Node`提供了`exports`对象。默认情况下，`exports`和`module.exports`指向同一个对象。最终共享的结果，还是以`module.exports`指向的对象为准。

 

