---
title: React学习笔记
date: 2023-04-21 11:12:06
tags:
  - React
---

[视频链接](https://www.bilibili.com/video/BV1bS4y1b7NV/?share_source=copy_web&vd_source=f3ff8a2761a3e07339584c4852cdd504)

课程代码：链接: https://pan.baidu.com/s/16hEN7j4hLDpd7NoFiS8dHw?pwd=4gxv 提取码: 4gxv 复制这段内容后打开百度网盘手机 App，操作更方便哦。

[课程大纲介绍](https://www.lilichao.com/index.php/2022/05/10/react%e8%a7%86%e9%a2%91%e6%95%99%e7%a8%8b%ef%bc%88alpha%e7%89%88%ef%bc%89/)

# React简介

网页是B/S架构，传统网页中用户每点击一次链接就会加载一个新的页面，加载全部内容。但是许多情况只需要局部刷新就可以满足需求，例如购物网站加入购物车的操作，此时网页不需要全部刷新，只需要局部发生变化。 

AJAX+DOM使得局部刷新成为可能，但是DOM操作仍然很麻烦，且存在兼容性问题。DOM操作本身十分占用系统资源，DOM的API十分繁复，使得各种操作十分的不优雅。

React是一个用于构建用户界面的JavaScript库。
React特点：
+ 虚拟DOM
+ 声明式
+ 基于组件
+ 支持服务器端渲染
+ 快速、简单、易学

## 1.HelloWorld
React.createElement()
- 用来创建一个React元素
- 参数：1.元素名（组件名），例如：`div`、`span`等； 2.元素中的属性，例如：`id`、`class`、`style`等 / ① 在设置事件时，属性名需要修改驼峰命名法 ② class属性需要使用className来设置； 3.元素的子元素（内容）

ReactDOM.createRoot()用来创建React根元素，需要一个DOM元素作为参数.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
</head>
<body>
<div id="root"></div>
<div id="button"></div>

<!--引入react的核心库-->
<!--https://unpkg.com/react@18.2.0/umd/react.development.js-->
<script src="script/react.development.js"></script>
<!--引入react-dom的核心库-->
<!--https://unpkg.com/react-dom@18.2.0/umd/react-dom.development.js-->
<script src="script/react-dom.development.js"></script>
<script>
    // 通过react向页面中添加div
    const div = React.createElement('div',{},'我是React创建的div');
    // console.log(div)

    // 获取跟元素对应的React元素
    // ReactDOM.createRoot()用来创建React根元素，需要一个DOM元素作为参数
    const root = ReactDOM.createRoot(document.getElementById('root'));

    //将div渲染到根元素
    root.render(div);

    const btn = React.createElement('button',{
      id:'btn',
      type:'button',
      className:'hello_btn',
      onClick:()=>{alert('点击了按钮')}
    },'点击一下');
    const btnRoot = ReactDOM.createRoot(document.getElementById("button"));
    btnRoot.render(btn);

</script>

</body>
</html>
```

⚠️ 注意：
1. React元素最总会将虚拟DOM准换为真实DOM
2. React元素一旦创建就无法修改，只能通过新创建的元素进行替换
3. 当重复调用render()方法时，React会采用差分算法进行高效更新

## 2.JSX
命令式编程
`const button = React.createElement('button',{},'我是按钮');`

声明式编程 - 结果导向编程
在React中可以通过JSX（JS拓展）来创建React元素，JSX需要被翻译为JS代码，才能被React执行

在React中使用JSX，必须引进Babel来完成翻译工作
> JSX 就是 React.createElement()的语法糖
> JSX在执行之前都会被babel转换为js代码

### 2.1 JSX的补充
使用React需要在页面中引入三个库 `react.js` `react-dom.js` `babel.js`（三者载入顺序不能变）

**JSX注意事项：**
1. JSX不是字符串，不要加引号
2. JSX中的html标签应该小写，React组件应该大写开头
3. JSX中有且只有一个根标签，只能使用一个div包裹
4. JSX的标签必须正确结束 （自结束标签必须写/）
5. 在JSX中可以用{}嵌入表达式
6. 如果表达式是空值，布尔值，undefined这些值，将不会显示
7. 在JSX中，属性可以直接在标签中显示
- 注意：
- lass需要使用className代替
- style需要使用对象设置

```JSX
    const name = "孙悟空";
    function fn(){
      return 'hello';
    }

    const div = <div 
      id="box" 
      onClick={() => { alert("HaHa") }}
      style={{backgroundColor:'yellow',border:'5px red solid'}}>
      我是一个div
      <input type="text" />
      {name}<br/>
      {fn()}
      </div>;

      const root = ReactDOM.createRoot(document.getElementById("root"));
      root.render(div)
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202403211011932.png)
### 2.2 列表渲染
{}只能用来放js表达式，而不能放语句（if for）

在语句中是可以去操作JSX
```JSX
let div;
if(lang==='en'){
  div=<div>hello {name}</div>
}else if(lang==='cn'){
  div = <div>你好 {name}</div>
}
```

```JSX
const arr = ['aa','bb','cc']
// JSX 会自动将数组中的元素在页面中显示
const list = <div>{arr}</div>
```
结果是：aabbcc

```JSX
const data=[];

//遍历 arr
for(let i=0;i<arr.length;i++){
   data.push(<li>{arr[i]}</li>);
 }
---------------
const data = arr.map(item=><li>{item}</li>)
const list = <ul>{data}</ul>

---------------
const list = {arr.map(item=> <li>{item}</li>)}
```
## 3.创建React项目
> 约定优于配置





