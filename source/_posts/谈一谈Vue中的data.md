---
title: 谈一谈Vue中的data
date: 2022-08-04 23:17:42
categories:
- Vue学习
tags:
- Vue
---
**问题：为什么data属性是一个函数而不是一个对象？**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220805200207.png)

### 1. 实例和组件定义data的区别
vue实例中定义data属性既可以是一个对象也可以是一个函数
```javascript
const app = new Vue({
  el:'#app',
  // 对象格式
  data:{
    foo:'foo'
  },
  // 函数格式
  data(){
    return{
      foo:'foo'
    }
  }
})
```
<b style="background-color:yellow;font-weight:800;">组件中定义data属性，只能是一个函数。</b>

如果为组件data直接定义为一个对象
```javascript
Vue.component('component1',{
  template:`<div>组件</div>`,
  data:{
    foo:'foo'
  }
})
```
会得到警告信息：![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220804234701.png)

### 2.组件data定义函数与对象的区别
上面提到组件中的```data```必须是一个函数，这是为什么？
我们在定义好一个组件之后，```vue```最终都会通过```vue.extend()```构成组件实例

我们来模仿一下组件构造函数，定义data属性，采用**对象**的形式
```javascript
function Component(){
}
Component.prototype.data = {
  count: 0
}

//创建两个组件实例
const componentA = new Component()
const componentB = new Component()
```
当我们修改```componentA```组件```data```属性的值，```componentB```中的值也发生了改变
```javascript
console.log(componentB.data.count); // 0
componentA.data.count = 1;
console.log(componentB.data.count); // 1
```
产生这样的原因是两者共用了同一个内存地址，```componentA```修改的内容，同样对```componentB```产生了影响。

如果我们采用函数的形式，则不会出现这种情况（**函数返回的对象内存地址并不相同**）
```javascript
function Component(){
  this.data = this.data()
}
Component.prototype.data = function(){
  return {
    count:0
  }
}
```
此时修改```componentA```组件```data```属性的值，```componentB```中的值不受影响

vue组件可能会有很多个实例，采用函数返回一个全新的data形式，使得每个实例对象的数据不会受到其他实例对象数据的污染

### 3.原理分析
首先从```vue```初始化```data```代码中，可以看出```data```的定义可以是函数也可以是对象

```javascript
function initData(vm:Component){
  let data = vm.$options.data;
  data = vm._data = typeof data === 'function'
  ? getData(data,vm)
  : data||{}
  ...
}
```
既然源码在初始化```data```时既能是```object```又能是```function```，为什么在将```data```声明为对象时会报警告呢？
组件在创建的时候，会进行选项的合并。在源码中自定义组件会进入```mergeoptions```进行选项合并
```javascript
Vue.prototype._init = function(options?:Object){
  ...
  // merge options
  if(options && options._isComponent){
    // optimize internal component instantiation（优化内部组件实例化）
    // since dynamic options merging is pretty slow, and none of the 
    // internal component options needs special treatment
    //（因为动态选项合并很慢，而且没有内部组件选项需要的特殊处理。）
    initInternalComponent(vm,options)
  }else{
    vm.$options = mergeOptions(
      resolveConstructorOptions(vm.constructor),
      options || {},
      vm
    )
  }
  ...
}
```
定义```data```会进行数据校验，这时```vm```实例为```undefined```，进入```if```判断，若```data```类型不是```function```，则会出现警告提示
```javascript
strats.data = function (
  parentVal: any,
  childVal: any,
  vm?: Component
): ?Function {
  if (!vm) {
    if (childVal && typeof childVal !== "function") {
      process.env.NODE_ENV !== "production" &&
        warn(
          'The "data" option should be a function ' +
            "that returns a per-instance value in component " +
            "definitions.",
          vm
        );

      return parentVal;
    }
    return mergeDataOrFn(parentVal, childVal);
  }
  return mergeDataOrFn(parentVal, childVal, vm);
};
```

### 4.结论
+ 根实例对象```data```可以是对象也可以是函数（根实例时单例），不会产生数据污染的情况
+ 组件实例对象的```data```必须为函数，目的是为了防止多个组件实例对象之间共用一个```data```，产生数据污染。采用函数的形式，```initData```时会将其作为工厂函数都会返回全新的```data```对象格式

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220805204613.png)
