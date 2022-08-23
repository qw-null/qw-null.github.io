---
title: Vue2.0对于数组变化的监测
date: 2022-08-19 22:42:35
tags:
- 理解Vue
---
### 1.背景
我们都知道，Vue2.0对于响应式数据的实现存在一些不足：
+ 无法检测 数组/对象 的新增
+ 无法检测通过索引改变数组的操作

### 2.分析
1.<b style="background:yellow;">无法检测 数组/对象 的新增？</b>
 vue检测数据的变动是通过```Object.defineProperty```实现的，所以无法监听数组的添加操作是可以理解的，因为是在构造函数中就已经为所有属性做了这个检测绑定操作。

2.<b style="background:yellow;">无法检测通过索引改变数组的操作。即```vm.items[indexOfItem] = newValue```?</b>
官方文档中对于这两点都是简要的概括为“由于JavaScript的限制”无法实现，而```Object.defineProperty```是实现检测数据改变的方案，这个限制是指```Object.defineProperty```

### 3.思考
<mark>```vm.items[indexOfItem] = newValue```真的不能被监听么？</mark>

> Vue对数组的7个变异方法（push、pop、shift、unshift、splice、sort、reverse）实现了响应式。这里就不做测试。接下来测试一下通过索引改变数组的操作能不能被监听到。

```javascript
// 遍历数组，用Object.definedProperty对每一项进行监测
function defineReactive(data, key, value) {
     Object.defineProperty(data, key, {
         enumerable: true,
         configurable: true,
         get: function defineGet() {
             console.log(`get key: ${key} value: ${value}`)
             return value
         },
         set: function defineSet(newVal) {
             console.log(`set key: ${key} value: ${newVal}`)
             value = newVal
         }
     })
}

function observe(data) {
    Object.keys(data).forEach(function(key) {
        defineReactive(data, key, data[key])
    })
}

let arr = [1, 2, 3]
observe(arr)
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220822140627.png)

### 4.测试说明
通过索引改变```arr[1]```，我们发现触发了```set```，也就是```Object.definedProperty```是可以检测到通过索引改变数组的操作的，那么Vue2.0为什么没有实现呢？
> 看一下来自尤大大的回答：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220822145717.png)

**小结**
处于对性能原因的考虑，没有去实现它，而不是不能实现。

<mark>对于对象而言，每一次的数据变更都会对对对象的属性进行一次枚举，一般对象本身的属性数量有限，所以对于遍历枚举等方式产生的性能损耗可以忽略不计，但是对于数组而言呢？<b style="background:pink;">数组包含的元素是可能达到成千上万的，假设对于每一次数组元素的更新都触发枚举/遍历，其带来的性能的损耗将与获得的用户体验不成正比，故vue无法检测数组的变动。</b></mark>

不过```Vue3.0```用```proxy```代替了```definedProperty```之后解决了这个问题。

### 5.解决方案
#### 数组
1.```this.$set(array, index, data)```

```javascript
//这是个深度的修改，某些情况下可能导致你不希望的结果，因此最好还是慎用
this.dataArr = this.originArr
this.$set(this.dataArr, 0, {data: '修改第一个元素'})
console.log(this.dataArr) 
//同样的 源数组也会被修改 在某些情况下会导致你不希望的结果       
console.log(this.originArr)   
```

2.```splice```
因为```splice```会被监听有响应式，而```splice```又可以做到增删改。

3.利用临时变量进行中转
```javascript
let tempArr = [...this.targetArr]
tempArr[0] = {data: 'test'}
this.targetArr = tempArr
```
#### 对象
1.```this.$set(obj, key ,value)```
可实现增、改
2.```watch```时添加```deep：true```深度监听，只能监听到属性值的变化，新增、删除属性无法监听
```javascript
this.$watch('blog', this.getCatalog, {
    deep: true
    // immediate: true // 是否第一次触发
  });

```
3.```watch```时直接监听某个```key```
```javascript
watch: {
  'obj.name'(curVal, oldVal) {
    // TODO
  }
}
```

