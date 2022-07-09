---
title: Webpack5学习笔记
date: 2022-04-20 10:46:09
categories:
- Webpack
tags: 
- Webpack教程
---
视频地址：[尚硅谷2022版Webpack5入门到原理（面试开发一条龙）](https://www.bilibili.com/video/BV14T4y1z7sw?p=3&share_source=copy_web)

# 上篇：基础
# 0. 前言
### 为什么需要打包工具？
开发时，我们会使用框架（React、Vue），ES6模块化语法，Less/Sass 等css预处理器等语法进行开发。
这些代码在浏览器中时无法直接运行的，需要编译成浏览器能识别的JS、CSS等语法，才能运行。这就是打包工具需要完成的工作。
除此之外，打包工具还能<b> 压缩代码、做兼容性处理、提升代码性能 </b>等。
### 常见的打包工具
Grunt、Webpack、Vite、Gulp、Rollup等

# 1.Webpack 的基本使用
Webpack 是**一个静态资源打包工具**。
它会以一个或者多个文件作为打包入口，将整个项目所有文件编译组合成一个或者多个文件输出出去。
输出的文件就是编译好的文件，可以在浏览器直接运行。我们将Webpack输出的文件叫做```bundle```。
### 功能介绍
Webpack本身功能是有限的：
+ 开发模式：仅能编译JS中的```ES Module```语法。
+ 生产模式：仅能编译JS中的```ES Module```语法和压缩JS代码。

因此<span style="color:red;">本身只能处理JS代码，对于其他代码的处理需要通过其他配置来实现</span>。

### 开始使用（初步体验）
1. 资源目录
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220702211636.png)
2. 创建文件
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220702211721.png)
3. 下载依赖
+ 初始化 ```npm init -y```
此时会生成一个```package.json```文件
需要注意的是 ```package.json``` 中 ```name``` 字段不能叫做 ```webpack```, 否则下一步会报错

+ 下载依赖 ```npm i webpack webpack-cli -D```
4. 启用Webpack
+ 开发模式：```npx webpack ./src/main.js --mode=development```
+ 生产模式：```npx webpack ./src/main.js --mode=production```
 ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220702212252.png)

5. 观察输出文件
默认```Webpack```会将文件打包输出到```dist```目录下。只需查看```dist```目录下的文件情况就好。

### 小结
**Webpack 本身功能比较少，只能处理 js 资源，一旦遇到 css 等其他资源就会报错。**

# 2.Webpack 的基本配置
### 5 大核心概念
1. entry（入口）
指示Webpack从那个文件开始打包
2. output（输出）
指示Webpack打包完的文件输出到哪里去，如何命名等
3. loader（加载器）
Webpack本身只能处理js、json等资源，其他资源需要借助loader，Webpack才能解析
4. plugins（插件）
拓展Webpack的功能
5. mode（模式）
主要有两种：①开发模式：```development``` ②生产模式：```production```
### 配置文件
```javascript
/* -- 该文件在项目根目录下：webpack.config.js -- */
const path = require("path"); //nodejs核心模块，专门用来处理路径问题

// ✨ Webpack 是基于 Node.js 运行的，所以采用 Common.js 模块化规范

module.exports = {
  // 入口
  entry: './src/main.js', // 相对路径
  // 输出
  output: {
    // 文件的输出路径
    // __dirname nodejs的变量，代表当前文件的文件夹目录
    path: path.resolve(__dirname, "dist"), // 绝对路径
    // 文件的输出名称
    filename: 'main.js'
  },

  // 加载器
  module: {
    rules: [
      // loader配置
    ]
  },
  // 插件
  plugins: [
    // plugin配置

  ],
  // 模式
  mode: "development"
};
```
**Webpack 是基于 Node.js 运行的，所以采用 Common.js 模块化规范**
运行指令：```npx webpack```

# 3.开发模式介绍
顾名思义，就是开发代码时使用的模式。
主要完成两件事：
1. 编译代码，是浏览器能够运行
开发时有样式资源、字体图标、图片资源、html资源等，webpack默认都不能处理这些资源，所以要加载配置来编译这些资源。
2. 代码质量检查，树立代码规范
提前检查代码的一些隐患，让代码运行时更加健壮。
提前检查代码规范和格式，统一团队内编码风格。

# 4.处理样式资源
需要借助Loader来帮助Webpack解析样式资源。
### 处理CSS资源
1. 下载包 ```npm i css-loader style-loader -D```
2. 功能介绍
+ css-loader：负责将 Css 文件编译成 Webpack 能识别的模块
+ style-loader：会动态创建一个 Style 标签，里面放置 Webpack 中 Css 模块内容
此时样式就会以 Style 标签的形式在页面上生效
3. 配置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703001507.png)

4. 添加CSS资源
创建CSS文件，编写样式 ➡️ 在webpack入口文件引入（```entry:"./src/main.js"```）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703001758.png)

5. 运行指令```npx webpack```
此时可以在网页源代码中看到样式文件的内容
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703001924.png)

