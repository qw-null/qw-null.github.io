---
title: Vue3了解过吗？
date: 2022-08-12 22:06:29
tags:
- Vue
---

### 一、Vue3的介绍
关于```vue3```的重构背景，尤大是这样说的：
> Vue新版本的理念成型于2018年末，当时Vue2的代码库已经有两岁半了。比起通用软件的生命周期这好像也没有那么久，但在这段时期，前端世界已经今非昔比了。
> 我们在更新（和重写）Vue的主要版本时，主要考虑两点因素：首先是<b style="background:yellow;">新的JavaScript语言特性在主流浏览器中的受支持水平</b>；其次是<b style="background:yellow;">当前代码库中随时间推移而逐渐暴露出来的一些设计和架构问题</b>。

简要来说就是：
+ 利用新的语言特性（ES6）
+ 解决架构问题

#### 1.1 哪些变化？
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220812221706.png)

Vue3的新特性可以概括为：
+ 速度更快
+ 体积更小
+ 更易维护
+ 更接近原生
+ 更易使用

##### 速度更快
Vue3 相比 Vue2：
+ 重写了虚拟 ```DOM ``` 实现
+ 编译模板的优化
+ 更高效的组件初始化
+ update性能提高 1.3～2倍
+ SSR速度提高了 2～3倍

##### 体积更小
通过```webpack```的```tree-shaking```功能，可以将无用模块“剪辑”，仅打包需要的能够```tree-shaking```，这样做有两大好处：
+ 对于开发人员，能够对```vue```实现更多其他功能，而不必担忧整体体积过大
+ 对于使用者，打包出来的包体积变小了

vue可以开发出更多其他的功能，而不必担忧```vue```打包出来的整体体积过多

##### 更易维护
**Composition API**
+ 可与现有的```Options API```一起使用
+ 灵活的逻辑组合与复合
+ ```Vue3```模块可以和其他框架搭配使用

**更好的Typescipt支持**
`Vue3`是基于`typescript`编写的，可以享受到自动的类型定义提示

**编译器重写**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220815203743.png)

##### 更接近原生
可以自定义渲染API
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220815204715.png)

##### 更易使用
响应式```API```暴露出来
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220815204829.png)

轻松识别组件重新渲染原因
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220815204934.png)

### 二、Vue3新增特性
Vue3中需要关注的一些新功能包括：
+ framents
+ Teleport
+ composition API
+ createRenderer

##### framents
在```vue3.x```中，组件现在支持有多个根组件
```html
<template>
  <header>...</header>
  <main v-bind="$attrs"> ... </main>
  <footer>...</footer>
</template>
```

##### Teleport
Teleport是一种能够将我们的模板移动到DOM中的```Vue app```之外的其他位置的技术，有点像```position:absolute```完全跳出页面的限制。

在```vue2```中，像```modals```、```toast```等这样的元素，如果我们嵌套在```vue```的某个组件内部，那么处理嵌套组件的定位、```z-index```和样式就会变得很困难

通过```Teleport```，我们可以在组件的逻辑位置写模板代码，然后在```Vue```应用范围之外渲染它
```html
<button @click="showToast" class="btn">打开 toast</button>
<!-- to 属性就是目标位置 -->
<teleport to="#teleport-target">
    <div v-if="visible" class="toast-wrap">
        <div class="toast-msg">我是一个 Toast 文案</div>
    </div>
</teleport>
```

##### createRenderer
通过```createRenderer```，我们能够构建自定义渲染器，我们能够将```vue```的开发模型扩展到其他平台
我们可以讲其生成在```canvas```画布上
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220815211353.png)

关于```createRenderer```，基本使用方法如下：
```javascript 
import { createRenderer } from '@vue/runtime-core'

const { render, createApp } = createRenderer({
  patchProp,
  insert,
  remove,
  createElement,
  // ...
})

export { render, createApp }

export * from '@vue/runtime-core'
```

##### composition API 
composition API，也就是组合式```API```，通过这种形式，我们能够更加容易维护我们的代码，将相同功能的变量进行一个集中式的管理
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220815230531.png)

关于```composition API```的使用，这里以下图展开：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220815230704.png)

简单使用：
```javascript
export default {
    setup() {
        const count = ref(0)
        const double = computed(() => count.value * 2)
        function increment() {
            count.value++
        }
        onMounted(() => console.log('component mounted!'))
        return {
            count,
            double,
            increment
        }
    }
}
```
### 三、非兼容变更
**Global API**
+ 全局```vue API```已更改为使用应用程序实例
+ 全局和内部```API```已经被重构伟可```tree-shakable```

**模板指令**
+ 组件上```v-model```用法已更改
+ ```<template v-for>```和非```v-for```节点上```key```用法已更改
+ 在同一元素上使用的```v-if```和```v-for```的优先级已更改
+ ```v-bind="object"```现在排序敏感
+ ```v-for```中的```ref```不再注册```ref```数组

**组件**
+ 只能使用普通函数创建功能组件
+ ```functional```属性在单文件组件（SFC）
+ 异步组件现在需要```defineAsyncComponent```方法来创建

**渲染函数**
+ 渲染函数```API```改变
+ ```$scopeSlots```property 已删除，所有插槽都经过```$slot```作为函数暴露
+ 自定义指令API已更改为与组件生命周期一致
+ 一些转换```class```被重命名了：```v-enter ➡️ v-enter-from```、```v-leave ➡️ v-leave-from```
+ 组件```watch```选项和实例方法```$watch```不再支持点分隔字符串路径，该用计算函数作为参数
+ 在```Vue 2.x```中，应用根容器的```outerHTML```将替换为根组件模板（如果根组件没有模板/渲染选项，则最终编译为模板）。```Vue 3.x ```现在使用应用程序容器的```innerHTML```。

**其他小改变**
+ 生命周期钩子函数的重命名：```beforeDestory ➡️ beforeUnmount```、```destoryed ➡️ unmounted```
+ 自定义指令API已更改为与组件生命周期一致
+ ```data```应始终声明为函数
+ 来自```mixin```的```data```选项现在可简单地合并
+ ```attribute```强制策略已更改
+ ```<template>``` 没有特殊指令的标记 (```v-if/else-if/else```、```v-for``` 或 ```v-slot```) 现在被视为普通元素，并将生成原生的 ```<template>``` 元素，而不是渲染其内部内容。

**移除API**
+ ```keyCode``` 支持作为 ```v-on``` 的修饰符
+ ```$on```，```$off``` 和 ```$once``` 实例方法
+ 过滤```filter```
+ 内联模板 ```attribute```
+ ```$destroy``` 实例方法。用户不应再手动管理单个 ```Vue``` 组件的生命周期。

