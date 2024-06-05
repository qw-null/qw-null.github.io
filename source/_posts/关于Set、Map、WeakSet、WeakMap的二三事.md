---
title: 关于Set、Map、WeakSet、WeakMap的二三事
date: 2022-04-25 14:29:45
categories:
- JavaScript
tags:
- JavaScript
---

## 1.Set

特征：
1. 成员不能重复
2. 只有键值，没有键名，有点类似于数组
3. 可以遍历

``` console.log(Set.prototype) ```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220425144545.png)

对于```Set```应该明确的几点：

<b>⭐ 不重复</b>
在js中判断元素相等的方法有两种：```===```和 ```Object.is```

```javascript
NaN === NaN  // false
Object.is(NaN , NaN) // true

+0 === -0 // true
Object.is(+0 , -0) // false
```
我们不妨来看一下，```Set```遵循哪种原则呢？

```javascript
var set = new Set();
set.add(1);
set.add(NaN);
set.add(NaN);
set.add(0);
set.add(-0);
console.log(set);
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220425145751.png)

可以看到，在```Set```中，认为```NaN```是相等的，认为```+0```和```-0```也是相等的，所以才会说```Set```中成员是<b style="color:red;">不重复的。</b>

<b>⭐ 数组 ⇌ Set</b>

数组转换为```Set```：``` new Set([1,2,3]) ```

```Set```转换为数组：``` [...set] ```

## 2.WeakSet

特征：
1. 成员都是对象（```Object```或者继承自```Object```的类型），尝试其他值会抛出```TypeError```
2. WeakSet是弱引用，随时可以消失（不计入垃圾回收机制）。可以用来保存DOM节点，不容易造成内存泄漏
3. 不能遍历

``` console.log(WeakSet.prototype) ```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220425151232.png)

对于```WeakSet```需要明确几点：

<b>⭐ 何为 强引用 & 弱引用？</b>

对于一个全局变量``` var key = {} ```，JS垃圾回收器不能自动将其回收，若想要释放内存，则需让 ```key = null```。

※ 强引用

```javascript
let cat = { name: "Kitty" };
const pets = [cat];

cat = null;
console.log(pets); // [{ name: "Kitty" }]
```
通过将变量 ``` cat ``` 创建为对象，并把这个对象放入一个数组 ```pets``` 中，然后通过将它的值设置为 ```null``` 来删除其对原始对象的引用。

尽管我们再也无法访问 ```cat``` 变量，但由于在 ```pets``` 数组和这个对象之间存在<b>强引用关系</b>，因此这个对象其实仍保留在内存中，并且可以通过 ```pets[0]``` 访问到它。 换句话说，<b>强引用可以防止垃圾回收从内存中删除对象。</b>

※ 弱引用

弱引用是对对象的引用，如果它还是对内存中对象的唯一引用，就能顺利地进行垃圾回收。相反，一般强引用都会防止垃圾回收。

```javascript
let pets = new WeakMap();
let cat = { name: "Kitty" };
pets.set(cat, "Kitty");
console.log(pets); // WeakMap {{…} => 'Kitty'}
cat = null;

// 等待垃圾回收后
console.log(pets); // WeakMap{}
```
通过利用 ```WeakMap``` 及其附带的弱引用，我们可以看到两种类型的引用之间的差异。虽然对原始 ```cat``` 对象的强引用仍然存在，但 ```cat``` 对象仍然存在于 ```WeakMap``` 中，我们可以毫无问题地访问它。

但是，当我们通过将 ```cat```变量重新赋值 ```null``` 来覆盖对原始 ```cat``` 对象的引用时，由于内存中对原始对象的唯一引用是来自我们创建的 ```WeakMap``` 的弱引用，所以它不会阻止垃圾回收的发生。这意味着当 ```JavaScript``` 引擎再次运行垃圾回收过程时，```cat``` 对象将从内存和我们分配给它的 ```WeakMap``` 中删除。

<b>因此这里的关键区就别在于</b>，强引用可以防止对象进行垃圾回收，而弱引用则不会。

默认情况下，<b style="color:red;">JavaScript 对其所有引用使用强引用，使用弱引用的唯一方法是使用 WeakMap 或 WeakSet。</b>

## 3.Map

特征：
1. 本质上是键值对
2. 可以遍历

Q：```Object```与```Map```的区别？
- 键的类型：```Object```的key值必须是```String```、```Number```或者```Symbol```，```Map```的键可以是JavaScript支持的所有类型。
- 元素顺序：```Map``` 元素的顺序遵循插入的顺序，而 ```Object``` 的则没有这一特性。
- 继承：```Map``` 继承自 ```Object``` 对象。

## 4.WeakMap