### 处理Less资源
1. 下载包 ```npm i less-loader -D```
2. 功能介绍
+ less-loader：负责将 Less 文件编译成 Css 文件
3. 配置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703124049.png)

其余步骤同上

### 处理 Sass 和 Scss 资源
1. 下载包 ```npm i sass-loader sass -D```
2. 功能介绍
+ sass-loader：负责将 sass 文件编译成 css 文件
+ sass：sass-loader 依赖 sass 进行编译
3. 配置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703141038.png)

# 5.处理图片资源
在过去Webpack4中，处理图片资源通过```file-loader```和```url-loader```进行处理。
现在，Webpack5中已经将两个Loader功能内置到Webpack里，只需简单的配置就可以处理图片资源。
1. 配置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703151535.png)
2. 使用图片资源（在css中引入图片）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703151629.png)
3. 对图片资源进行优化
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703151811.png)
优点：减少请求次数 ； 缺点：图片体积变大

# 6.修改输出文件的目录
1. 配置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703152856.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703152941.png)

# 7.自动清空上次打包的资源
1. 配置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703162904.png)

# 8.处理字体图标资源
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703172540.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703172734.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703172803.png)
3. 配置
🌟 在module -> rules 进行配置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703173049.png)

type: "asset/resource" 和 type: "asset"的区别：
1. ```type: "asset/resource"``` 相当于```file-loader```, 将文件转化成 ```Webpack``` 能识别的资源，其他不做处理
2. ```type: "asset"``` 相当于```url-loader```, 将文件转化成 ```Webpack``` 能识别的资源，同时小于某个大小的资源会处理成 ```data URI``` 形式

# 9.处理其他资源
1. 配置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703191702.png)

# 10.处理JS资源 
可能有人就会问，js资源Webpack不是已经处理了吗，为什么我们还要继续处理？
原因是Webpack对js的处理是有限的，只能编译js中ES模块化语法，不能编译其他语法，导致js不能在IE等浏览器运行，因此会去做一些兼容性的处理。其次，团队内部对代码格式是有严格要求的，需要使用专业的工具来进行检查。

+ 针对js兼容性处理，使用```Babel```来完成
+ 针对代码格式，使用```Eslint```来完成

先完成Eslint，检测代码格式无误后，再由Babel来做代码兼容。

### Eslint
可组装的JavaScript和JSX检查工具。（用来检测js和jsx语法的工具，可以配置各项功能）
1. 配置文件
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703194958.png)
2. 具体配置
以```.eslintrc.js```配置文件为例：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703195105.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703195139.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703195305.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703195416.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703195451.png)
比较有名的规则继承：
[Eslint 官方的规则：eslint:recommended](https://eslint.bootcss.com/docs/rules/)
[Vue Cli 官方的规则：plugin:vue/essential](https://github.com/vuejs/vue-cli/tree/dev/packages/@vue/cli-plugin-eslint)
[React Cli 官方的规则：react-app](https://github.com/facebook/create-react-app/tree/main/packages/eslint-config-react-app)
3. 在Webpack中使用（使用的是plugins）
+ 下载包  ```npm i eslint-webpack-plugin eslint -D```
+ 定义Eslint配置文件
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703200759.png)
+ 配置
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703201041.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703201117.png)

### Babel
1. 下载包 ```npm i babel-loader @babel/core @babel/preset-env -D ```
2. 定义Babel配置文件
+ babel.config.js
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703215954.png)
3. 配置
+ webpack.config.js（在module - rules）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703220101.png)

# 11.处理HTML资源
1. 下载包 ``` npm i html-webpack-plugin -D```
2. 配置（使用的是plugins）
+ webpack.config.js
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703221528.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703221611.png)
3. 修改index.html
去掉引入的 js 文件，因为 HtmlWebpackPlugin 会自动引入
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703221757.png)

# 12.开发服务器 & 自动化
1. 下载包 ```npm i webpack-dev-server -D```
2. 配置
+ webpack.config.js（单独开一项 devServer ）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703222431.png)
3. 运行指令 ```npx webpack serve```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703222642.png)
日常开发中使用的就是这种形式

# 13.生产模式介绍
生产模式是开发完成代码后，我们需要得到代码将来部署上线。
这个模式下我们主要对代码进行优化，让其运行性能更好。

优化主要从两个角度出发:
+ 优化代码运行性能
+ 优化代码打包速度

### 生产模式准备
1. 文件目录
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703223601.png)
2. 修改 webpack.dev.js
因为文件目录变了，所以所有绝对路径需要回退一层目录才能找到对应的文件
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703223718.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703223853.png)
3. 修改 webpack.prod.js
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703223821.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703223930.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703224542.png)
4. 配置运行指令
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703224542.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220703224634.png)
以后启动指令：
+ 开发模式：```npm start``` 或 ```npm run dev```
+ 生产模式：```npm run build```

# 中篇：Webpack高级配置
所谓的高级配置其实就是进行Webpack优化，让代码在编译/运行时性能更好
将会从以下角度进行优化：
+ 提升开发体验
+ 提升打包构建速度
+ 减少代码体积
+ 优化代码运行性能
