---
title: HTTP和HTTPS的区别
date: 2022-08-29 17:16:53
tags:
- 计算机网络
---
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220829193338.png)

`HTTPS`是在`HTTP`的基础上加入`SSL`协议，`SSL`依靠证书来验证服务器的身份，并为浏览器和服务器之间的通信加密（在传输层）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220904205651.png)

<mark>**HTTP + 加密 + 认证 + 完整性保护 = HTTPS**</mark>

1. `HTTPS`协议需要到`CA`申请证书，需要一定的费用
2. `HTTP`的信息是明文传输；`HTTPS`是具有安全性的SSL加密传输协议
3. `HTTP`是直接与`TCP`进行数据传输；而`HTTPS`运行在`SSL/TLS`(安全传输层协议)之上，`SSL/TLS`运行在`TCP`之上，用的端口也不一样，`HTTP`是80端口，`HTTPS`是443端口
4. `HTTP`的连接很简单，是无状态的；`HTTPS`协议是由`SSL+HTTP`协议构建的，可进行加密传输、身份认证的网络协议，比`HTTP`协议安全
5. `HTTP`响应比`HTTPS`快，是因为`HTTP`使用`TCP`三次握手建立连接，客户端与服务器之间只交换3个包。`HTTPS`除了`TCP`的三个包，还有`SSL`握手的9个包，<mark>**共12个包**</mark>。

#### some questions
1. `HTTP`是无状态协议，这个如何理解?
答：也就是说服务器不维护任何有关客户端过去所发请求的消息。这其实是一种懒政，有状态协议会更加复杂，需要维护状态(历史信息)，而且如果客户或服务器失效，会产生状态的不一致，解决这种不一致的代价更高。
2. `HTTP` 属于哪一层的协议啊?`HTTPS`呢？
答：`HTTP` 是应用层协议，以 `TCP`(传输层)作为底层协议，默认端口为 `80`。`HTTPS`是应用层协议，运行在`SSL/TLS`(安全传输层协议)之上，`SSL/TLS`运行在`TCP`之上，默认端口为`443`。
3. `HTTP`通信过程是什么？
+ 服务器在 80 端口等待客户的请求
+ 浏览器发起到服务器的 TCP 连接(创建套接字 Socket)
+ 服务器接收来自浏览器的 TCP 连接
+ 浏览器(HTTP 客户端)与 Web 服务器(HTTP 服务器)交换 HTTP 消息
+ 关闭 TCP 连接
4. 使用 `SSL/TLS` 进行通信的双方需要使用非对称加密方案来通信，但是非对称加密设计了较为复杂的数学算法，在实际通信过程中，计算的代价较高，效率太低，因此，`SSL/TLS` 实际对消息的加密使用的是对称加密。
5. TLS握手过程
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220829202914.png)
*（**第1随机数**：由客户端产生，随Client Hello过程发送给服务器端；**第2随机数**：由服务器端产生，随Server Hello过程发送给客户端；**第3随机数（预主密钥）**：由客户端产生，使用公钥加密后传递给服务器端，服务器端接收后使用自己的私钥解密得到预主密钥）*

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220829214411.png" width="50%" />

**步骤1**： 客户端通过发送Client Hello报文开始SSL通信。报文中包含客户端支持的SSL的指定版本、加密组件（Cipher Suite）列表（所使用的加密算法及密钥长度等）。
**步骤2**： 服务器可进行SSL通信时，会以Server Hello报文作为应答。和客户端一样，在报文中包含SSL版本以及加密组件。服务器的加密组件内容是从接收到的客户端加密组件内筛选出来的。
**步骤3**： 之后服务器发送Certificate报文。报文中包含公开密钥证书。
**步骤4**： 最后服务器发送Server Hello Done报文通知客户端，最初阶段的SSL握手协商部分结束。
**步骤5**: SSL第一次握手结束之后，客户端以Client Key Exchange报文作为回应。报文中包含通信加密中使用的一种被称为Pre-master secret的随机密码串。该报文已用步骤3中的公开密钥进行加密。
**步骤6**： 接着客户端继续发送Change Cipher Spec报文。该报文会提示服务器，在此报文之后的通信会采用Pre-master secret密钥加密。
**步骤7**： 客户端发送Finished报文。该报文包含连接至今全部报文的整体校验值。这次握手协商是否能够成功，要以服务器是否能够正确解密该报文作为判定标准。
**步骤8**： 服务器同样发送Change Cipher Spec报文。
**步骤9**： 服务器同样发送Finished报文。
**步骤10**： 服务器和客户端的Finished报文交换完毕之后，SSL连接就算建立完成。当然，通信会受到SSL的保护。从此处开始进行应用层协议的通信，即发送HTTP请求。
**步骤11**： 应用层协议通信，即发送HTTP响应。
**步骤12**： 最后由客户端断开连接。断开连接时，发送close_notify报文。上图做了一些省略，这步之后再发送TCP FIN报文来关闭与TCP的通信。


<mark>https的详细握手过程</mark>
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220829230757.png)