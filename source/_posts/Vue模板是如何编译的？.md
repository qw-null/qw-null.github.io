---
title: Vue模板是如何编译的？
date: 2022-07-26 14:02:18
tags:
- Vue
---
### 认识模板编译
在Vue文件中使用```<template></template> ```表示模板，其内部包裹的代码并不是原生的HTML，因此浏览器是不认识模板的。所以我们要做的工作就是把```<template></template>```内部的代码编译成浏览器认识的原生HTML，这就是模板编译。

页面从```<template></template>```包裹的代码到视图最终展示的主要流程是
1. 提取出模板中的原生HTML和非原生HTML（比如绑定的属性、事件、指令等）
2. 经过一些处理生成render函数
3. render函数再将模板内容生成对应的VNode
4. 再经过patch过程（Diff）得到要渲染到视图的VNode
5. 最后根据VNode创建真实DOM节点，也就是原生的HTML插入到视图中，完成渲染

上述的1，2，3步骤就是模板编译的过程。
那代码究竟在步骤2中发生了什么？它是怎么编译，最终生成render函数的呢？

### 模板编译详解 - 源码
#### baseCompile()
这个就是模板编译的入口函数，接收两个参数：1.```template```:就是要转换的模板字符串；2.```options```:就是转换时需要的参数

编译的流程主要有三步：
1. 模板解析：通过正则等方式提取出```<template></template>```里的标签元素、属性、变量等信息，并解析成抽象语法树AST
2. 优化：遍历AST找出其中的静态节点和静态根节点，并进行标记
3. 代码生成：根据AST生成渲染函数render

上述三个步骤分别对应三个函数。

首先查看```baseCompile()```函数在哪里调用，即**模板编译从那里开始**。
源码地址：```src/complier/index.js - 11行```

```javascript
export const createCompiler = createCompilerCreator(function baseCompile (
  template: string, // 就是要转换的模板字符串
  options: CompilerOptions //就是转换时需要的参数
): CompiledResult {
  // 1. 进行模板解析，并将结果保存为 AST
  const ast = parse(template.trim(), options)

  // 没有禁用静态优化的话
  if (options.optimize !== false) {
    // 2. 就遍历 AST，并找出静态节点并标记
    optimize(ast, options)
  }
  // 3. 生成渲染函数
  const code = generate(ast, options)
  return {
    ast,
    render: code.render, // 返回渲染函数 render
    staticRenderFns: code.staticRenderFns
  }
})
```
--- 
上述的3个步骤在源码中均体现出来，接下来看一下具体的编译流程是啥样的。
以以下模板为例：
```html
<template>
    <div id="app">{{name}}</div>
</template>
```
打印编译后的结果：
```json 
{
  ast: {
    type: 1,
    tag: 'div',
    attrsList: [ { name: 'id', value: 'app' } ],
    attrsMap: { id: 'app' },
    rawAttrsMap: {},
    parent: undefined,
    children: [
      {
        type: 2,
        expression: '_s(name)',
        tokens: [ { '@binding': 'name' } ],
        text: '{{name}}',
        static: false
      }
    ],
    plain: false,
    attrs: [ { name: 'id', value: '"app"', dynamic: undefined } ],
    static: false,
    staticRoot: false
  },
  render: `with(this){return _c('div',{attrs:{"id":"app"}},[_v(_s(name))])}`,
  staticRenderFns: [],
  errors: [],
  tips: []
}
```
生成的内容看起来有些吃力，实际上注意以下三个步骤都做了什么就行了：
+ ast字段是第一步生成的
+ static字段，就是标记，是在第二步中根据ast里的type加上去的
+ render字段，就是第三步生成的
--- 

再来看源码。
#### 1.parse()
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220726105550.png)

实现的功能是 通过正则等方法提取出来```<template></template>```模板字符串里所有的tag、props、children信息，生成一个对应结构的ast对象 。

parse()接收两个参数：
+ template：就是要转换的模板字符串
+ options：就是转换时需要的参数。它包含有四个钩子函数，就是用来把parseHTML解析出来的字符串提取出来，并生成对应的AST

核心步骤如下：
调用```parseHTML```函数对模板字符串进行解析：
+ 解析到开始标签、结束标签、文本、注释分别进行不同的处理
+ 解析过程中遇到文本信息就调用文本解析器```parseText```函数进行文本解析
+ 解析过程中遇到包含过滤器，就调用过滤器解析器```parseFilters```函数进行解析

每一步解析的结果都合并到一个对象上（就是最后的AST）

