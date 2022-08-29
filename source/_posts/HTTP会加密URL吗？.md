---
title: HTTP会加密URL吗？
date: 2022-08-29 11:17:49
tags:
- 计算机网络
---

字节一面问题：<span style="color:#374ef4;font-weight:800">HTTP会加密URL吗？</span>

<span style="color:#374ef4;font-weight:800">答案是，会加密。</span>

### HTTP加密URL原因分析
因为`URL`的信息都是保存在`HTTP Header`中的，而`HTTPS`是会对`HTTP Header + HTTP Body`整个加密的，所以`URL`自然是会被加密的。

下图是`HTTP/1.1`的请求头部，可以看到是包含`URL`信息的。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220829113351.png)

对应的实际的`HTTP/1.1`的请求头部：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220829113827.png)

`HTTP/1.1`请求的第一行包含请求方法和请求路径。`HTTP/2`用一系列伪头部（pseudo-header）替换了请求行，这五个伪头部很容易识别，因为他们在名称的开头用了一个冒号来表示。

比如请求方法和路径伪头字段如下：
+ ```:method```伪头字段包含HTTP方法
+ ```:path```伪头字段包含目标URL的路径和查询部分

如下图：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220829114347.png)
*此时浏览器现实的信息是已经解密后的信息，并非URL没有加密*

采用抓包工具抓包HTTPS的数据是什么都看不到的，只会显示`Application Data`，表示这是一个已经加密的HTTP应用数据
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220829114559.png)

### HTTPS可以看到域名吗？
从上面我们可以知道，`HTTPS` 是已经把  `HTTP Header + HTTP Body` 整个加密的，所以我们是无法从加密的 `HTTP` 数据中获取请求的域名的。
但是我们<span style="color:#374ef4;font-weight:800">可以在TLS握手的过程中看到域名信息</span>

如下图，TLS第一次握手的“Client Hello” 消息中，有个`server name`字段，它就是请求的域名地址。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220829115538.png)

### HTTPS的应用数据是如何保证完整性的？
很多人以为HTTP数据就是用对称加密密钥（TLS握手过程中协商出来的对称加密密钥）加密后就直接发送了，然后就疑惑HTTP 数据有没有通过摘要算法来保证完整性？

实际上，TLS在实现上分为 <mark>**握手协议**</mark>和<mark>**记录协议**</mark>两层：
+ TLS握手协议就是我们说的TLS四次握手的过程，负责协商加密算法和生成对称密钥，后续用此密钥来保护应用程序数据（即HTTP数据）
+ TLS记录协议负责保护应用程序数据并验证其完整性和来源，所以对HTTP数据加密是使用记录协议

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220829130424.png)
具体过程如下：
+ 首先，消息被分割成多个较短的片段，然后分别对每个片段进行压缩
+ 接下来，经过压缩的片段会被<span style="color:#374ef4;font-weight:800">加上消息认证码（MAC值，这个是通过哈希算法生成的），这是为了保证完整性，并进行数据的认证</span>。通过附加消息认证码的MAC值，可以识别出篡改。与此同时，为了防止重放攻击，在计算消息认证码时，还加上了片段的编码。
+ 再接下来，经过压缩的片段再加上消息认证码会一起通过对称密码进行加密。
+ 最后，上述经过加密的数据再加上数据类型、版本号、压缩后的长度组成的报头就是最终的加密报文数据。

记录协议完成后，最终的加密报文数据将传递到传输控制协议（TCP）层进行传输。