---
title: Web3.0 DeFi项目实战开发
date: 2023-12-19 14:41:02
tags:
---

[视频地址](https://www.bilibili.com/video/BV14A411178x/?share_source=copy_web&vd_source=f3ff8a2761a3e07339584c4852cdd504)

# Web3.0 简介

- web1.0 时代：只能读，不能写，没有互动功能方法
- web2.0 时代：可读可写，内容由全体网民产生，中心化（数据储存在服务器、易产生垄断）
- web3.0 时代：去中心化（最大的特点）、web3.0 的基础是区块链技术（区块链技术是元宇宙等时髦概念的底层架构和逻辑基础）、数据掌握在生产者手中。

web3.0 能够实现去中心化得益于区块链催生的智能合约技术，它不仅可以记录信息，还可以运行应用程序。
去中心化的应用 - DAPP

区块链支撑的 web3.0
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312191534592.png)

# 元宇宙

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312191510341.png)

# NFT

NFT（Non-fungible token）非同质化代币、不可互换的代币
数字藏品

# 区块链

[区块链 Demo 地址](https://github.com/anders94/blockchain-demo)
**定义**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312191538680.png)

**特征**
去中心化、共识机制、不可篡改
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312191543547.png)

**应用场景**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312191606112.png)

**类型**
公链、私链、联盟链、混合链
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312191607904.png)

**hash 算法**

- 从 hash 值不可以反向推导出原始数据
- 输入数据的微小变化会得到完全不同的 hash 值
- 相同的数据会得到相同的值
- 执行效率高效，长的文本也能很快地计算出哈希值
- hash 算法的冲突概率很小

**区块的概念**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312191623512.png)

第 n 块的 Hash 值 = Hash(第 n-1 块的 Hash 值+第 n 块的账本数据)

**P2P 网络**

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312191630511.png)

**记账与挖矿**

为什么会有人愿意花费精力来记账？
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312191634252.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312191635640.png)

以谁记录的账本为准？
比特币采取的是工作量证明法（POW），也就是让记账的人去解一道运算量很大的数学题，谁能够最先解出来，就用谁的账本，同时谁也就获得了比特币的奖励，这个解数学题的过程就叫做挖矿，所以挖矿比拼的就是矿机 CPU 的运算能力。

**共识机制**
广泛应用的共识机制：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312191641728.png)
