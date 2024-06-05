---
title: Vue项目实战：登录权限问题
date: 2022-08-16 10:42:00
tags:
- Vue
---

一个管理项目一般需要将：<b>1.登录功能</b> <b>2.权限验证功能</b> 分离

+ 登录：当用户填写完账号和密码后向服务端验证是否正确，验证通过之后，服务端会返回一个```token```，拿到```token```之后（我会将这个```token```存到```cookie```中，保证刷新页面后能记住用户登录状态），前端会根据```token```再去拉取一个 ```user_info``` 的接口来获取用户的详细信息（如用户权限，用户名等等信息）

+ 权限验证：通过```token```获取用户对应的 ```role```，动态根据用户的 ```role``` 算出其对应有权限的路由，通过 ```router.addRoutes``` 动态挂载这些路由

上述所有的数据和操作都是通过```vuex```全局管理控制的。(补充说明：刷新页面后 ```vuex```的内容也会丢失，所以需要重复上述的那些操作)接下来。

### 登录 + 获取用户信息
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220816113733.png)

### 权限验证

权限控制的整体思路：
1. 前端有一张路由表，表示每一个路由可访问的权限。
2. 用户登录之后，通过```token```获取用户的```role```，动态根据用户的```role```算出其对应的权限路由，再通过```router.addRoutes```动态挂载路由

**但这些控制都只是页面级的，说白了前端再怎么做权限控制都不是绝对安全的，后端的权限验证是逃不掉的。**

主流方式：
+ 前端控制页面级权限（不同用户显示不同的侧边栏、限制其能进入的页面）；
+ 后端则会验证每一个涉及请求的操作，验证其是否有该操作的权限，每一个后台的请求不管是 ```get``` 还是 ```post``` 都会让前端在请求 ```header```里面携带用户的 ```token```<span style="background:yellow;">【```request.js```文件中请求拦截器中添加】</span>，后端会根据该 ```token``` 来验证用户是否有权限执行该操作。（若没有权限则抛出一个对应的状态码，前端检测到该状态码，做出相对应的操作）

### 具体实现
1. 创建```vue```实例的时候将```vue-router```挂载，但这个时候```vue-router```挂载一些登录或者不用权限的公用的页面。
2. 当用户登录后，获取用```role```，将```role```和路由表每个页面的需要的权限作比较，生成最终用户可访问的路由表。
3. 调用```router.addRoutes(store.getters.addRouters)```添加用户可访问的路由。
4. 使用```vuex```管理路由表，根据```vuex```中可访问的路由渲染侧边栏组件。

```javascript
// router.js
import Vue from 'vue';
import Router from 'vue-router';

import Login from '../views/login/';
const dashboard = resolve => require(['../views/dashboard/index'], resolve);
//使用了vue-routerd的[Lazy Loading Routes
](https://router.vuejs.org/en/advanced/lazy-loading.html)

//所有权限通用路由表 
//如首页和登录页和一些不用权限的公用页面
export const constantRouterMap = [
  { path: '/login', component: Login },
  {
    path: '/',
    component: Layout,
    redirect: '/dashboard',
    name: '首页',
    children: [{ path: 'dashboard', component: dashboard }]
  },
]

//实例化vue的时候只挂载constantRouter
export default new Router({
  routes: constantRouterMap
});

//异步挂载的路由
//动态需要根据权限加载的路由表 
export const asyncRouterMap = [
  {
    path: '/permission',
    component: Layout,
    name: '权限测试',
    meta: { role: ['admin','super_editor'] }, //页面需要的权限
    children: [
    { 
      path: 'index',
      component: Permission,
      name: '权限测试页',
      meta: { role: ['admin','super_editor'] }  //页面需要的权限
    }]
  },
  { path: '*', redirect: '/404', hidden: true }
];

```
这里我们根据 ```vue-router官方推荐``` 的方法通过```meta```标签来标示改页面能访问的权限有哪些。如```meta: { role: ['admin','super_editor'] }```表示该页面只有 **admin** 和 **超级编辑** 才能有资格进入。

<span style="background:yellow;">注意事项：这里有一个需要非常注意的地方就是 404 页面 **一定要最后加载** ，如果放在constantRouterMap一同声明了404，后面的所以页面都会被拦截到404</span>


```javascript
//permission.js
import router from './router'
import store from './store'
import { Message } from 'element-ui'
import NProgress from 'nprogress'
import 'nprogress/nprogress.css'
import { getToken } from '@/utils/auth'
import { isRelogin } from '@/utils/request'

NProgress.configure({ showSpinner: false })

const whiteList = ['/login', '/auth-redirect', '/bind', '/register']

router.beforeEach((to, from, next) => {
  NProgress.start()
  if (getToken()) {
    to.meta.title && store.dispatch('settings/setTitle', to.meta.title)
    /* has token*/
    if (to.path === '/login') {
      next({ path: '/' })
      NProgress.done()
    } else {
      if (store.getters.roles.length === 0) {
        isRelogin.show = true
        // 判断当前用户是否已拉取完user_info信息
        store.dispatch('GetInfo').then(() => {
          isRelogin.show = false
          store.dispatch('GenerateRoutes').then(accessRoutes => {
            // 根据roles权限生成可访问的路由表
            router.addRoutes(accessRoutes) // 动态添加可访问路由表
            next({ ...to, replace: true }) // hack方法 确保addRoutes已完成
          })
        }).catch(err => {
            store.dispatch('LogOut').then(() => {
              Message.error(err)
              next({ path: '/' })
            })
          })
      } else {
        next()
      }
    }
  } else {
    // 没有token
    if (whiteList.indexOf(to.path) !== -1) {
      // 在免登录白名单，直接进入
      next()
    } else {
      next(`/login?redirect=${to.fullPath}`) // 否则全部重定向到登录页
      NProgress.done()
    }
  }
})

router.afterEach(() => {
  NProgress.done()
})

```

#### 小提一下：按钮权限的控制
首先按钮的权限可以使用```v-if```进行判断，但是如果页面过多，每个页面都需要获取用户权限```role```和路由表里的```meta.btnPermissions```，然后再做判断。

①配置路由表中的```btnPermissions``` (设置元信息)
②自定义权限指令
③在标签中使用```v-has```







