---
title: JWT认证登录小Demo
date: 2023-11-10 09:51:22
categories:
  - SpringBoot
tags:
  - Java
---

## Token 认证

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311100955887.png)

**Token 认证流程**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311100956226.png)

**Token 认证的特点**

基于 token 的用户认证是一种服务端无状态的认证方式，服务端不用存放 token 数据。

用解析 token 的时间换取 session 的存储空间，从而减轻服务器的压力，减少频繁的查询数据库。

token 完全由应用管理，所以它可以避开同源策略。

## JWT

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311100959862.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101000448.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101000897.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101000499.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101000886.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101001022.png)

**JWT 特点**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101001776.png)

**JWT 实现**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101002221.png)

生成 Token
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101002562.png)

解析 Token
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101003112.png)

## 具体实现

**项目结构**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101034576.png)

**User.java**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101034810.png)

**JwtUtils.java**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101035218.png)

**Result.java**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101036558.png)

**ResultCode.java**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101036759.png)

**UserController.java**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101037480.png)

## 接口测试结果

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311101037067.png)

🌟 [github 仓库](https://github.com/qw-null/loginDemo)
