---
title: Vue项目实战：尚品汇
date: 2022-02-13 14:54:13
categories:
- Vue实战项目
tags:
- Vue
---

[尚硅谷VUE项目实战-尚品汇(大型\重磅)](https://www.bilibili.com/video/BV1Vf4y1T7bw?share_source=copy_web)

## 0 项目简介
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220213145705.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220213145730.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220213150118.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220213150219.png)

## 1 项目初始化

<b>★vue-cli脚手架初始化项目</b>

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220213151031.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220213152327.png)
-- 选择vue2版本

<b>★认识项目的组成</b>

+  ```node_modules```文件夹：项目依赖
+  ```public```文件夹：一般放置一些静态资源（图片），需要注意，放在public文件夹中的静态资源，webpack进行打包时，会原封不动的打包到dist文件夹中。
+ ```src```文件夹（程序员源代码文件夹）：
- - ```assets```文件夹：一般也是放置静态资源（一般放置多个组件共用的静态资源），需要注意，放置在assets文件夹里面的静态资源，在webpack打包的时候，webpack会把静态资源当作一个模块，打包到JS文件里面。
- - ```components```文件夹：一般放置的是非路由组件（全局组件）
- - ```App.vue```文件：唯一的根组件。Vue的组件都是```.vue```文件
- - ```main.js```文件：程序入口文件，也是整个程序中最先执行的文件

+ ```babel.config.js```文件：配置文件（babel相关）
+ ```package.json```文件：认为是项目的‘身份证’，记录项目叫什么，项目当中有哪些依赖、项目怎么运行。
+ ```package-lock.json```文件：缓存性文件
+ ```README.md```文件：说明性文件

<b>★项目的其他配置</b>

<b>1.项目运行起来时，让浏览器自动打开</b>

修改 package.json 文件

```json
"scripts": {
    "serve": "vue-cli-service serve --open",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  }
```

<b>2.eslint校验功能关闭</b>

方法：在根目录下创建```vue.config.js```文件

```javascript
module.exports = {
  // 关闭eslint
  lintOnSave: false
}
```

<b>3.配置别名（例如将src配置别名为@）</b>

方法：在根目录下创建```jsconfig.json```文件 
@代表的是src文件夹，当文件过多时可以方便查找

```json
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@/*": [
        "src/*"
      ]
    }
  },
  "exclude": [
    // 下列文件夹中不可使用
    "node_modules",
    "dist"
  ]
}
```

<b>★项目路由的分析</b>

vue-router

前端所谓的路由：k-v键值对。
key：URL（地址栏中的路径）
value：相应的路由组件

注意：项目上中下结构

* 路由组件：
Home首页路由组件、Search路由组件、login登录路由
* 非路由组件：
Header
Footer 在【首页、搜索页】，但在 【登录 | 注册】 页面是没有

## 2 完成非路由组件Header与Footer的业务

在项目中，不再以 HTML+CSS 为主，主要搞业务逻辑。

开发项目时：
1. 书写静态页面（HTML + CSS）
2. 拆分组件
3. 获取服务器的数据动态展示
4. 完成相应的动态业务逻辑

注意：
1. 创建组件的时候，需要保证 组件结构 + 组件样式 + 图片资源 正确
2. 项目采用less样式，但浏览器不识别less样式，需要通过less、less-loader进行处理less，把less样式变为css样式，浏览器才能识别。

```bash
// 安装less和less-loader【注意：less-loader不能安装最新版，否则报错】

npm install --save less less-loader@5
```
3. 如果想让组件识别less样式，需要在style标签的身上加上 ```lang="less"```

#### 2.1 使用组件的步骤（非路由组件）

- 创建或者定义
- 引入
- 注册
- 使用

#### 2.2 路由组件的搭建
vue-router
安装命令：```npm install --save vue-router@3```

上面对项目的分析可知，路由组件应该有四个：Home、Search、Login、Register

- ```components```文件夹：经常放置非路由组件（共用全局组件）
- ```pages | views```文件夹：经常放置路由组件

##### 2.2.1 配置路由
项目当中配置路由一般放置在```router```文件夹中

