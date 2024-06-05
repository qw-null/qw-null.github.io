---
title: How-to-use-Hexo
date: 2021-06-02 15:45:32
categories:
- 博客
tags: 
- 博客
- hexo
---

## 如何使用搭建的博客？
### 1 新建博客
项目目录下运行命令
```bash
hexo new title
```
> tips:其中title为文章的标题名称

### 2 如何进行分类【category  & tag】
#### 2.1 创建分类选项
+ 项目目录下执行命令
  ```bash
  hexo new page categories
  ```
+ 成功后会提示信息
  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210602155850.png)
+ 根据路径信息找到改文件，并打开修改
  ```
  ---
  title: 文章分类
  date: 2021-06-02 15:14:47
  type: "categories"
  ---
  ```
+ 保存关闭文件

#### 2.2 创建标签选项
+ 项目目录下执行命令
  ```bash
  hexo new page tags
  ```
+ 成功后会提示信息
  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210602160228.png)
+ 根据路径信息找到改文件，并打开修改
  ```
  ---
  title: 标签
  date: 2021-06-02 15:19:33
  type: "tags"
  ---
  ```
+ 保存关闭文件

#### 2.3 为文章添加分类与标签
在文章内容中进行修改
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210602161206.png)

> 重新编译之后运行即可查看页面出现分类和标签
> ```bash
> hexo clean && hexo g && hexo s
> ```
> ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210602161127.png)
**注意：** hexo一篇文章只能属于一个分类，也就是说如果在“- web前端”下方添加“-xxx”，hexo不会产生两个分类，而是把分类嵌套（即该文章属于 “- web前端”下的 “-xxx ”分类）。

未完待续......