---
title: Web3.0 DeFi项目实战开发
date: 2023-12-19 14:41:02
category:
- 区块链
tags:
- 区块链
- web3
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

第一步：打开 [infura 网站地址](https://infura.io/dashboard)，使用邮箱注册后登陆。

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

### 5.单位转换

```javascript
// 1.Eth 转为 wei
const num1 = web3.utils.toWei('0.3', 'ether');
console.log('num1=',num1);
// 300000000000000000

// 2. wei 转为Eth
const balance = Web3.utils.fromWei("300000", "ether");
// balance= 0.0000000000003
```
---
### 6.Eth转账[⚠️存在问题]
```javascript
web3.eth.sendSignedTransaction(signedTransactionData [, callback])
```

**参数**

- `signedTransactionData`-`String`：以HEX格式签名的交易数据。

  交易数据对象可以包含如下字段：

  - `from`- `String|Number`：发送帐户的地址。如果未指定，则使用web3.eth.defaultAccount属性。或web3.eth.accounts.wallet中本地钱包的地址。
  - `to`- `String`:(可选）消息的目标地址，若未定义则为合同发送消息。
  - `value`- `Number|String|BN|BigNumber`:(可选）为wei中的交易转移的数量，如果是合约发送消息，则是捐赠给合约地址。
  - `gas` - `Number`:(可选，默认：待定）用于交易的gas（未使用的gas会退还）。
  - `gasPrice`- `Number|String|BN|BigNumber`:(可选）此交易的gas价格，以wei为单位，默认为[web3.eth.gasPrice](https://web3js.readthedocs.io/en/1.0/web3-eth.html#eth-gasprice)。
  - `data`- `String`:(可选）包含合同上函数调用数据的[ABI字节字符串](http://solidity.readthedocs.io/en/latest/abi-spec.html)。
  - `nonce`- `Number`:(可选）随机数的整数。

- `callback`-`Function`：（可选）可选回调，将错误对象作为第一个参数返回，结果作为第二个参数返回。

**返回**

`PromiEvent`：promise组合的事件，将在交易完成时调用。包含以下事件

- `"transactionHash"`返回`String`：在发送事务并且事务哈希可用之后立即触发。
- `"receipt"`返回`Object`：在交易确认时触发。
- `"confirmation"`返回`Number`，`Object`：每次确认都会被调用，直到第12次确认。接收确认编号作为第一个参数，将数据作为第二个参数。
- `"error"`返回`Error`：如果在发送过程中发生错误，则会触发。

1. 构建转账参数

   区块链转账和支付宝转账类似，需要 `发送方` 、`接收方`、`金额`、`密码`

   另外需要添加部分区块链参数：`矿工费gas`、`地址转账交易次数`

   ```javascript
        // 获取账户交易次数
         let nonce = await web3.eth.getTransactionCount(fromaddress);
         // 获取预计转账gas费
         let gasPrice = await web3.eth.getGasPrice();
         // 转账金额以wei为单位
         let balance = await web3.utils.toWei(number);
         var rawTx = {
           from: fromaddress,
           nonce: nonce,
           gasPrice: gasPrice,
           to: toaddress,
           value: balance,
           data: "0x00", //转Token代币会用到的一个字段
         };
   ```

2. 通过转账参数计算最终gas费用，并将通过私钥将转账参数进行编码加密
   **⚠️注意：ethereumjs-tx目前已经处于停用状态**
   > ethereumjs-tx 第三方库请选择1.3.7版本


   ```javascript
   import Tx from "ethereumjs-tx";  
   // 将私钥去除“ox”后进行hex转化
         var privateKey = new Buffer(privatekey.slice(2), "hex");
         //需要将交易的数据进行预估gas计算，然后将gas值设置到数据参数中
         let gas = await web3.eth.estimateGas(rawTx);
         rawTx.gas = gas;
        // 通过 ethereumjs-tx 实现私钥加密Ï
         var tx = new Tx(rawTx);
         tx.sign(privateKey);
         var serializedTx = tx.serialize();
   ```

3. 通过 `sendSignedTransaction` api发送转账交易，并且获取交易id

    ```javascript
      
      web3.eth
        .sendSignedTransaction("0x" + serializedTx.toString("hex"))
        .on("transactionHash", (txid) => {
          console.log("交易成功,请在区块链浏览器查看");
          console.log("交易id", txid);
          console.log(`https://goerli.etherscan.io/tx/${txid}`);
        })
        // .on('receipt', (ret)=>{console.log('receipt')})
        // .on('confirmation', (ret)=>{console.log('confirmation')})
        .on("error", (err) => {
          console.log("error:" + err);
        });
    ```

4. 区块链浏览器或者目标钱包产看转账结果

   goerli区块链浏览器 https://goerli.etherscan.io/tx/交易id
---

# 账户系统

## `密码`

密码不是私钥，它是在创建账户时候的密码（可以修改）
密码在以下情况下会使用到：
1. 作为转账的支付密码
2. 用keystore导入钱包的时候需要输入的密码，用来解锁keystore的

## `私钥 Private Key`

私钥由64位长度的十六进制的字符组成，比如：`0xA4356E49C88C8B7AB370AF7D5C0C54F0261AAA006F6BDE09CD4745CF54E0115A`，一个账户只有一个私钥且不能修改。
通常一个钱包中私钥和公钥是成对出现的，有了私钥，我们就可以通过一定的算法生成公钥，再通过公钥经过一定的算法生成地址，这一过程都是不可逆的。私钥一定要妥善保管，若被泄漏别人可以通过私钥解锁账号转出你的该账号的数字货币。

## `公钥 Public Key`

公钥(Public Key)是和私钥成对出现的，和私钥一起组成一个密钥对，保存在钱包中。*公钥由私钥生成，但是无法通过公钥倒推得到私钥。公钥能够通过一系列算法运算得到钱包的地址，因此可以作为拥有这个钱包地址的凭证。*

## `Keystore`

Keystore常见于以太坊钱包，它是将私钥以加密的方式保存为一份 JSON 文件，这份 JSON 文件就是 keystore，所以它就是加密后的私钥。Keystore必须配合钱包密码才能导入并使用该账号。当黑客盗取 Keystore 后，在没有密码情况下, 有可能通过暴力破解 Keystore 密码解开 Keystore，所以建议使用者在设置密码时稍微复杂些，比如带上特殊字符，至少 8 位以上，并安全存储。

## `助记词 Mnemonic`

私钥是64位长度的十六进制的字符，不利于记录且容易记错，所以用算法将一串随机数转化为了一串12 ~ 24个容易记住的单词，方便保存记录。注意：

1. 助记词是私钥的另一种表现形式
2. 助记词=私钥，这是不正确的说法，通过助记词可以获取相关联的多个私钥，但是通过其中一个私钥是不能获取助记词的，因此**助记词≠私钥**。

## `BIP`

要弄清楚助记词与私钥的关系，得清楚BIP协议，是`Bitcoin Improvement Proposals`的缩写，意思是Bitcoin 的改进建议，用于提出 Bitcoin 的新功能或改进措施。BIP协议衍生了很多的版本，主要有BIP32、BIP39、BIP44。

**BIP32**

BIP32是 HD钱包的核心提案，通过种子来生成主私钥，然后派生海量的子私钥和地址，种子是一串很长的随机数。

**BIP39**

由于种子是一串很长的随机数，不利于记录，所以我们用算法将种子转化为一串12 ~ 24个的单词，方便保存记录，这就是BIP39，它扩展了 HD钱包种子的生成算法。

**BIP44**

BIP44 是在 BIP32 和 BIP43 的基础上增加多币种，提出的层次结构非常全面，它允许处理多个币种，多个帐户，每个帐户有数百万个地址。

在BIP32路径中定义以下5个级别：

```
m/purpose'/coin_type'/account'/change/address_index
```

- purpose：在BIP43之后建议将常数设置为44'。表示根据BIP44规范使用该节点的子树。
- Coin_type：币种，代表一个主节点（种子）可用于无限数量的独立加密币，如比特币，Litecoin或Namecoin。此级别为每个加密币创建一个单独的子树，避免重用已经在其它链上存在的地址。开发人员可以为他们的项目注册未使用的号码。
- Account：账户，此级别为了设置独立的用户身份可以将所有币种放在一个的帐户中，从0开始按顺序递增。
- Change：常量0用于外部链，常量1用于内部链，外部链用于钱包在外部用于接收和付款。内部链用于在钱包外部不可见的地址，如返回交易变更。
- Address_index：地址索引，按顺序递增的方式从索引0开始编号。

BIP44的规则使得 HD钱包非常强大，用户只需要保存一个种子，就能控制所有币种，所有账户的钱包，因此由BIP39 生成的助记词非常重要，所以一定安全妥善保管，那么会不会被破解呢？如果一个 HD 钱包助记词是 12 个单词，一共有 2048 个单词可能性，那么随机的生成的助记词所有可能性大概是`5e+39`，因此几乎不可能被破解。

## `HD钱包`

通过BIP协议生成账号的钱包叫做HD钱包。这个HD钱包，并不是Hardware Wallet硬件钱包，这里的 HD 是`Hierarchical Deterministic`的缩写，意思是分层确定性，所以HD钱包的全称为比特币分成确定性钱包 。

## 密码、私钥、keystore与助记词的关系
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312261553646.png)

## 钱包的核心：私钥

基于以上的分析，我们对以太坊钱包的账号系统有了一个很好的认识，那么我们在使用钱包的过程中，该如何保管自己的钱包呢？主要包含以下几种方式：

- 私钥（Private Key）
- Keystore+密码（Keystore+Password）
- 助记词（Mnemonic code）

通过以上三种中的一种方式都可以解锁账号，然后掌控它，所以对于每种方式中的数据都必须妥善包括，如有泄漏，请尽快转移数字资产。

我们可以得到以下总结：

- 通过私钥+密码可以生成keystore，即加密私钥；
- 通过keystore+密码可以获取私钥，即解密keystore。
- 通过助记词根据不同的路径获取不同的私钥，即使用HD钱包将助记词转化成种子来生成主私钥，然后派生海量的子私钥和地址。

## 通过助记词 - 创建账户
需要使用`bip39`协议将助记词转换成种子，再通过`ethereumjs-wallet`库生成hd钱包，根据路径的不同从hd钱包中获取不同的keypair，keypair中就包含有公钥、私钥，再通过`ethereumjs-util`库将公钥生成地址，从而根据助记词获取所有关联的账号，能获取到公钥、私钥、地址等数据信息。

**1.依赖库**
需要用到三个库：bip39、ethereumjs-wallet/hdkey、ethereumjs-util。先安装依赖库，`cd`到项目跟路径运行命令`npm i bip39 ethereumjs-wallet ethereumjs-util`。

- [bip39](https://github.com/bitcoinjs/bip39)：随机产生新的 mnemonic code，并可以将其转成 binary 的 seed。
- [ethereumjs-wallet](https://github.com/ethereumjs/ethereumjs-wallet)：生成和管理公私钥，下面使用其中 hdkey 子套件来创建 HD 钱包。
- [ethereumjs-util](https://github.com/ethereumjs/ethereumjs-util)：Ethereum 的一个工具库。
- https://iancoleman.io/bip39/


**2.通过助记词创建账号**
* 创建助记词

  ```javascript
  // 引入bip39模块
  import * as bip39 from "bip39";
  // 创建助记词 
  let mnemonic = bip39.generateMnemonic();
  console.log(mnemonic);
  // 结果 12位助记词
  // vote select solar shy embrace immense lizard stamp scrub vague negative forward
  ```
* 根据助记词生成密钥对 keypair
  
  ```javascript
  // 导入分层钱包模块
  import { hdkey } from "ethereumjs-wallet";
  //1.将助记词转成seed
  let seed = await bip39.mnemonicToSeed("12位助记词");
  //3.通过hdkey将seed生成HD Wallet
  let hdWallet = hdkey.fromMasterSeed(seed);
  //4.生成钱包中在m/44'/60'/0'/0/i路径的keypair
  let keypair = hdWallet.derivePath("m/44'/60'/0'/0/0");
  console.log(keypair);
     
  ```
keypair 密钥对
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202312291458907.png)

**3. 由keypair 获取钱包地址和私钥**

```javascript
// 获取钱包对象
let wallet = keypair.getWallet();
// 获取钱包地址
let lowerCaseAddress = wallet.getAddressString();
// 获取钱包校验地址
let CheckSumAddress = wallet.getChecksumAddressString();
// 获取私钥
let prikey = wallet.getPrivateKey().toString("hex");
 console.log("lowerCaseAddress", lowerCaseAddress);
 console.log("CheckSumAddress", CheckSumAddress);
 console.log("prikey", prikey);
/*
lowerCaseAddress 0xd9fc0fd4412616c7075e68b151ab3a7bcb9a3f54
CheckSumAddress 0xd9fC0FD4412616c7075E68b151ab3A7bcB9A3f54
prikey 3fc11495517f1f015bbcb6c311da66e3b26b23e4c91c1285ccc4b69d9d274002
*/
```

##  导出账户

> 一个已经存在的账户导出 私钥 和 keystore

1. 通过分层钱包对象 + 密码 创建keystore
```javascript
  let keystore = await wallet.toV3(data.pass1); // 参数必须为 字符串
```
2. 通过私钥和密码创建 keystore 
``` javascript
const keystore = await web3.eth.accounts.encrypt("账户私钥","密码");
```
```javascript
// 模拟keystore数据
const keystoreJsonV3 = {
        version: 3,
        id: "dbb70fb2-52ad-4e1f-9c19-0b50329f89c3",
        address: "445b469888528dacd9b87246c5ce70407adaa411",
        crypto: {
          ciphertext:
            "1e53e7e775644422600188b8134907992db40d278064ea3a966da4dcdf80db64",
          cipherparams: { iv: "f9d2b047019674eee449b316f4a21491" },
          cipher: "aes-128-ctr",
          kdf: "scrypt",
          kdfparams: {
            dklen: 32,
            salt: "153e074d78d0ba36fae3e46e582c42e53f61653cb5d4f1a3a3f68094e6ca0160",
            n: 8192,
            r: 8,
            p: 1,
          },
          mac: "e91456c59b2505c16b80c2495ab7b4633273c2ae366cb6953f27de8cfebad629",
        },
      };
 const res = web3.eth.accounts.decrypt(keystoreJsonV3, "1235");
 console.log(res);
```
3. 通过keystore解密私钥
```javascript
import ethwallet from "ethereumjs-wallet";  
   let pass = prompt("请输入密码");
       let wallet;
       try {
         wallet = await ethwallet.fromV3(keystore, pass);
       } catch (error) {
         alert("密码错误");
         return false;
       }
   let key = wallet.getPrivateKey().toString("hex");
```

## 导入账户

> 通过 私钥、助记词、keystore 导入一个已经存在的钱包账户 地址 和 私钥

1. 通过keystore获取 私钥和地址 
  ```javascript
  import ethwallet from "ethereumjs-wallet";  
    let pass = prompt("请输入密码");
        let wallet;
        try {
          wallet = await ethwallet.fromV3(keystore, pass);
        } catch (error) {
          alert("密码错误");
          return false;
        }
  let key = wallet.getPrivateKey().toString("hex");
  let address = wallet.getAddressString()
  ```
2. 通过助记词 获取地址和私钥
  ```javascript
  let mnemonic=prompt("请输入助记词")
  let seed = bip39.mnemonicToSeed(mnemonic)
  let hdwallet = hdkey.fromMasterSeed(seed)
  let keypair = hdWallet.derivePath("m/44'/60'/0'/0/0");
  // 获取钱包对象
  let wallet = keypair.getWallet();
  // 获取钱包地址
  let lowerCaseAddress = wallet.getAddressString();
  // 获取钱包校验地址
  let CheckSumAddress = wallet.getChecksumAddressString();
  // 获取私钥
  let prikey = wallet.getPrivateKey().toString("hex");
  ```
3. 通过私钥获取 地址
  ```javascript
  import ethwallet from "ethereumjs-wallet";     
  let privatekey=new Buffer( prompt("请输入私钥"), 'hex' )
  let wallet = ethwallet.fromPrivateKey(privatekey)
  // 获取钱包地址
  let lowerCaseAddress = wallet.getAddressString();
  ```
