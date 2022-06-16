---
title: Vue面经二三事
date: 2022-06-15 00:00:40
tags: 
- 面经
---

## 1.v-if和v-for哪个优先级更高？

回答：
1. 在```Vue2```中```v-for```的优先级高于```v-if```，在```Vue3```中刚好相反，```v-for```的优先级低于```v-if```。

2. 文档中明确指出<b style='color:red'>永远不要把```v-for```和```v-if```同时用在同一个元素上</b>。二者同时使用时，从输出的渲染函数可以看出会先执行循环再判断条件，在循环的过程中多次对DOM元素进行添加或删除，这样会造成大量性能的消耗。

3. 实际开发过程中对```v-for="user in users" v-if="user.isActive"```通常存在两种解决方法：
①将```v-if```的判断过程移到外部计算中，先对数据进行计算筛选出要展示的数据。
②此时把 ```v-if``` 移动至容器元素上 (比如 ```ul```、```ol```)即可。

在```Vue2```和```Vue3```中对语句
```Html
<div v-for="item in items" :key="item.id" v-if="item.isActive">
  {{ item.name }}
</div>
```
的渲染函数如下：
```javascript
// Vue2
ƒ anonymous(
) {
with(this){return _c('div',{attrs:{"id":"app"}},_l((items),function(item){return (item.isActive)?_c('div',{key:item.id},[_v("\n      "+_s(item.name)+"\n    ")]):_e()}),0)}
}

//Vue3
(function anonymous(
) {
const _Vue = Vue

return function render(_ctx, _cache) {
  with (_ctx) {
    const { renderList: _renderList, Fragment: _Fragment, openBlock: _openBlock, createElementBlock: _createElementBlock, toDisplayString: _toDisplayString, createCommentVNode: _createCommentVNode } = _Vue

    return shouldShowUsers
      ? (_openBlock(true), _createElementBlock(_Fragment, { key: 0 }, _renderList(items, (item) => {
          return (_openBlock(), _createElementBlock("div", { key: item.id }, _toDisplayString(item.name), 1 /* TEXT */))
        }), 128 /* KEYED_FRAGMENT */))
      : _createCommentVNode("v-if", true)
  }
}
})
```


## 2. v-for中key的作用是什么？
回答：
1. key的作用主要是为了更高效的更新虚拟DOM。

2. vue在patch过程中<b style='color:red'>判断两个节点是否是相同节点key是一个必要条件</b>。从源码中可以知道，vue判断两个节点是否相同时主要判断两者的key和元素类型等，因此如果不设置key，它的值就是undefined，则可能永远认为这是两个相同节点，只能去做更新操作，这造成了大量的dom更新操作，明显是不可取的。

3. 实际使用中在渲染一组列表时key必须设置，并且必须是唯一标识，应该避免使用index作为key值，使用index作为key值可能会导致一些隐藏的bug；在vue中使用相同元素标签过渡切换时，也会使用key属性，其目的是为了让vue可以区分它们，否则vue只会替换其内部属性而不会触发过渡效果。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220615095710.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220615095750.png)

## 3.双向数据绑定以及它的实现原理？
回答：
1. vue中双向数据绑定是通过指令```v-model```实现的，可以绑定一个动态数值到视图，同时视图中的变化也能改变这个数值。```v-model```是语法糖，默认情况下相当于```:value和@input```。
2. 通常在表单项上使用```v-model```，原生的表单项可以直接使用```v-model```，自定义组件上如果要使用需要在组建内绑定value并且处理输入事件。
3. 输出包含```v-model```模板的渲染函数，<b style='color:red'>发现它会被转换为value属性的绑定以及一个事件监听，事件回调函数中会做相应的变量更新操作</b>。
4. 使用```v-model```可以减少大量繁琐的事件处理代码，提高开发效率，代码可读性也更好。

后续追问：
1.v-model和sync修饰符有什么区别？
2.
