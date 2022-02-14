---
title: Vue项目实战：尚品汇
date: 2022-02-13 14:54:13
categories:
- Vue实战项目
tags:
- VUE
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

1. 项目运行起来时，让浏览器自动打开
```package.json```文件中
```json
"scripts": {
    "serve": "vue-cli-service serve --open",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  }
```

2. eslint校验功能关闭
方法：在根目录下创建```vue.config.js```文件

```javascript
module.exports = {
  // 关闭eslint
  lintOnSave: false
}
```

3. 配置别名（例如将src配置别名为@）
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

路由组件：
Home首页路由组件、Search路由组件、login登录路由
非路由组件：
Header
Footer【在首页、搜索页】，但在 登录 | 注册 页面是没有

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

## 3 路由组件的搭建
vue-router
安装命令：```npm install --save vue-router@3```

上面对项目的分析可知，路由组件应该有四个：Home、Search、Login、Register

- ```components```文件夹：经常放置非路由组件（共用全局组件）
- ```pages | views```文件夹：经常放置路由组件

#### 3.1 配置路由
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

#### 3.2 小结

<b>路由组件与非路由组件的区别？</b>

1. 路由组件一般放置在 ```pages | views```文件夹，非路由组件一般放置```components```文件夹中
2. 路由组件一般需要在```router```文件夹中进行注册（使用的即为组件的名字），非路由组件在使用的时候，一般都是以标签的形式使用
3. 注册完路由，不管路由组件还是非路由组件身上都有```$route```和```$router```属性

```$route```:一般获取路由信息【路径、query、params等】

```$router```:一般进行编程式导航，进行路由跳转【push | replace】

#### 3.3 路由跳转
