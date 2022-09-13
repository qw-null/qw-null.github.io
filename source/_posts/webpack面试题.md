---
title: Webpack面试题
date: 2022-09-11 15:32:01
tags:
- Webpack
---

## 1.什么是Webpack？
`webpack` 是一个用于现代 `JavaScript` 应用程序的静态模块打包工具。宗旨是<mark>一切静态资源皆可打包</mark>。
有人就会问为什么要webpack？webpack是现代前端技术的基石，常规的开发方式，比如jquery,html,css静态网页开发已经落后了。现在是MVVM的时代，数据驱动界面。
webpack做的事情是，<mark>**分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用**</mark>。
当 webpack 处理应用程序时,它会递归地构建一个依赖关系图(dependency graph),其中包含应用程序需要的每个模块,然后将所有这些模块打包成一个或多个 bundle。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220912151111.png)

## 2.Webpack的核心概念
一共5个核心概念：`Entry（入口）`、`Output（出口）`、`Loader（模块转换器）`、`Plugins（插件）`、`Module(模块)`。

**1.Entry（入口）**：指示 webpack 应该使用哪个模块来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。
**2.Output（出口）**：告诉webpack在哪里输出它所创建的结果文件，以及如何命名这些文件，默认值为`./dist`。
**3.Loader（模块转换器）**：将所有类型的文件转换为webpack能够处理的有效模块，然后就可以利用webpack的打包能力，对它们进行处理。
**4.Plugins（插件）**：在webpack构建流程中的特定时机注入拓展逻辑来改变构建结果或你想要做的事情。
**5.Module(模块)**：开发者将程序分解成离散功能块，并称之为模块。在webpack里一个模块对应着一个文件，webpack会从配置的`Entry`开始递归找出所有依赖的模块。

## 3.webpack工作的原理是什么？工作流程是什么？
`webpack` 读取配置，根据入口开始遍历文件，解析依赖，使用`loader`处理各模块，然后将文件打包成`bundle`后输出到`output`指定的目录中。

webpack的工作流程是：
Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：
1. `初始化参数`：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数
2. `开始编译`：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译
3. `确定入口`：根据配置中的 entry 找出所有的入口文件
4. `编译模块`：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理
5. `完成模块编译`：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系
6. `输出资源`：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会
7. `输出完成`：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统

在以上过程中，Webpack 会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果。

## 4.常见的Loader有哪些？
`raw-loader`：加载文件原始内容（utf-8）
`file-loader`：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)
`url-loader`：与 file-loader 类似，区别是用户可以设置一个阈值，大于阈值会交给 file-loader 处理，小于阈值时返回文件 base64 形式编码 (处理图片和字体)
`source-map-loader`：加载额外的 Source Map 文件，以方便断点调试
`svg-inline-loader`：将压缩后的 SVG 内容注入代码中
`image-loader`：加载并且压缩图片文件
`json-loader`：加载 JSON 文件（默认包含）
`babel-loader`：把 ES6 转换成 ES5
`ts-loader`: 将 TypeScript 转换成 JavaScript
`awesome-typescript-loader`：将 TypeScript 转换成 JavaScript，性能优于 ts-loader
`sass-loader`：将SCSS/SASS代码转换成CSS
`css-loader`：加载 CSS，支持模块化、压缩、文件导入等特性
`style-loader`：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS
`postcss-loader`：扩展 CSS 语法，使用下一代 CSS，可以配合 autoprefixer 插件自动补齐 CSS3 前缀
`eslint-loader`：通过 ESLint 检查 JavaScript 代码
`tslint-loader`：通过 TSLint检查 TypeScript 代码
`coverjs-loader`：计算测试的覆盖率
`vue-loader`：加载 Vue.js 单文件组件
`i18n-loader`: 国际化
`cache-loader`: 可以在一些性能开销较大的 Loader 之前添加，目的是将结果缓存到磁盘里

## 5.常见的Plugin有哪些？
`define-plugin`：定义环境变量 (Webpack4 之后指定 mode 会自动配置)
`ignore-plugin`：忽略部分文件
`html-webpack-plugin`：简化 HTML 文件创建 (依赖于 html-loader)
`web-webpack-plugin`：可方便地为单页应用输出 HTML，比 html-webpack-plugin 好用
`uglifyjs-webpack-plugin`：不支持 ES6 压缩 (Webpack4 以前)
`terser-webpack-plugin`: 支持压缩 ES6 (Webpack4)
`webpack-parallel-uglify-plugin`: 多进程执行代码压缩，提升构建速度
`mini-css-extract-plugin`: 分离样式文件，CSS 提取为独立文件，支持按需加载 (替代extract-text-webpack-plugin)
`serviceworker-webpack-plugin`：为网页应用增加离线缓存功能
`clean-webpack-plugin`: 目录清理
`ModuleConcatenationPlugin`: 开启 Scope Hoisting
`speed-measure-webpack-plugin`: 可以看到每个 Loader 和 Plugin 执行耗时 (整个打包耗时、每个 Plugin 和 Loader 耗时)
`webpack-bundle-analyzer`: 可视化 Webpack 输出文件的体积 (业务组件、依赖第三方模块)

