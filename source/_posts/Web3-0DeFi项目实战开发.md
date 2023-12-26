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

## 比特币 - 第一个区块链的应用

比特币是区块链的一个应用。

## 以太坊

以太坊作为一种基于区块链技术的去中心化应用平台，与比特币仅支持货币交易不同，可以在以太坊上开发和运行去中心化的应用程序。以太坊背后的核心思想是开发人员可以在分布式网络中创建和运行程序代码，而不受中心化控制。以太坊使用以太币（Ether，代码为 ETH）作为交易货币。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312201005448.png)

以太币干什么用？
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312201008445.png)

以太坊-区块链钱包：
钱包里存储的是数字资产的账户信息和密钥信息，而不是实际资产。
热钱包（HotWallet）、冷钱包（Cold Wallet），其中冷钱包是脱离网络连接的离线钱包，热钱包需要联网才能够使用。

钱包的概念：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312201029737.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312201032861.png)
一组助记词可以产生很多账号

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312201116899.png)

## 智能合约

什么是智能合约？
智能合约是一种自动执行合约的协议，其中的条款和条件以计算机代码的形式编写。它们运行在区块链技术之上，如以太坊。智能合约允许在没有第三方的情况下进行可靠的交易和协议执行。

智能合约的主要特点包括：

1. **自动执行**：一旦设定条件被满足，合约就会自动执行。
2. **透明和不可篡改**：所有的交易和操作都被记录在区块链上，任何人都可以查看，但无法更改。
3. **安全性**：智能合约的代码一经部署，就无法更改，确保了合约的可靠性和一致性。
4. **去中心化**：智能合约运行在区块链上，没有中央管理者，使得合约的执行更为公正和透明。

智能合约可以应用于许多场景，例如资金转移、资产交易、供应链管理、投票系统等。它们的出现为许多传统的合约和交易提供了新的、更高效的解决方案。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312201126869.png)

# 项目开发

## web3 连接到以太坊网络（测试网、主网）

### 1.什么是 web3

web3 是以太坊官方提供的一个连接以太坊区块链的模块，允许使用 HTTP 或 IPC 与本地或远程以太坊节点进行交互，它包含以太坊生态系统几乎所有的功能。web3 模块主要连接以太坊暴露出来的 RPC 层。开发者利用 web3 连接 RPC 层，可以连接任何暴露了 RPC 接口的节点，从而与区块链交互。web3 是一个集合库，支持多种开发语言使用 web3，其中的 JavaScript API 叫做 web3.js，另外还有 web3.py，web3j。

web3.eth：用于与以太坊区块链和智能合约之间的交互。

web3.utils：包含一些辅助方法。

web3.shh：用于协议进行通信的 P2P 和广播。

web3.bzz：用于与群网络交互的 Bzz 模块。

web3.js 开发文档：https://web3js.readthedocs.io/en/v1.8.1/

web3.js 中文文档 : https://learnblockchain.cn/docs/web3.js/

### 2.实例化 web3

web3 要与以坊节点进行交互，需要创建一个 web3 对象。

```javascript
var Web3 = require("web3");
// "Web3.providers.givenProvider" will be set if in an Ethereum supported browser.
var web3 = new Web3(
  Web3.givenProvider || "ws://some.local-or-remote.node:8546"
);
```

根据 API 可知需要指定节点地址，我们将`ws://some.local-or-remote.node:8546`换成其它连接到以太坊网络的节点的地址，以此来确定连接的以太坊的网络。
那么连接到以太坊网络的节点的地址是多少呢？这里我们需要使用到 infura。

### 3.获取连接到以太坊网络的节点地址

infura 提供公开的 Ethereum 主网和测试网络节点，到 infura.io 网站注册后即可获取各个网络的地址。请按照如下步骤获取地址。

第一步：打开 infura 网站地址：https://infura.io/dashboard，使用邮箱注册后登陆

第二步：点击上图标记的“create new project”按钮创建一个新项目。然后弹出如下弹框，在输入框输入项目名，如”MyEtherWallet“，然后点击“create project”按钮创建。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312251507913.png)

第三步：然后会显示如下界面，点击下图中的选择框，可以看到提供主网、Kovan测试网络、Ropsten测试网络、GoerLi测试网络的节点地址。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312251510130.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312251511763.png)

第四步：选择GoerLi测试网络，然后复制地址
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312251512187.png)

连接到以太坊GoerLi测试网络
现在将复制的地址替换掉实例化web对象的地址，如下
``` javascript
var Web3 = require("web3")
var web3 = new Web3(Web3.givenProvider || 'wss://goerli.infura.io/ws/v3/cb7e63cf28244e4499b4b6fb6162e746');
console.log("Web3:", web3)
```
连接到以太坊主网与GoerLi测试网络一样的，只需复制主网节点的地址去实例化web3即可。由于在主网上交易需要花费gas，因此我们基于GoerLi测试网络进行开发，后续开发完成后可再切换到主网。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312251515346.png)

## web3.js 高频API

### 1.账号创建
```javascript
web3.eth.accounts.create([entropy]);
```
参数：

entropy - String (可选): 它是一个可选项，是一个随机字符串，将作为解锁账号的密码。如果没有传递字符串，则使用random生成随机字符串。

返回值：

Object：包含以下字段的一个帐户对象：

address- string：帐户地址。

privateKey- string：帐户私钥。前端永远不应该在localstorage中以未加密的方式共享或存储！

signTransaction(tx [, callback])- Function：签名交易的方法。

sign(data)- Function：签名二进制交易的方法。

示例：
```javascript
  // 创建账号
  const account = web3.eth.accounts.create("123");
  console.log('account=',account);
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312251549225.png)

### 2.获取地址 
使用API `web3.eth.accounts.create()`创建了新账户后生成了一个账户对象，在该对象中拥有`address`属性，即账户的私钥。
```javascript
  // 获取地址
  let addr = account.address;
  console.log('address = ',addr);
```
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312251610782.png)

### 3.获取私钥
使用API `web3.eth.accounts.create()`创建了新账户后生成了一个账户对象，在该对象中拥有`privateKey`属性，即账户的私钥。

```javascript
  //获取私钥
  let privateKey = account.privateKey;
  console.log('privateKey = ',privateKey);
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312251613169.png)

### 4.余额获取
根据地址获取以wei为单位余额

```javascript
  // 获取余额
  web3.eth.getBalance(addr).then((res) => {
    console.log(res)
});
```