```javascript
// router下的index.js文件
// 配置路由
import Vue from 'vue'
import VueRouter from 'vue-router'

//使用插件
Vue.use(VueRouter);
//引入路由组件
import Home from '@/pages/Home'
import Search from '@/pages/Search'
import Login from '@/pages/Login'
import Register from '@/pages/Register'
//配置路由
export default new VueRouter({
  // 配置路由
  routes: [
    {
      path: "/home",
      component: Home
    },
    {
      path: "/login",
      component: Login
    },
    {
      path: "/register",
      component: Register
    },
    {
      path: "/search",
      component: Search
    }

  ]

})
```

##### 2.2.2 小结

<b>路由组件与非路由组件的区别？</b>

1. 路由组件一般放置在 ```pages | views```文件夹，非路由组件一般放置```components```文件夹中
2. 路由组件一般需要在```router```文件夹中进行注册（使用的即为组件的名字），非路由组件在使用的时候，一般都是以标签的形式使用
3. 注册完路由，不管路由组件还是非路由组件身上都有```$route```和```$router```属性

* ```$route``` :一般获取路由信息【路径、query、params等】

* ```$router``` :一般进行编程式导航，进行路由跳转【push | replace】

##### 2.2.3 路由跳转
路由跳转有两种形式：
+ 声明式导航```router-link```
+ 编程式导航```$router.push | $router.replace```

声明式导航能做的，编程式导航都能做。编程式导航除了可以进行路由跳转，还可以做一些其他的业务逻辑。

#### 2.3 Footer组件的显示与隐藏

Footer组件：在Home、Search显示Footer组件；在登录、注册时隐藏

显示或者隐藏组件：```v-if | v-show```
两者使用```v-show```更好一些，原因：```v-if```控制显示与否是通过操作DOM实现的，```v-if```会频繁的操作DOM，损耗性能。```v-show```仅仅通过样式显示元素（```display:block```）或者隐藏元素（```display:none```）。

+ 我们可以依据路由身上的```$route```获取当前路由的信息，通过路由路径（```this.$route.path```）判断Footer显示与隐藏。
+ 配置路由的时候，可以给路由添加路由元信息【meta】，路由需要配置对象，它的key不能乱写，需要依据开发文档。

#### 2.4 路由传参

<b>路由跳转有几种方式？</b>

路由跳转有两种形式：
+ 声明式导航```router-link```（务必要有 to 属性）
+ 编程式导航```$router.push | $router.replace```（可以书写一些自己的业务）

<b>路由传参，参数有几种写法？</b>

+ ```params```参数：属于路径当中的一部分，需要注意，在配置路由时，需要占位
+ ```query```参数：不属于路径当中的一部分，类似于ajax中的queryString ```/home?k=v&k=v```会直接拼接到地址后面,不需要占

```javascript
// 路由传递参数：
  // 第一种：字符串形式
  this.$router.push("/search/" + this.keyword + "?k=" + this.keyword.toUpperCase());
  // 第二种：模板字符串
  this.$router.push('/search/ ${this.keyword} ?k= ${this.keyword.toUpperCase())')
  // 第三种：对象【推荐】
  this.$router.push({
    name: 'search',
    params: { keyword: this.keyword },
    query: { k: this.keyword.toUpperCase() }
  })
```
路由配置时占位符写法：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220215104243.png)

##### 路由传参的相关面试题

<b>题目1：</b>路由传递参数（对象写法）path是否可以结合params参数一起使用？

答案：路由跳转传参的时候，对象的写法可以是name、path的形式，但是需要注意的是，path这种写法不能与params参数一起使用。params参数只能和name形式结合使用。

```javascript
// 第三种：对象【推荐】
  this.$router.push({
    name: 'search',
    params: { keyword: this.keyword },
    query: { k: this.keyword.toUpperCase() }
  })
```

<b>题目2：</b>如何指定params参数可传可不传？

如果路由要求传递params参数，但是你就不传递params参数，会发现跳转后的URL存在问题。
```已传递params参数：http://localhost:8080/#/search/aaa?k=AAA```
```未传递params参数：http://localhost:8080/#/?k=AAA```可以发现丢失```search```

答案：如何指定params参数可传可不传，在配置路由的时候，在占位符的后面添加一个问号【代表params参数可传递或者不传递】