```javascript
// parse()的部分源码
export function parse (
  template: string, // 要转换的模板字符串
  options: CompilerOptions // 转换时需要的参数
): ASTElement | void {
  parseHTML(template, {
    warn,
    expectHTML: options.expectHTML,
    isUnaryTag: options.isUnaryTag,
    canBeLeftOpenTag: options.canBeLeftOpenTag,
    shouldDecodeNewlines: options.shouldDecodeNewlines,
    shouldDecodeNewlinesForHref: options.shouldDecodeNewlinesForHref,
    shouldKeepComment: options.comments,
    outputSourceRange: options.outputSourceRange,
    // 解析到开始标签时调用，如 <div>
    start (tag, attrs, unary, start, end) {
        // unary 是否是自闭合标签，如 <img />
        ...
    },
    // 解析到结束标签时调用，如 </div>
    end (tag, start, end) {
        ...
    },
    // 解析到文本时调用
    chars (text: string, start: number, end: number) {
      // 这里会判断判断很多东西，来看它是不是带变量的动态文本
      // 然后创建动态文本或静态文本对应的 AST 节点
      ...
    },
    // 解析到注释时调用
    comment (text: string, start, end) {
      // 注释是这么找的
      const comment = /^<!\--/
      if (comment.test(html)) {
      // 如果是注释，就继续找 '-->'
      const commentEnd = html.indexOf('-->')
      ...
    }
  })
  // 返回的这个就是 AST
  return root
}
```
上面解析文本时调用```chars()```会根据不同类型的节点加上不同type，来标记AST节点类型，这个属性在下一步标记的时候也会用的

|  type   | AST 节点类型  |
|  ----  | ----  |
| 1  | 元素节点 |
| 2  | 包含变量的动态文本节点 |
| 3  | 没有变量的纯文本节点 |

#### 2.optimize()
这个函数就是在AST里找出静态节点和静态根节点，并添加标记，为了后面patch过程中，直接跳过静态节点的比对，直接克隆一份过去，从而优化patch的性能。

optimize()的大致过程：
+ 标记静态节点（markStatic），就是判断type是上面哪种类型（type值的1，2，3）
+ + type的值为1：就是包含子元素的节点，设置static为false，并递归标记子节点，直到标记完所有的子节点
+ + type的值为2：设置static为false
+ + type的值为3: 就是不包含子节点和动态属性的纯文本节点，把它的static = true，patch的时候就会跳过这个，直接克隆一份

+ 标记静态根节点（markStaticRoots），这里的原理和标记静态节点基本相同，知识需要满足下面条件的节点才算是静态根节点
+ + 节点本身必须是静态节点
+ + 必须有子节点
+ + 子节点不能只有一个文本节点

```javascript
export function optimize (root: ?ASTElement, options: CompilerOptions) {
  if (!root) return
  isStaticKey = genStaticKeysCached(options.staticKeys || '')
  isPlatformReservedTag = options.isReservedTag || no
  // 标记静态节点
  markStatic(root)
  // 标记静态根节点
  markStaticRoots(root, false)
}
```
#### 3.generate()
这个就是生成render的函数，就是说最终会返回下面这个东东
```javascript
// 比如有这么个模板
<template>
    <div id="app">{{name}}</div>
</template>

// 上面模板编译后返回的 render 字段 就是这样的
render: `with(this){return _c('div',{attrs:{"id":"app"}},[_v(_s(name))])}`
```
可以看出上面的render正是虚拟DOM的结构，就是把一个标签分为tag、props、children。
```javascript
// 把内容格式化一下，容易理解一点
with(this){
  return _c(
    'div',
    { attrs:{"id":"app"} },
    [  _v(_s(name))  ]
  )
}
```
---
其中```with```是用来欺骗词法作用域的，它可以让我们更快的饮用一个对象上的多个属性
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220726130648.png)

函数内部的```_c```、```_v```、```_s```分别表示什么含义？

```javascript
// 其实不止这几个，由于本文例子中没有用到就没都复制过来占位了
export function installRenderHelpers (target: any) {
  target._s = toString // 转字符串函数
  target._l = renderList // 生成列表函数
  target._v = createTextVNode // 创建文本节点函数
  target._e = createEmptyVNode // 创建空节点函数
}
// 补充
_c = createElement // 创建虚拟节点函数
```
---
继续看generator()的源码

```javascript
export function generate (
  ast: ASTElement | void,
  options: CompilerOptions
): CodegenResult {
  const state = new CodegenState(options)
  // 就是先判断 AST 是不是为空，不为空就根据 AST 创建 vnode，否则就创建一个空div的 vnode
  const code = ast ? (ast.tag === 'script' ? 'null' : genElement(ast, state)) : '_c("div")'

  return {
    render: `with(this){return ${code}}`,
    staticRenderFns: state.staticRenderFns
  }
}
```
这个流程很简单，就是判断AST是不是为空，不为空就根据AST创建VNode，否则就创建一个空的div的VNode。
创建VNode主要是通过```genElement()```函数来实现的。

#### 4.genElement()
这个函数的逻辑很清晰，通过```if/else```判断传进来的AST元素节点的属性，来执行不同的生成函数。

