---
title: JS模块化
date: 2022-06-15 16:00:57
tags:
- JavaScript
---
> 在JS发展初期，主要是为了实现简单的页面逻辑交互。如今CPU、浏览器性能得到了极大的提升，很多页面逻辑迁移到了客户端（表单验证等），随着web2.0时代的到来，Ajax技术得到广泛应用，jQuery等前端库层出不穷，前端代码日益膨胀。在最开始将所有的JS代码都封装在同一个JS文件中的做法不再合适，这样做带来了一定的问题：1.耦合度高，不方便后期维护；2. 功能点不明确；3.容易污染全局环境。于是提出了模块化的概念。

### 1.什么是模块？
+ 将一个复杂的程序依据一定的规则（规范）封装成几个块（文件），并进行组合在一起
+ 块的内部数据/实现是私有的，只是向外暴露一些接口（方法）与外部其他模块通信

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220615171844.png)
（*上述两种方式： 对象可以随时修改，一点不安全*）

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220615172030.png)

### 2.模块化的好处？
1. 避免命名冲突（减少命名空间的污染）
2. 更好的分离，按需加载
3. 更高复用性
4. 高可维护性

### 3.页面引入多个\<script>
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220622235645.png)
引入模块化随之带来的问题是 
1. 请求过多：首先我们要依赖多个模块，那样就会发送多个请求，导致请求过多

2. 依赖模糊：我们不知道他们的具体依赖关系是什么，也就是说很容易因为不了解他们之间的依赖关系导致加载先后顺序出错。

3. 难以维护

因为模块化过程中可能会出现上述的问题，因此，提出了模块化规范的概念。

### 4.模块化规范

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/模块化规范.png)

#### 4.1 CommonJS
最开始出现的CommonJS仅仅针对服务器端，后来才逐步支持浏览器端。
> 每个文件都可当作一个模块（JS文件）
> 在服务器端：模块的加载运行是同步的
> 在浏览器端：模块需要提前编译打包处理

<b>基本语法：</b>
✨ 暴露模块：```module.exports = value(任意数据类型)``` 、```exports.xxx = value```

  Q: 暴露的模块到底是什么？➡️ A：暴露的是 ```exports``` 这个对象
 
✨ 引入模块：``` require（XXX）// 1.第三方模块：XXX为模块名；2.自定义模块：XXX为模块文件路径 ```

<b>实现：</b>
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220623101353.png) 

说明：浏览器是无法识别CommonJs的，所以需要引进Browserify打包成为浏览器可以识别的js文件

#### 4.2 AMD (Asynchronous Module Definition)
专门用于浏览器端，模块加载是异步的。
AMD依赖于一个库 ``` require.js ```

<b>基本语法：</b>
✨ 暴露模块：
```javascript
// 定义没有依赖的模块
define (function(){
  return 模块
})

//定义有依赖的模块
define(['module1','module2'],function(m1,m2){
  return 模块
})

//引入使用模块
require(['module1','module2'],function(m1,m2){
  使用m1 / m2
})

```

AMD的规范是依赖于```require.js```这个库的

第三方库jQuery默认支持AMD，在AMD中引入使用时，名字应该使用``` jquery ```

#### 4.3 CMD (了解即可，由阿里大佬提出)
专门用于浏览器端，模块的加载是异步的
模块使用时才会加载执行

<b>基本语法：</b>
✨ 定义暴露模块
```javascript
// 定义没有依赖的模块
define(function(require,exports,module){
  exports.xxx = value
  module.exports = value
})

// 定义有依赖的模块
define(function(require,exports,module){
  // 引入依赖模块（同步）
  var module2 = require('./module2')
  // 引入依赖模块（异步）
  require.async('./module3',function(m3){
    // 
  })
  // 暴露模块
  exports.xxx = value
})

```

✨ 引入使用模块
```javascript
define(function(require){
  var m1 = require('./module1')
  var m4 = require('./module4')
  m1.show()
  m4.show()
})
``` 
实际上，CMD是将CommonJS和AMD进行了组合，在定义暴露模块时采用AMD的做法，而暴露模块时采用CommonJS的方法。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220623152406.png)

#### 4.4 ES6 Module（⭐ 重要）

ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。

<b>✨ 规范：</b>
1. 每个文件都是一个模块
2. 要借助Babel和Browserify依次编译代码，才能在浏览器运行

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220623164111.png)
（*浅谈一嘴 ➡️ Babel的两个重要功能：1.将ES6转化为ES5； 2.将JSX转化为JS（React中）*）


<b>✨ 语法：</b>
+ 暴露模块：```export```:```export```命令用于规定模块的对外接口
+ 引入模块：```import```:```import```命令用于输入其他模块提供的功能



<b>暴露模块：</b>

```javascript
1.分别暴露： export 暴露内容
2.统一暴露： export {暴露内容1，暴露内容2}
3.默认暴露： export default 暴露内容
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220624150216.png)
（说明：想暴露谁就暴露谁，必须在 **正常代码** 的前面加export）


<b>引入模块：</b>

```javascript
1.方法1:  import {xxx,yyy} from './module1'
2.方法2:  import module3 from './module3'

// 1. 导入默认
import defaultModule from './myModule'

// 2. 导入指定的一个
import {Emp} from './myModule'
// 导入指定的多个
import {Emp, person} from './myModule'

// 3. 导入所有
import * as allFromModule from './myModule'
```

**➡️ ES6 模块与 CommonJS 模块的差异**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220627001731.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220627002132.png)

第二个差异是因为 CommonJS 加载的是一个对象（即module.exports属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

### 总结：
+ CommonJS规范主要用于服务端编程，加载模块是同步的，这并不适合在浏览器环境，因为同步意味着阻塞加载，浏览器资源是异步加载的，因此有了AMD CMD解决方案。

+ AMD规范在浏览器环境中异步加载模块，而且可以并行加载多个模块。不过，AMD规范开发成本高，代码的阅读和书写比较困难，模块定义方式的语义不顺畅。

+ CMD规范与AMD规范很相似，都用于浏览器编程，依赖就近，延迟执行，可以很容易在Node.js中运行。不过，依赖SPM 打包，模块的加载逻辑偏重

+ **ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案。**