---
title: 青训营-如何写好JS
date: 2022-02-11 11:43:22
categories:
- 字节青训营
tags:
- JavaScript
---
## 01 各司其责
JavaScript -- Behavioral
CSS -- Presentational
HTML -- Structual

##### 例子 夜间模式的实现
1. 版本一

```javascript
const btn = document.getElementById('modeBtn');
btn.addEventListener('click', (e) => {
  if (e.target.innerHTML === '太阳模式') {
    body.style.backgroundcolor = 'black';
    body.style.color = 'white';
    e.target.innerHTML = '月亮模式';
  } else {
    body.style.backgroundcolor = 'white';
    body.style.color = 'black';
    e.target.innerHTML = '太阳模式';
  }
})
```
实现功能没有问题，但是存在 使用JS操作CSS应该实现的功能 的问题。

2. 版本二

```javascript
const btn = document.getElementById('modeBtn');
btn.addEventListener('click', (e) => {
  const body = document.body;
  if (body.className !== 'night') {
    body.className = 'night';
  } else {
    body.className = '';
  }
})
```

版本二的优点是通过JS控制状态的变化，具体的样式则交由CSS来实现。

3. 版本三
```html
<input id="modeCheckBox" type="checkbox">
···
<label id="modeBtn" for="modeCheckBox"></label>
···
```
```css
#modeCheckBox{
  display:none;
}

#modeCheckBox:checked + .content{
  background-color: black;
  color: white;
  transition: all 1s;
}
```
要实现的功能仅为纯展示功能，因此可以通过HTML+CSS来实现即可。

<b>小结：</b>
+ HTML/CSS/JS 各司其责
+ 避免不必要的由JS直接操作样式
+ 可以通过class来表示状态
+ 纯展示类交互寻求零JS方案

自己动手实现：

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/录制_2022_02_11_17_11_54_118.gif)

[在线效果](https://codepen.io/qw-null/pen/mdqwJxM)

```html
<h1>ChangeMode</h1>
<input id="modeCheckBox" type="checkbox">

<label id="modeBtn" for="modeCheckBox"></label>
```

```css
body{
  margin: 0;
  padding: 0;
  display:flex;
  justify-content:center;
  align-items:center;
  height:100vh;
}

#modeCheckBox:checked + .content{
  background-color: black;
  color: white;
  transition: all 1s;
}

#modeCheckBox{
  display:none;
}

#modeBtn{
  margin-left:20px;
  cursor:pointer;
}

#modeBtn::after {
  content: '🌞';
}

#modeCheckBox:checked + #modeBtn::after {
  content: '🌜';
}

.dark{
  background-color: black;
  color: white;
  transition: all 1s;
}

.bright{
  background-color: white;
  color: black;
  transition: all 1s;
}
```

```javascript
const changeMode = () => {
  const modeCheck = document.getElementById("modeCheckBox");
  modeCheck.addEventListener('click',()=>{
    if (modeCheck.checked) {
      document.body.className = 'dark';
    }else{
      document.body.className = 'bright';
    }
  })
}

changeMode();
```

实现时通过js控制body的类名来改变效果。

🌞与🌜的切换是通过```input + label```的方式实现：

input选中时显示🌜，未选中显示🌞。

*之前写过的一个项目的手机版的下拉菜单图标的展示也是通过这种方式进行切换。*


## 02 组件封装

组件是指Web页面上抽出来一个个包含模板（HTML）、功能（JS）和样式（CSS）的单元。

好的组件具备封装性、正确性、拓展性、复用性。

(有些难)
## 03 过程抽象

用来处理局部细节控制的一些方法

函数式编程思想的基础应用

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220211203005.png)

什么是过程抽象呢？举个例子，比如你有间小房子，房子有门、有窗，这里门和窗也就是数据。那么你可以开门，也可以开窗，开门和开窗就属于过程。我们不仅仅可以抽象数据，还可以抽象这个过程。

##### 例子 操作次数限制
+ 一些异步交互
+ 一次性的HTTP请求

```html
<ul>
    <li>Task1：学习 html</li>
    <li>Task2：学习 css</li>
    <li>Task3：学习 JavaScript</li>
</ul>

<script>
const ulElem = document.querySelector('ul');
const liElems = ulElem.querySelectorAll('li');

liElems.forEach(liElem => {
    liElem.addEventListener('click', e => {
        e.target.className = 'completed';
        setTimeout(() => {
            ulElem.removeChild(e.target);
        }, 2000);
    });
});
</script>
```
上述代码实现的效果是完成一个任务之后，点击后将其删除，但是需要点击2s之后才能完成删除，如果在2s之内再去点击 已经点击了要删除的任务，就会报错。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220211204137.png)
报错的原因实际上就是点击了已经删除的数据项，已经不存在，所以也就无法删除。

因此，解决方法也很简单了，只要限制只能执行一次删除造作就好了。因此可以封装一个高级函数```once```，这个函数的目标就是保证删除操作只能执行一次，确保操作安全。

```javascript
function once(fn){
  return function(...args){
    if(fn){
      const res = fn.apply(this,args);
      fn = null;
      return res;
    }
  }
}

liElems.forEach(liElem => {
    liElem.addEventListener('click', once((e) => {
        e.target.className = 'completed';
        setTimeout(() => {
            ulElem.removeChild(e.target);
        }, 2000);
    }));
});
```