```javascript
{
  path: "/search/:keyword?",
  component: Search,
  meta: { show: true },
  name: 'search'
},
```
<b>题目3：</b>params参数可以传递也可以不传递，但是如果传递的是空串，如何解决？

传递空串的params参数会导致路径出现问题

```javascript
this.$router.push({
  name: 'search',
  params: { keyword: '' },
  query: { k: this.keyword.toUpperCase() }
})
```
传递后的路径：
```http://localhost:8080/#/?k=AAA```

答案：使用undefined解决。
```javascript
this.$router.push({
  name: 'search',
  params: { keyword: '' || undefined },
  query: { k: this.keyword.toUpperCase() }
})
```

<b>题目4：</b>路由组件能不能传递props数据？

+ 第一种：布尔值写法
```javascript
{
  path: "/search/:keyword?",
  component: Search,
  meta: { show: true },
  name: 'search',
  // 第一种：布尔值写法
  props: true
}
```
在search页面通过props接收：
```props:['keyword']```

+ 第二种：对象写法
```javascript
    {
      path: "/search/:keyword?",
      component: Search,
      meta: { show: true },
      name: 'search',
      // 第二种：对象写法
      props: {
        a: 1,
        b: 2
      }
    }
```

在search页面通过props接收：
```props:['a','b']```

+ 第三种：函数写法：可以params参数，query参数，通过props传递给路由组件
```javascript
  props: ($route) => {
    return {
      keyword: $route.params.keyword,
      k: $route.query.k
    }
  }
```

##### ★★进一步的思考
<b>编程式导航跳转到当前路由（参数不变），多次执行会抛出NavigationDuplicated的警告错误？</b>

路由跳转有两种形式：编程式导航和声明式导航，上述问题不会出现在声明式导航中的，因为vue-router底层已经处理好了。

编程式导航进行路由跳转的时候出现这种错误的原因是当前的```vue-router```的版本是```3.5.3```，它引入了```promise```，因此可以通过给```push```方法传递相应的成功、失败的回调函数，捕获当前错误进行解决。
```javascript
this.$router.push({
  name: 'search',
  params: { ketword:this.keyword },
  query: { k:this.keyword.toUpperCase()}
},
()=>{},
()=>{})
```
但是上述这种解决方法治标不治本，在别的组件中```push | replace```，编程式导航还是要类似的错误。因此需要更加通用的解决方法。解决之前需要明确几点信息：

+ ```this```：指的是当前组件实例（search）

+ ```this.$router```属性：当前的这个属性，属性值VueRouter类的一个实例，当在入口文件注册路由的时候，给组件实例添加```$router | $route```属性

+ ```push```:VueRouter类的一个实例


```javascript

function VueRouter(){

}

// 原型对象的方法
VueRouter.prototype.push = function(){
  //函数的上下文为VueRouter类的一个实例
}

     ----------------------

let $router = new VueRouter();

$router.push(xxx);

```

最终解决方法：重写push、replace方法
```javascript
// router.js文件
// 先把VueRouter原型对象的push、replace方法保存一份
let originPush = VueRouter.prototype.push;
let originReplace = VueRouter.prototype.replace;

// call|apply区别
  // 相同点：都可以调用函数一次，都可以篡改函数的上下文一次
  // 不同点：call与apply传递参数：call传递参数用逗号隔开，apply方法执行需要传递数组

// 重写push | replace方法
// 第一个参数：告诉原来的push方法，你往哪里跳转（传递哪些参数）
// 第二个参数：成功的回调，第三个参数：失败的回调
VueRouter.prototype.push = function (location, resolve, reject) {
  if (resolve && reject) {
    originPush.call(this, location, rosolve, reject)
  } else {
    originPush.call(this.location, () => { }, () => { })

  }

}

VueRouter.prototype.replace = function (location, resolve, reject) {
  if (resolve && reject) {
    originReplace.call(this, location, resolve, reject)
  } else {
    originReplace.call(this.location, () => { }, () => { })
  }

}
```
## 3 Home组件
#### 3.1 组件拆分步骤
1. 先将静态页面完成
2. 拆分出静态组件
3. 获取服务器的数据进行展示
4. 完成动态业务

