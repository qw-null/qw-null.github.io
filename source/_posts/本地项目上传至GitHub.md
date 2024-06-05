---
title: 本地项目上传至GitHub
date: 2021-06-08 19:58:18
categories:
- GitHub使用
tags:
- GitHub
---

当你遭受巨大痛苦时，你要懂得自己忍受，尽量不用你的痛苦去搅扰别人。

## 0.需求说明
本地创建项目，github上未存在该项目的repository，现在要将该项目上传至github。

## 1.操作步骤

#### 1.1 GitHub中新建仓库

_注意：GitHub中新建的仓库名称需要与本地的项目文件夹的名称一致_

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210609103848.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20210609104142.png)

#### 1.2本地项目文件夹下运行命令
点击“Git Bash Here”，打开git命令行。
> 运行命令:
```bash
git init

git add .

git commit -m 'initproject'(git commit -m '提交信息说明')

git remote add origin 自己的项目地址

git push -u origin master
```

此时便已将本地项目上传至github