```javascript
export function genElement (el: ASTElement, state: CodegenState): string {
  if (el.parent) {
    el.pre = el.pre || el.parent.pre
  }

  if (el.staticRoot && !el.staticProcessed) {
    return genStatic(el, state)
  } else if (el.once && !el.onceProcessed) { // v-once
    return genOnce(el, state)
  } else if (el.for && !el.forProcessed) { // v-for
    return genFor(el, state)
  } else if (el.if && !el.ifProcessed) { // v-if
    return genIf(el, state)

    // template 节点 && 没有插槽 && 没有 pre 标签
  } else if (el.tag === 'template' && !el.slotTarget && !state.pre) {
    return genChildren(el, state) || 'void 0'
  } else if (el.tag === 'slot') { // v-slot
    return genSlot(el, state)
  } else {
    // component or element
    let code
    // 如果有子组件
    if (el.component) {
      code = genComponent(el.component, el, state)
    } else {
      let data
      // 获取元素属性 props
      if (!el.plain || (el.pre && state.maybeComponent(el))) {
        data = genData(el, state)
      }
      // 获取元素子节点
      const children = el.inlineTemplate ? null : genChildren(el, state, true)
      code = `_c('${el.tag}'${
        data ? `,${data}` : '' // data
      }${
        children ? `,${children}` : '' // children
      })`
    }
    // module transforms
    for (let i = 0; i < state.transforms.length; i++) {
      code = state.transforms[i](el, code)
    }
    // 返回上面作为 with 作用域执行的内容
    return code
  }
}
```
> 在这里还可以发现v-for的优先级高于v-if

### render 函数
在Vue项目的```main.js```文件中，存在以下代码：![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220725164129.png)
调用render函数会得到传入的模板（```.vue```文件）对应的虚拟DOM，那么这个render函数是从哪里来的？它是如何将```.vue```文件转换成浏览器可识别的代码的呢？

render函数的来源有两种方式：
+ 第一种就是经过模板编译生成render函数
+ 第二种是我们自己定义在组件里的render函数，这种会跳过模板编译的过程

#### 自定义的render
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220726131746.png)
上面三种情况最后编译出的内容完全一样。
那么，你一定会有一个疑问，既然模板能自己编译自动生成，为什么还要提出自定义render？
原因有二：
1. 自己把VNode写了，就会直接跳过模板编译，不用再经历模板编译过程中解析模板动态属性、事件、指令等，所以性能上会有所提升。这一点在下面的渲染优先级上有所体现。
2. 还有一些情况，能让我们的代码写法更加灵活，更加方便简洁，不会冗余

*（在Element-UI的组件源码中就有大量的重写render函数）*

##### 1.渲染优先级
Vue的生命周期中关于模板编译的部分，如下
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220726142359.png)
从图中可以知道，如果有```template```，就不会管```el```，所以**template比el的优先级更高**。

那我们自己手写的render函数呢？
```html
<div id='app'>
    <p>{{ name }}</p>
</div>
<script>
    new Vue({
        el:'#app',
        data:{ name:'沐华' },
        template:'<div>掘金</div>',
        render(h){
            return h('div', {}, '好好学习，天天向上')
        }
    })
</script>
```
结果在页面上渲染出来的只有```<div>好好学习，天天向上</div>```。因此，**render函数的优先级更高**。
所以，综上所述：<span style="background-color:yellow; font-weight:800;">优先级：render函数 > template > outerHTML </span>
因为不管是```el```挂载的```outerHTML```，还是```template```，最后都会被编译成```render```函数，而如果已经有了```render```函数，就跳过前面的编译。这一点在源码中也有体现。

```javascript
 Vue.prototype.$mount = function ( el, hydrating ) {
    el = el && query(el);
    var options = this.$options;
    // 如果没有 render 
    if (!options.render) {
      var template = options.template;
      // 再判断，如果有 template
      if (template) {
        if (typeof template === 'string') {
          if (template.charAt(0) === '#') {
            template = idToTemplate(template);
          }
        } else if (template.nodeType) {
          template = template.innerHTML;
        } else {
          return this
        }
      // 再判断，如果有 el
      } else if (el) {
        template = getOuterHTML(el);
      }
    }
    return mount.call(this, el, hydrating)
  };
```

##### 2.更灵活的写法
以如下代码为例：
```javascript
<template>
    <h1 v-if="level === 1">
      <a href="xxx">
        <slot></slot>
      </a>
    </h1>
    <h2 v-else-if="level === 2">
      <a href="xxx">
        <slot></slot>
      </a>
    </h2>
    <h3 v-else-if="level === 3">
      <a href="xxx">
        <slot></slot>
      </a>
    </h3>
</template>
<script>
  export default {
    props:['level']
  }
</script>
```
我们可以换一种方式，写出和上面编译效果一样的代码
```javascript
<script>
  export default {
    props:['level'],
    render(h){
      return h('h' + this.level, this.$slots.default())
    }
  }
</script>
```
或者下面这样，多次调用的时候就很方便
```javascript
<script>
  export default {
    props:['level'],
    render(h){
      const tag = 'h' + this.level
      return (<tag>{this.$slots.default()}</tag>)
    }
  }
</script>
```

## 小结

![何为模板编译？](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220726154204.png)

![模板编译详解](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220913150808.png)





