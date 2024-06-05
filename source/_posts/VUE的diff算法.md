---
title: 说一说Vue的diff算法
date: 2022-07-25 10:24:02
tags:
- Vue
---

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725102930.png)

## 1.Vue的diff算法是什么？
diff算法是一种通过同层的树节点比较的高效算法。其有两个特点：
+ 比较只会在同层级进行，不会跨层级比较
+ 在diff比较的过程中，循环从两边向中间比较

diff算法在很多场景下都有应用，在vue中，作用于虚拟dom渲染成真实dom的新旧VNode节点比较


## 2.Vue的diff算法的比较方式是什么？
diff算法的整体策略为：<span style="color:red;font-weight:800;">深度优先，同层比较。</span>

1. 只会在同层级进行，不会跨层级比较
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725104449.png)
2. 在比较过程中，循环会从两边向中间收拢
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725104537.png)
---
下面举个vue通过diff算法更新的例子：
新旧节点如下图所示：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725104923.png)
第一次循环之后发现旧节点D（old - endIndex）和新节点D（new - startIndex）相同，直接复用旧节点D作为diff后的第一个真实节点，同时旧节点的endIndex往前移动到C，新节点的startIndex移动到C
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725105449.png)
第二次循环，发现此时旧节点的endIndex对应的节点C与新节点的startIndex对应的节点C相同，故此进行和第一次循环相同的工作
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725105802.png)
第三次循环发现新节点E未出现在旧节点中，只能直接创建真实节点E插入到C节点之后，然后将新节点的startIndex移动到下一个节点A上
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725110338.png)
第四次循环发现旧节点的startIndex对应的节点值A和新节点startIndex对应的节点值A相同，故将A节点移动到真实节点E之后。此时同时将旧节点的startIndex后移一位，新节点的startIndex后移一位
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725110821.png)
第五次循环发现旧节点的startIndex对应的节点B与新节点startIndex对应的节点B相同，所以将节点B移动到真实节点A之后，同时新旧节点的startIndex都后移一位
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725111112.png)
第六次循环发现旧节点的startIndex的值大于endIndex值，所以就需要创建新节点中所有startIndex到endIndex的节点，放在真实节点B之后
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725111412.png)
## 3.Vue的diff算法的原理分析
在Vue中当数据发生变化时，set方法会调用Dep.notify通知所有订阅者Watcher，订阅者就会调用patch给真实DOM打补丁，更新相应的视图

```javascript
function patch(oldVnode, vnode, hydrating, removeOnly) {
    if (isUndef(vnode)) { // 没有新节点，直接执行destory钩子函数
        if (isDef(oldVnode)) invokeDestroyHook(oldVnode)
        return
    }

    let isInitialPatch = false
    const insertedVnodeQueue = []

    if (isUndef(oldVnode)) {
        isInitialPatch = true
        createElm(vnode, insertedVnodeQueue) // 没有旧节点，直接用新节点生成dom元素
    } else {
        const isRealElement = isDef(oldVnode.nodeType)
        if (!isRealElement && sameVnode(oldVnode, vnode)) {
            // 判断旧节点和新节点自身一样，一致执行patchVnode
            patchVnode(oldVnode, vnode, insertedVnodeQueue, null, null, removeOnly)
        } else {
            // 否则直接销毁及旧节点，根据新节点生成dom元素
            if (isRealElement) {

                if (oldVnode.nodeType === 1 && oldVnode.hasAttribute(SSR_ATTR)) {
                    oldVnode.removeAttribute(SSR_ATTR)
                    hydrating = true
                }
                if (isTrue(hydrating)) {
                    if (hydrate(oldVnode, vnode, insertedVnodeQueue)) {
                        invokeInsertHook(vnode, insertedVnodeQueue, true)
                        return oldVnode
                    }
                }
                oldVnode = emptyNodeAt(oldVnode)
            }
            return vnode.elm
        }
    }
}

```
patch函数前两个参数为oldVNode和VNode，分别代表新的节点和之前的旧节点，函数中主要做了四个判断：
1️⃣ 没有新节点，直接触发旧节点的destory钩子
2️⃣ 没有旧节点，说明是页面刚开始初始化的时候，此时，根本不需要比较，直接调用createElm全部新建
3️⃣ 旧节点和新节点一样：通过sameVnode判断节点是否一样，一样直接调用patchVnode去处理这两个节点
4️⃣ 旧节点和新节点自身不一样：当两个节点不一样时，直接创建新节点，删除旧节点

