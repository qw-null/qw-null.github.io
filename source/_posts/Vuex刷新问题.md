---
title: 刷新浏览器后，Vuex的数据是否存在？如何解决？
date: 2022-08-19 22:11:14
categories:
- Vue学习
tags: 
- vue
---

在```vue```项目中用```vuex```来做全局的状态管理，发现当刷新网页后，保存在```vuex```实例```store```里的数据会丢失。
原因：因为```store```里面的数据是保存在运行内存中的，当页面刷新时，页面会重新加载```vue```实例，```store```里面的数据就会被重新赋值初始化。

解决方法有两种：
+ 使用```vuex-along```
+ 使用```localStorage```或者```sessionStorage```

#### 使用 vuex-along
vuex-along 的实质也是将```vuex```中的数据存放到```localStorage```或者```sessionStorage```中，只不过这个存取过程组件会帮我们完成，我们只需要用```vuex```的读取数据方式操作就可以了。
**使用方法：**
安装```vuex-along```：```npm install vuex-along --save```
配置```vuex-along```：在```store/index.js```中最后添加以下代码：
```javascript
import VueXAlong from 'vuex-along' //导入插件
export default new Vuex.Store({
    //modules: {
        //controler  //模块化vuex
    //},
    plugins: [VueXAlong({
        name: 'store',     //存放在localStroage或者sessionStroage 中的名字
        local: false,      //是否存放在local中  false 不存放 如果存放按照下面session的配置
        session: { list: [], isFilter: true } //如果值不为false 那么可以传递对象 其中 当isFilter设置为true时， list 数组中的值就会被过滤调,这些值不会存放在seesion或者local中
    })]
});
```

#### 使用 localStorage 或者 sessionStorage
```javascript
created() {
    //在页面加载时读取sessionStorage里的状态信息
    if (sessionStorage.getItem("store")) {
      this.$store.replaceState(
        Object.assign(
          {},
          this.$store.state,
          JSON.parse(sessionStorage.getItem("store"))
        )
      );
    }
    //在页面刷新时将vuex里的信息保存到sessionStorage里
    window.addEventListener("beforeunload", () => {
      sessionStorage.setItem("store", JSON.stringify(this.$store.state));
    });
},
```
