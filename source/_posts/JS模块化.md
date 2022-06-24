---
title: JS模块化
date: 2022-06-15 16:00:57
tags:
- JavaScript
---

### 1.什么是模块化？
在最开始将所有的JS代码都封装在同一个JS文件中，这样做带来了一定的问题：1.耦合度高，不方便后期维护；2. 功能点不明确；3.容易污染全局环境

将一个复杂的程序依据一定的规则（规范）封装成几个块（文件），并进行组合在一起，块的内部数据/实现是私有的，只是向外暴露一些接口（方法）与外部其他模块通信
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220615171844.png)
（*上述两种方式： 对象可以随时修改，一点不安全*）

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220615172030.png)

### 2.为什么要模块化？
1.降低复杂度、2.低耦合性、3.部署方便

### 3.模块化的好处？
1. 避免命名冲突
2. 更好的分离，按需加载
3. 更高复用性
4. 高可维护性

### 4.页面引载script
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220622235645.png)
引入模块化随之带来的问题是 1.请求过多、2.依赖模糊、3.难以维护  

因为模块化过程中可能会出现上述的问题，因此，提出了模块化规范的概念。

### 5.模块化规范
#### 5.1 CommonJS
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

#### 5.2 AMD (Asynchronous Module Definition)
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

#### 5.3 CMD (了解即可，由阿里大佬提出)
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

#### 5.4 ES6规范（⭐ 重要）
依赖模块需要编译打包处理（ES6的语法现在还有浏览器不支持，需要将ES6通过工具转换为ES5）

语法：
+ 导出模块：```export```
+ 引入模块：```import```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220623164111.png)

<b>暴露模块：</b>

```javascript
// 1.分多次导出模块的多个部分
export class Emp{ }
export function fun(){ }
export var person = {};

// 2.一次导出模块的多个部分
class Emp{  }
function fun(){  }
var person = {};
export {Emp, fun, person}

//3.default 导出只能有一个
export default { }
```
<b>导入模块：</b>

```javascript
// 1. 导入默认
import defaultModule from './myModule'; 

// 2. 导入指定的一个
import {Emp} from './myModule';
// 导入指定的多个
import {Emp, person} from './myModule';

// 3. 导入所有
import * as allFromModule from './myModule'; 
```
