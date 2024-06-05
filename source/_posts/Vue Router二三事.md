---
layout: vue
title: Router二三事
date: 2021-07-08 11:12:19
categories:
- Vue学习
tags:
- Vue
---
## 1.初步使用
安装命令：```npm install vue-router ```

创建```router```文件夹以及内部的```index.js```
```javascript
// router - index.js文件

import Vue from 'vue'
import VueRouter from "vue-router"

Vue.use(VueRouter)

const routes = [
  { path:'/', component:()=>import('../components/a.vue') },
  { path:'/b', component:()=>import('../components/b.vue') },
  { path:'/c', component:()=>import('../components/c.vue') },
]

const router = new VueRouter({
  routes
})
export default router
```
在项目的```main.js```文件中引入router即可
```javascript
// main.js 文件部分内容
import router from "@/router"

new Vue({
  router,
  …… ,
  render: (h) => h(App),
}).$mount("#app");
```

可以在```App.vue```文件中通过``` <router-view />```使用
```html
<template>
  <div id="app">
    <router-view />
  </div>
</template>
```

## 2.进一步配置
#### 情景一：访问不存在的路由
项目中访问不存在的路由，需要跳转到404页面。需要在```router```下的```index.js```文件中配置
```javascript
// router - index.js文件部分内容
import Vue from 'vue'
import VueRouter from "vue-router"

Vue.use(VueRouter)

const routes = [
  …… ,
  { path: "/404",component: () => import("@/views/error-page/404")},
  { path:'*', redirect:'/404'}
]

const router = new VueRouter({
  routes
})
export default router
```

#### 情景二：路由模式的设置
路由有两种模式：```hash模式``` 和 ```history模式```，设置时通过```mode```字段配置，默认模式为hash模式
```javascript
// router - index.js文件部分内容
import Vue from 'vue'
import VueRouter from "vue-router"

…… ,
const router = new VueRouter({
  routes,
  mode:'hash' // 'history'
})
export default router
```

#### 情景三：渲染多级路由 
渲染多级路由通过```children```字段来实现
```javascript
// router - index.js文件

import Vue from 'vue'
import VueRouter from "vue-router"

Vue.use(VueRouter)

const routes = [
  { path:'/', component:()=>import('../components/a.vue'), children:[
    {
      path:'aa', component:()=>import('../components/aa.vue')}
  ] },
  ……
]

const router = new VueRouter({
  routes
})
export default router
```
在```a.vue```文件中通过标签```<router-view/>```来使用
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220708130141.png)

#### 情景四；路由跳转
路由跳转使用```<router-link>```标签实现
请注意，我们没有使用常规的 ```a``` 标签，而是使用一个自定义组件 ```router-link``` 来创建链接。这使得 ```Vue Router``` 可以在不重新加载页面的情况下更改 ```URL```，处理 ```URL``` 的生成以及编码。我们将在后面看到如何从这些功能中获益。