接下来，谈一谈patchVnode部分
```javascript
function patchVnode (oldVnode, vnode, insertedVnodeQueue, removeOnly) {
    // 如果新旧节点一致，什么都不做
    if (oldVnode === vnode) {
      return
    }

    // 让vnode.el引用到现在的真实dom，当el修改时，vnode.el会同步变化
    const elm = vnode.elm = oldVnode.elm

    // 异步占位符
    if (isTrue(oldVnode.isAsyncPlaceholder)) {
      if (isDef(vnode.asyncFactory.resolved)) {
        hydrate(oldVnode.elm, vnode, insertedVnodeQueue)
      } else {
        vnode.isAsyncPlaceholder = true
      }
      return
    }
    // 如果新旧都是静态节点，并且具有相同的key
    // 当vnode是克隆节点或是v-once指令控制的节点时，只需要把oldVnode.elm和oldVnode.child都复制到vnode上
    // 也不用再有其他操作
    if (isTrue(vnode.isStatic) &&
      isTrue(oldVnode.isStatic) &&
      vnode.key === oldVnode.key &&
      (isTrue(vnode.isCloned) || isTrue(vnode.isOnce))
    ) {
      vnode.componentInstance = oldVnode.componentInstance
      return
    }

    let i
    const data = vnode.data
    if (isDef(data) && isDef(i = data.hook) && isDef(i = i.prepatch)) {
      i(oldVnode, vnode)
    }

    const oldCh = oldVnode.children
    const ch = vnode.children
    if (isDef(data) && isPatchable(vnode)) {
      for (i = 0; i < cbs.update.length; ++i) cbs.update[i](oldVnode, vnode)
      if (isDef(i = data.hook) && isDef(i = i.update)) i(oldVnode, vnode)
    }
    // 如果vnode不是文本节点或者注释节点
    if (isUndef(vnode.text)) {
      // 并且都有子节点
      if (isDef(oldCh) && isDef(ch)) {
        // 并且子节点不完全一致，则调用updateChildren
        if (oldCh !== ch) updateChildren(elm, oldCh, ch, insertedVnodeQueue, removeOnly)

        // 如果只有新的vnode有子节点
      } else if (isDef(ch)) {
        if (isDef(oldVnode.text)) nodeOps.setTextContent(elm, '')
        // elm已经引用了老的dom节点，在老的dom节点上添加子节点
        addVnodes(elm, null, ch, 0, ch.length - 1, insertedVnodeQueue)

        // 如果新vnode没有子节点，而vnode有子节点，直接删除老的oldCh
      } else if (isDef(oldCh)) {
        removeVnodes(elm, oldCh, 0, oldCh.length - 1)

        // 如果老节点是文本节点
      } else if (isDef(oldVnode.text)) {
        nodeOps.setTextContent(elm, '')
      }

      // 如果新vnode和老vnode是文本节点或注释节点
      // 但是vnode.text != oldVnode.text时，只需要更新vnode.elm的文本内容就可以
    } else if (oldVnode.text !== vnode.text) {
      nodeOps.setTextContent(elm, vnode.text)
    }
    if (isDef(data)) {
      if (isDef(i = data.hook) && isDef(i = i.postpatch)) i(oldVnode, vnode)
    }
  }

```
patchVnode主要做以下4个判断：
1️⃣ 新节点是否为文本节点，如果是，直接更新dom的文本内容为新节点的文本内容
2️⃣ 新节点和旧节点如果都有子节点，则调用updateChildren函数处理比较更新子节点
3️⃣ 只有新节点有子节点，旧节点没有，不需要比较，所有子节点都是全新的，直接全部新建（创建出新的DOM，添加进老节点里作为子节点）
4️⃣ 只有旧节点有子节点而新节点没有子节点，要做的就是将所有的旧节点的子节点全部删除

## 小结
+ 当Vue中的数据发生变化时，set方法会调用Dep.notify通知所有订阅者Watcher，订阅者会调用patch函数给真实DOM打补丁
+ 在patch函数中通过isSameVnode方法进行判断新旧节点是否相同，相同则调用patchVnode方法
+ patchVnode方法主要做了以下操作：
+ + 找到对应的真实dom，称为el
+ + 如果新节点均为文本节点，且只有文本内容不同，直接更新旧节点的文本内容即可
+ + 如果旧节点有子节点而新节点没有子节点，将旧节点的所有子节点删除就OK了
+ + 如果新节点有子节点而旧节点没有子节点，则创建新节点的子节点添加到旧节点里作为子节点
+ + 新旧节点都有子节点，调用updateChildren函数比较子节点  
+ updateChildren函数主要做以下操作：
+ + 设置新旧VNode的头尾指针
+ + 新旧头尾指针进行比较，循环向中间靠拢，根据情况调用patchVnode进行patch工作、调用createElm创建新节点等

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725125732.png)


