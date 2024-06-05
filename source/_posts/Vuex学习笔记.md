---
title: Vuex 学习笔记
date: 2021-06-01
categories:
- Vue学习
tags: 
- vue
---

## 1.Vuex概述
### 1.1 Vue组件之间共享数据
+ 父向子传值：v-bind 属性绑定
+ 子向父传值：v-on  事件绑定
+ 兄弟组件之间共享数据：EventBus
  
  > $emit  发送数据的那个组件
  > $on  接收数据的那个组件

== 以上方式适用于小范围数据共享 == 

*详情请见：【后续补充】*

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210608155748.png" style="zoom:120%;" />

### 1.2 如何使用Vue UI创建项目？

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/1.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/2.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/3.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/4.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/5.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/6.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/7.png)

创建完成之后，到电脑目录下就可以查看到创建的项目，使用vscode打开即可

### 1.3 Vuex的使用
-----

Vuex 是实现组件全局状态（数据）管理的一种机制，可以方便实现组件之间的数据共享，适用于大范围的数据共享。

-----

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/clipboard1.png" style="zoom:120%;" />

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/clipboard3.png)

### 1.4 Vuex知识点
Vuex中的核心概念
+ State
+ Mutation
+ Action
+ Getter

##### 1.4.1 State
提供唯一的公共数据源，所有共享的数据都要统一放在Store的State中进行存储
```javascript
const store = new Vuex.Store({
  state:{
    count: 0
  }
})

```
+ 组件访问state中数据的第一种方式：  
```javascript
this.$store.state.全局数据名称
```

+ 组件访问state中数据的第二种方式：
 ```javascript
// 1.从Vuex中按需导入mapState函数
import { mapState } from 'vuex'

//通过刚才导入的mapState函数，将当前组件需要的全局数据映射为当前组件的computed计算属性
//2.将全局数据映射为当前组件的计算属性
computed:{
  ...mapState(['count'])
  // ...表示展开运算符，意思是将全局数据映射为当前组件的计算属性
}
 ```
Vuex中不允许组件直接修改Store中的数据

##### 1.4.2 Mutation
用于变更Store中的数据，<b>mutation 必须是同步函数</b>
①只能通过Mutation变更store数据，不可以直接操作store中的数据
②通过这种方式虽然操作起来繁琐一些，但是可以集中监控所有数据的变化
```javascript
const store = new Vuex.Store({
  state:{
    count: 0
  },
  mutations:{
    add(state){
      //变更状态
      state.count++
    }
  }
})

```
+ 组件中触发mutation的第一种方式：  
```javascript
methods:{
  handle(){
    this.$store.commit('add')
  }
}
```
+ 组件触发mutation的第二种方式：
```javascript
// 1.从vuex中按需导入mapMutations函数
import { mapMutations } from 'vuex'
// 通过刚才导入的mapMutations函数，按需要的mutations函数映射为组件的methods方法
methods:{
  ...mapMutations(['add'])
  // 将 `this.add()` 映射为 `this.$store.commit('add')`
}
```

<b>可以在触发mutations时传递参数</b>

```javascript
const store = new Vuex.Store({
state:{
    count: 0
  },
  mutations:{
    addN(state,step){
      //变更状态      
      state.count += step
    }
  }
})
```

```javascript
  methods:{
    handle(){
        this.$store.commit('add',3)
    }
  }

另外一种方法：

  methods:{
    ...mapMutations(['addN'])
  }
```
上述第二种方法，使用时采用如下方法传递参数：
```html
 <button @click="addN(5)">+5</button>
```


##### 1.4.3 Action
用于处理异步任务
如果通过异步操作变更数据，必须通过Action,而不能使用Mutation,但是在Actions中还是要通过出发Mutation的方式间接变更数据
```javascript
const store = new Vuex.Store({
  state:{
    count: 0
  },
  mutations:{
    add(state){
      //变更状态
      state.count++
    }
  },
  actions:{
    addAsync(context){
      setTimeout(()=>{
        //通过commit去触发mutation函数
        context.commit('add')
      },1000)
    }
  }
})

```
+ 组件中触发action的第一种方式：  
```javascript
methods:{
  handle(){
    this.$store.dispatch('addAsync')
  }
}
```
+ 组件中触发action的第二种方式：
```javascript
// 1.从vuex中按需导入mapActions函数
import { mapActions } from 'vuex'
// 通过刚才导入的mapActions函数，按需要的acyions函数映射为组件的methods方法
methods:{
  ...mapActions(['addASync'])
}
```

与1.4.2Mutation中相同，也可以携带参数
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210608161404.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210608161437.png)

##### 1.4.4 Getter
Getter用于对Store中的数据进行加工处理形成新的数据.（*Getter不会修改State中的数据，只起到包装的作用*）

特点：
> 1.Getter可以对Store中已有的数据加工处理之后形成新的数据，类似Vue的计算属性
>
>2.Store中的数据发生改变，Getter的数据也会跟着变化

```javascript
// 定义 Getter
const store = new Vuex.Store({
	state:{
		count: 0
	},
    getters:{
        showNum: state => {
            return '当前最新的数量是【'+ state.count +'】'
        }
    }
})
```

+ 使用getters的第一种方式：

```javascript
this.$store.getters.名称
//例如：this.$store.getters.showNum (this.可以省略)
```

+ 使用getters的第二种方法
```javascript
import { mapGetters }from 'vuex'

computed: {
	...mapGetter(['showNum'])
}
```

## 2.Vuex小Demo
Demo效果图：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210618145323.png)

Demo业务流程图：
<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/todolist.png"  />

Demo地址：
[VuexToDoListDemo](https://github.com/qw-null/VuexToDoListDemo)
