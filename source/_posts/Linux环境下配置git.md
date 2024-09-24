---
layout: post
title: Linux环境下配置git
date: 2024-09-24 14:25:01
tags:
- Git
---
总共分为两个步骤：安装 + 配置，最终实现通过Git拉取代码。

## 1.安装Git 
```bash
yum install git

## 查看版本
git --version
```
得到返回信息 "git version XX.XX.XX.XX" 表明Git已经安装成功。

## 2.配置Git
分为两个步骤：初始化Git并生成授权证书 + 代码仓库（github）配置

### 2.1 初始化Git
```bash
git config --global user.name "your name"
git config --global user.email "your email"
```
### 2.2 生成授权证书
```bash
ssh-keygen -t rsa -C "your email"
```
执行该命令后，会有3次确认：
第一次：生成公钥和私钥的文件名称(默认即可)
第二次：生成证书的密码(默认为空即可)
第三次：确认证书密码(默认为空即可)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202409241447794.png)

### 2.3查看生成公钥
```bash
cd ~/.ssh
ls
cat id_rsa.pub
```
执行`ls`命令后，会看到两个文件`id_rsa.pub`,`id_rsa`，其中`id_rsa.pub`就是生成的公钥。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202409241452475.png)

### 2.4 配置代码仓库
登录github，在用户设置中，找到SSH and GPG keys，点击New SSH key，将生成的公钥复制到Key中，然后点击Add SSH key即可。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202409241453045.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202409241454415.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202409241455156.png)

运行命令`ssh git@github.com`, 出现以下信息则表示配置成功。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202409241459846.png)