## 6.说一说Loader和Plugin的区别？
+ `Loader` 本质就是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果。因为 `Webpack` 只认识 `JavaScript`，所以 `Loader` 就成了翻译官，对其他类型的资源进行转译的预处理工作。（webpack是基于 `node` 的，只能处理 `JS`和 `JSON` 文件，`loader` 的作用是用来处理其他类型的文件（`less\vue....`等） 可以将 `less` 转成 `css` 文件，将 `jsx`处理成 `JS`文件，将其他版本的 `ES` 处理成浏览器能识别的 `ES` 版本）
+ `Plugin` 就是插件，基于事件流框架 `Tapable`，插件可以扩展 `Webpack` 的功能，在 `Webpack` 运行的生命周期中会广播出许多事件，`Plugin` 可以监听这些事件，在合适的时机通过 `Webpack` 提供的 `API` 改变输出结果。

+ `Loader` 在 `module.rules` 中配置，作为模块的解析规则，类型为数组。每一项都是一个 `Object`，内部包含了 `test(类型文件)`、`loader`、`options (参数)`等属性。
+ `Plugin` 在 `plugins` 中单独配置，类型为数组，每一项是一个 `Plugin` 的实例，参数都通过构造函数传入。

## 7.模块打包原理知道吗？
webpack实际上为每个模块创造了一个可以导出和导入的环境，本质上并没有修改代码的执行逻辑，代码执行顺序与模块加载顺序也完全一致。

## 8.文件监听的原理？
在发现源码发生变化时，自动重新构建出新的输出文件。

Webpack开启监听模式，有两种方式：
1. 启动webpack命令，带上`--watch`参数 
2. 在配置`webpack.config.js`中设置`watch:true`

缺点：每次需要手动刷新浏览器

原理：轮询判断文件的最后编辑时间是否发生变化，如果某个文件发生了变化，并不会立刻告诉监听者，而是先缓存起来，等`aggregateTimeout`后再执行。
```javascript
module.export = {
    // 默认false,也就是不开启
    watch: true,
    // 只有开启监听模式时，watchOptions才有意义
    watchOptions: {
        // 默认为空，不监听的文件或者文件夹，支持正则匹配
        ignored: /node_modules/,
        // 监听到变化发生后会等300ms再去执行，默认300ms
        aggregateTimeout:300,
        // 判断文件是否发生变化是通过不停询问系统指定文件有没有变化实现的，默认每秒问1000次
        poll:1000
    }
}
```

## 9.说一说Webpack的热更新原理？
webpack的热更新又称为热替换（`Hot Module Replacement`，缩写为`HMR`）。这个机制可以做到不用刷新浏览器而将新更新的模块替换掉旧的模块。

HMR的核心就是客户端从服务器端拉取更新后的文件，准确的说是`chunk diff`（`chunk`需要更新的部分），实际上`Webpack-dev-server`(`WDS`)与浏览器之间维护了一个`Websocket`，当本地资源发生变化时，`WDS`会向浏览器推送更新，并带上构建时的hash，让客户端与上一次的资源进行对比。客户端对比出差异后会向`WDS`发起`Ajax`请求来获取更新内容（文件列表、hash），这样客户端就可以再借助这些信息继续向`WDS`发起`jsonp`请求获取该`chunk`的增量更新。

后续的部分（拿到增量更新之后如何处理？哪些状态应该保留？哪些又需要更新？）由`HotModulePlugin`来完成，提供了相关的API以供开发者针对自身场景进行处理，像`vue-loader`和`react-hot-loader`都是借助这些API来实现HMR。

在webpack中开启热模块：
```javascript
const webpack = require('webpack')
module.exports = {
  // ...
  devServer: {
    // 开启 HMR 特性
    hot: true
    // hotOnly: true
  }
}
```

## 10.文件指纹是什么？怎么用？
文件指纹是打包后输出的文件名的后缀。
+ `Hash`：和整个项目的构建相关，只要项目文件有修改，整个项目构建的hash值就会更改
+ `Chunkhash`：和webpack打包的chunk有关，不同的`entry`会生出不同的chunkhash
+ `Contenthash`：根据文件内容来定义`hash`，文件内容不变，则`Contenthash`不变
#### JS的文件指纹设置
设置`output`的`filname`，用`chunkhash`
```javascript
module.exports = {
    entry: {
        app: './scr/app.js',
        search: './src/search.js'
    },
    output: {
        filename: '[name][chunkhash:8].js',
        path:__dirname + '/dist'
    }
}
```
#### CSS的文件指纹设置
设置 `MiniCssExtractPlugin` 的 `filename`，使用 `contenthash`。
```javascript
module.exports = {
    entry: {
        app: './scr/app.js',
        search: './src/search.js'
    },
    output: {
        filename: '[name][chunkhash:8].js',
        path:__dirname + '/dist'
    },
    plugins:[
        new MiniCssExtractPlugin({
            filename: `[name][contenthash:8].css`
        })
    ]
}

```
#### 图片的文件指纹设置
设置`file-loader`的`name`，使用`hash`
占位符名称及含义：
+ ext：资源后缀名
+ name：文件名称
+ path：文件的相对路径
+ folder：文件所在的文件夹
+ contenthash：文件的内容hash，默认是md5生成
+ hash：文件内容的hash，默认是md5生成
+ emoji：一个随机的指代文件内容的emoj
```javascript
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename:'bundle.js',
        path:path.resolve(__dirname, 'dist')
    },
    module:{
        rules:[{
            test:/\.(png|svg|jpg|gif)$/,
            use:[{
                loader:'file-loader',
                options:{
                    name:'img/[name][hash:8].[ext]'
                }
            }]
        }]
    }
}
```