#### 3.2 全局组件的使用方法
首先明确全局组件的引入，注册需要在程序入口```main.js```中实现

例如，引如一个名为TypeNav的全局组件，```mian.js```中的代码：
```javascript
import TypeNav from '@/pages/Home/TypeNav'
// 第一个参数：全局组件的名字   第二个参数：哪一个组件
Vue.component(TypeNav.name, TypeNav)
```

## 4 axios
#### 4.1 为什么需要二次封装axios?
请求拦截器、响应拦截器：
+ 请求拦截器可以在发送请求之前处理一些业务
+ 响应拦截器，当服务器数据返回以后，可以处理一些事情

```bash
// 安装axios
npm install --save axios
```

#### 4.2 项目中api文件夹【axios】
在接口中，路径都带有```/api```，因此可将```/api```设置为baseURL
api下的request.js文件的基本结构如下：
```javascript
// 对于axios进行二次封装
import axios from 'axios';

// 1.利用axios对象的方法create去创建一个axios实例
const requests = axios.create({
  // 配置对象
  // 基础路径，发送请求的路径都会出现api
  baseURL: '/api',
  // 代表请求超时时间为5s
});

//请求拦截器：在发送请求之前，请求拦截器可以检测到，可以在请求发送出去之前做一些事情
requests.interceptors.request.use((config) => {
  //config：配置对象，对象里面有一个属性很重要->headers请求头

  return config;
})

// 响应拦截器
requests.interceptors.response.use((res) => {
  // 成功的回调函数：服务器相应数据回来以后，相应拦截器可以检测到，可以做一些事情
  return res.data;

}, (error) => {
  // 响应失败的回调函数
  return Promise.reject(error)
})



// 对外暴露
export default requests;
```

其中的一些配置内容可以参考[axios文档](http://www.axios-js.com/zh-cn/docs/)

#### 4.3 接口的统一管理
* 项目很小：完全可以在组件的生命周期函数中发送
* 项目很大：需要在```api```文件夹下的```index.js```中统一管理

##### 4.3.1 跨域问题
什么是跨域？协议、域名、端口号不同的请求，称之为跨域。

例如：
http://localhost:8080/#/home  -- 前端项目本地服务
http://39.98.123.211 -- 后台服务器

解决跨域的方法：```JSONP``` ```CROS``` ```代理```

+ 代理方法
在```webpack.config.js```文件 或者 ```vue.config.js```文件中设置代理
```javascript
module.exports = {
  //...
  devServer: {
    proxy: {
      '/api': {
        target: 'http://localhost:3000',//目标服务器
        pathRewrite: { '^/api': '' }, // 路径重写
      },
    },
  },
};

```

## 5 nprogress
这是一个类似youtube、Medium等网站上的小进度条插件。纳米级的进度条，涓涓细流动画告诉你的用户，一些事情正在发生！
安装命令：``` npm install --save nprogress ```

<b>项目当中如何使用？</b>

可以在项目接口的请求拦截器【进度条开始】和响应拦截器【进度条结束】中使用。

注意：在使用时要引入 ```nprogress.css``` 文件，否则无法显示

```javascript
// 引入进度条
import nprogress from 'nprogress'

// 引入进度条样式
import "nprogress/nprogress.css"

// start方法：进度条开始 done方法：进度条结束
nprogress.start();
nprogress.done();
```
❤ 如果想要修改进度条的颜色，可在```node_modules```中找到```nprogress```下的```nprogress.css```，修改其中的样式即可

## 6 vuex状态管理库

vuex是官方提供的一个状态管理库，可以集中式管理项目中组件共用的数据。
切记，并不是所有的项目都需要vuex，如果项目很小，完全不需要vuex，如果项目很大，组件很多，数据很多，数据维护很费劲，则使用vuex。

安装命令：```npm install --save vuex```

#### 6.1 vuex核心概念
state
mutations 
actidons
getters
modules

详情见 [vuex学习笔记](https://qw-null.github.io/2021/06/01/Vuex%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/)

#### 6.2 vuex实现模块式开发
如果项目过大，组件过多，接口也很多，数据很多，可以让vuex实现模块式开发。

