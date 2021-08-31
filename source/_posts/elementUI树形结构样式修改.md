---
title: ElementUI树形结构样式修改
date: 2021-08-31 15:17:49
categories:
- CSS样式
tags:
- CSS
---

### 0.最终效果图
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210831152413.png)
> PS:基于[ElementUI中的树形控件进行实现](https://element.eleme.cn/#/zh-CN/component/tree)

原效果图：![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210831153551.png)

### 1.实现步骤
分为4各步骤：
+ 更换父子节点以及根节点的图标
+ 调整父子节点的缩进
+ 绘制结点间的关系线
+ 更换最前方的小三角形图标
  
#### 1.1 更换父子节点以及根节点的图标
实现思路：通过data.class的值来确定父子节点和根节点【树形结构的数据中要包含class字段】，分别为其加上不同的图标，图标使用<svg-icon>展示。
```html
<el-tree
    :data="data"
    :props="defaultProps"
    default-expand-all
    indent="0"
    class="tree-line"
    highlight-current
    :expand-on-click-node="true"
    @node-click="handleNodeClick"
>
    <span slot-scope="{ node, data }" class="custom-tree-node">
    <!-- 父级icon -->
    <span v-show="data.class === 0" class="iconfont" style="margin-right: 5px"> <svg-icon icon-class="danghui" style="color:#ff0000;font-size:10px;" /> </span>
    <!--非父节点非根节点icon-->
    <span v-show="data.class === 1" class="iconfont" style="margin-right: 5px"> <svg-icon icon-class="dangOrg" style="color:#ff0000;font-size:10px;" /> </span>
    <!-- 子集icon -->
    <span v-show="data.class === 2" class="iconfont" style="margin-right: 5px"><svg-icon icon-class="dangMember" style="font-size:10px;" /></span>
    <span>{{ node.label }}</span>
    </span>
</el-tree>
```
#### 1.2 调整父子节点的缩进
```css
.el-tree-node {
    position: relative;
    padding-left: 5px; /* 缩进量 */
  }
.el-tree-node__children {
    padding-left: 5px; /* 缩进量 */
  }
```
#### 1.3 绘制结点间的关系线
```css
/* 竖线 */
.el-tree-node::before {
    content: "";
    height: 100%;
    width: 1px;
    position: absolute;
    //left: -3px;
    top: -18px;
    border-width: 1px;
    border-left: 1px solid #D1CECE;
}
/* 当前层最后一个节点的竖线高度固定 */
.el-tree-node:last-child::before {
    height: 38px; // 可以自己调节到合适数值
}
/* 横线 */
.el-tree-node::after {
    position: absolute;
    content: " ";
    width: 20px;
    height: 20px;
    /* left: -3px; */
    top: 20px;
    border-width: 1px;
    border-top: 1px solid #D1CECE;
}
/* 去掉最顶层的虚线，放最下面样式才不会被上面的覆盖了 */
& > .el-tree-node::after {
    border-top: none;
}
& > .el-tree-node::before {
    border-left: none;
}
```
#### 1.4 更换最前方的小三角形图标
```css
/* 展开关闭的icon修改 */
  .el-tree-node__expand-icon.expanded {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  .el-icon-caret-right:before {
    content: "\e783";
    font-size: 15px;
    font-family:element-icons !important;
  }
  .el-tree-node__expand-icon.expanded.el-icon-caret-right:before {
    content: "\e781";
    font-size: 15px;
    font-family:element-icons !important;
  }
```
Q:content属性的值如何取到？
A:打开[ElementUI图标库](https://element.eleme.cn/#/zh-CN/component/icon),F12选中需要的图标，在样式中即可获取到。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210831160209.png)

### 2.完整代码
```html
<el-tree
    :data="data"
    :props="defaultProps"
    default-expand-all
    indent="0"
    class="tree-line"
    highlight-current
    :expand-on-click-node="true"
    @node-click="handleNodeClick"
>
    <span slot-scope="{ node, data }" class="custom-tree-node">
    <!-- 父级icon -->
    <span v-show="data.class === 0" class="iconfont" style="margin-right: 5px"> <svg-icon icon-class="danghui" style="color:#ff0000;font-size:10px;" /> </span>
    <!--非父节点非子节点-->
    <span v-show="data.class === 1" class="iconfont" style="margin-right: 5px"> <svg-icon icon-class="dangOrg" style="color:#ff0000;font-size:10px;" /> </span>
    <!-- 子集icon -->
    <span v-show="data.class === 2" class="iconfont" style="margin-right: 5px"><svg-icon icon-class="dangMember" style="font-size:10px;" /></span>
    <span>{{ node.label }}</span>
    </span>
</el-tree>
```
```javascript
data: [{
    // class:0 一级节点  class:1非父节点，非子节点     class:2子节点
    label: '一级 1',
    class: 0,
    children: [{
        label: '二级 1-1',
        class: 1,
        children: [{
        label: '三级 1-1-1',
        class: 1,
        children: [{
            label: '一级 2',
            class: 1,
            children: [{
            label: '二级 2-1',
            class: 1,
            children: [{
                    label: '三级 2-1-1',
                    class: 2
                }]
                }, 
                {
                label: '二级 2-2',
                class: 1,
                children: [{
                    label: '三级 2-2-1',
                    class: 2
                }]
            }]
        }]
        }]
    }]
    }, {
    label: '一级 3',
    class: 0,
    children: [{
            label: '二级 3-1',
            class: 1,
            children: [{
            label: '三级 3-1-1',
            class: 2
            }]
        }, 
        {
            label: '二级 3-2',
            class: 1,
            children: [{
            label: '三级 3-2-1',
            class: 2
            }]
        }]
    }],
    defaultProps: {
        children: 'children',
        label: 'label'
    }
```
```css
.tree-line{
  .el-tree-node {
    position: relative;
    padding-left: 5px; // 缩进量
  }
  .el-tree-node__children {
    padding-left: 5px; // 缩进量
  }
  // 竖线
  .el-tree-node::before {
    content: "";
    height: 100%;
    width: 1px;
    position: absolute;
    //left: -3px;
    top: -18px;
    border-width: 1px;
    border-left: 1px solid #D1CECE;
  }
  // 当前层最后一个节点的竖线高度固定
  .el-tree-node:last-child::before {
    height: 38px; // 可以自己调节到合适数值
  }
  // 横线
  .el-tree-node::after {
    position: absolute;
    content: " ";
    width: 20px;
    height: 20px;
    //left: -3px;
    top: 20px;
    border-width: 1px;
    border-top: 1px solid #D1CECE;
  }
  // 去掉最顶层的虚线，放最下面样式才不会被上面的覆盖了
  & > .el-tree-node::after {
    border-top: none;
  }
  & > .el-tree-node::before {
    border-left: none;
  }
  // 展开关闭的icon修改
  .el-tree-node__expand-icon.expanded {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  .el-icon-caret-right:before {
    content: "\e783";
    font-size: 15px;
    font-family:element-icons !important;
  }
  .el-tree-node__expand-icon.expanded.el-icon-caret-right:before {
    content: "\e781";
    font-size: 15px;
    font-family:element-icons !important;
  }
}
```