上述代码中的高级函数```once```实际上就是为了让函数只执行一次。为了能够让“只执行一次”的需求覆盖不同的事件处理，我们可以将这个需求剥离出来。这个过程称为<b style="color:red;">过程抽象</b>。

#### 高阶函数
+ 以函数作为参数
+ 以函数作为返回值
+ 常用于作为函数修饰器

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220211205700.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220211205725.png)

<b>函数节流</b>

概念：限制一个函数在一定时间内只能执行一次。

> 举个栗子，坐火车或地铁，过安检的时候，在一定时间（例如10秒）内，只允许一个乘客通过安检入口，以配合安检人员完成安检工作。上例中，每10秒内，仅允许一位乘客通过，分析可知，“函数节流”的要点在于，在 一定时间 之内，限制 一个动作 只 执行一次 。

<B>Q: 为什么需要函数节流？</B>
前端开发过程中，有一些函数或者事件会在多时间内被多次触发，例如 ```onresize```、```scroll```、```mousehover```等，这些事件的触发频率很高，如果在这些函数内部执行了其他函数，尤其是DOM操作的函数，会严重浪费计算机的资源，降低程序速度，甚至造成浏览器卡死、崩溃。因此需要限制执行次数。

```javascript
// 定时器方案
function throttle(fn, delay = 200) {
    let timer = null;
    return function () {
        if (!timer) {
            timer = setTimeout(() => {
                fn.apply(this, arguments);
                timer = null;
            }, delay);
        }
    }
}
```

函数节流的使用场景：
1. 懒加载、滚动加载、加载更多或监听滚动条位置；
2. 百度搜索框，搜索联想功能；
3. 防止高频点击提交，防止表单重复提交；


<b>函数防抖</b>
概念：函数防抖（debounce），就是指触发事件后，在 n 秒内函数只能执行一次，如果触发事件后在 n 秒内又触发了事件，则会重新计算函数延执行时间。

> 举个栗子，坐电梯的时候，如果电梯检测到有人进来（触发事件），就会多等待 10 秒，此时如果又有人进来（10秒之内重复触发事件），那么电梯就会再多等待 10 秒。在上述例子中，电梯在检测到有人进入 10 秒钟之后，才会关闭电梯门开始运行，因此，“函数防抖”的关键在于，在 一个事件 发生 一定时间 之后，才执行 特定动作。

<B>Q: 为什么需要函数防抖？</B>
前端开发过程中，有一些函数或者事件会在多时间内被多次触发，例如 ```onresize```、```scroll```、```mousehover```等，这些事件的触发频率很高，如果在这些函数内部执行了其他函数，尤其是DOM操作的函数，会严重浪费计算机的资源，降低程序速度，甚至造成浏览器卡死、崩溃。因此需要限制执行次数。

```javascript
function debounce(fn,wait){
    var timer = null;
    return function(){
        if(timer){
            clearTimeout(timer);
        }
        timer = setTimeout(() => {
        fn.apply(this, arguments)
      }, wait)
    }
}
```

函数防抖的使用场景：
1. 搜索框搜索输入。只需用户最后一次输入完，再发送请求；
2. 用户名、手机号、邮箱输入验证；
3. 浏览器窗口大小改变后，只需窗口调整完后，再执行 resize 事件中的代码，防止重复渲染。

<b>iterative</b>

```javascript
// 检验是否可迭代
const isIterable = obj => obj !== null
    && typeof obj[Symbol.iterator] === 'function';

function iterative(fn) {
    return function (subject, ...rest) {
        if (isIterable(subject)) {
            // 把所有的执行结果存储于 result 中并返回
            const result = [];
            for (const obj of subject) {
                result.push(fn.apply(this, [obj, ...rest]));
            }
            return result;
        }
        return fn.apply(this, [subject, ...rest]);
    }
}
```

#### 纯函数 & 非纯函数

<b>纯函数：</b>输入值确定，则输出值就会确定，不会因为执行次序，执行环境等的改变而改变

```javascript
function add(x,y){
  return x+y;
}
```
上述计算两个数之和的方法，不论在什么时候，两个加数确定则最终结果就会确定。

<b>非纯函数：</b>执行顺序，执行环境不同会造成最终呈现的结果不同。

```html
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
</ul>

<script>
    function setColor(elems, color) {
        for (const elem of elems) {
            elem.style.color = color;
        }
    }

    const els1 = document.querySelectorAll('li:nth-child(2n+1)');
    const els2 = document.querySelectorAll('li:nth-child(3n+1)');
    setColor(els2, 'blue');
    setColor(els1, 'red');
</script>
```
上面的代码中，两个 ```setColor``` 的调用顺序如果更换，执行的结果会不一样，所以它的结果会受外部环境的影响，是非纯函数。

使用高阶函数，可以减少系统里面非纯函数的数量，从而使得系统的稳定性和可靠性加强。

比如说，我们现在需要实现两个函数，一个是 ```setColor(elem, color)```，一次改变一个元素的颜色，还有一个 ```setColors(elems, color)```，一次改变多个元素的颜色。显然这两个函数都是非纯函数，但是我们可以选择直接定义：

```const setColors = iterative(setColor);```

这样就减少了一个非纯函数，有利于我们进行单元测试。

#### 命令式 & 声明式

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220212225631.png)

命令式代码更加强调执行的过程，强调怎么做。

<hr>

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220212225909.png)

声明式代码强调是什么，而不是过程。
