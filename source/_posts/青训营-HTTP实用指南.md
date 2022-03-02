---
title: 字节青训营-HTTP实用指南
date: 2022-01-23 14:45:40
categories:
- 字节青训营
---
## 01 初识HTTP
浏览器地址栏输入URL，到显示界面，经历的过程：

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220123145912.png)

#### HTTP介绍
Hyper Text Transfer Protocol，超文本传输协议
+ 应用层协议，基于TCP
+ 请求响应
+ 简单可扩展
+ 无状态

## 02 协议分析
#### 发展：

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220123151034.png)

#### 报文：

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220123151331.png)

#### 字段分析：
##### Method
名称|说明
:----|:----
GET|请求一个指定资源的表示形式，使用GET的请求应该只被用于获取数据
POST|用于将实体提交到指定的资源，通常导致在服务器上的状态变化或副作用
PUT|用于请求有效载荷替换目标资源的所有当前表示
DELETE|删除指定资源
HEAD|请求一个 与GET请求的响应 相同的响应
CONNECT|建立一个到由目标资源标识的服务器的隧道
OPTIONS|用于描述目标资源的通信选项
TRACE|沿着到目标资源的路径执行一个消息环回测试
PATCH|用于对资源应用部分修改


特性：
Safe(安全的)：上述方法中，对于其中不会修改服务器数据的方法，我们认为其是安全的，例如，GET、HEAD、OPTIONS
Idempotent(幂等的)：对于同样的请求执行一次与执行多次的效果是一样的，服务器的状态也是一样的，例如，GET、HEAD、OPTIONS、PUT、DELETE

<i style="background:#ccffcc"><b>所有的safe方法都是idempotent的</b></i>
 
##### 状态码

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220123154523.png)

+ 200 OK-客户端请求成功
+ 301 - 资源（网页等）被永久转移到其他URL
+ 302 - 临时跳转
+ 401 Unauthorized - 请求未经授权
+ 404 - 请求资源不存在
+ 500 - 服务器内部错误
+ 504 Gated Timeout - 网关或代理的服务器无法在规定时间内获得想要的响应

##### 常用请求头
名称|说明
:---|:---
Accept|接收类型，表示浏览器支持的MIME类型（对标服务器端返回的Content-Type）
Content-Type|客户端发送出去实体内容的类型
Cache-Control|指定请求和响应遵循的缓存机制，例如no-cache
If-Modified-Since|对应服务器端的Last-Modified,用来匹配看文件是否变得，只能精确到1s之内
Expires|缓存控制，在这个时间内不会请求，直接使用缓存，服务器端事件
Max-age|代表资源在本地缓存多少秒，有效时间内不会请求，而是使用缓存
If-None-Match|对应服务端的ETag，用来匹配文件是否变动（非常精确）
Cookie|有cookie并且同域访问时会自动带上
Referer|该页面的来源URL（适用于所有类型的请求，会精确到详细页面地址，csrf拦截常用到这个字段）
Origin|最初的请求是从哪里发起的（只会精确到端口），Origin比Referer更尊重隐私
User-Agent|用户客户端的一些必要的信息，如UA头部等

##### 常用的响应头

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220123162231.png)

##### 缓存

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220123164131.png)

must-revalidate会与max-age结合使用，如果缓存中设置了must-revalidate且max-age到期，即使本地有缓存也无法使用。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124094913.png)

##### Cookie
当用户第一次访问并登陆一个网站的时候，cookie的设置以及发送会经历以下4个步骤：

(1)客户端发送一个请求到服务器  →  (2)服务器发送一个HttpResponse响应到客户端，其中包含Set-Cookie的头部  →  (3)客户端保存cookie，之后向服务器发送请求时，HttpRequest请求中会包含一个Cookie的头部  →  (4)服务器返回响应数据

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124100217.png)

属性项|属性项介绍
:---|:---
NAME=VALUE|键值对，可以设置要保存的 Key/Value，注意这里的 NAME 不能和其他属性项的名字一样
Expires|Cookie的有效期，在设置的某个时间点后该 Cookie 就会失效
Domain|限制Cookie 生效的域名，默认为创建cookie的服务域名
Path|限制指定Cookie的发送范围的文件目录，默认为当前
Secure|仅在HTTPS安全连接时，才可以发送Cookie
HTTPOnly|Javascript脚本无法获得Cookie
SameSite|①None同站、跨站请求都可以发送<br>②Strict仅在同站发送<br>③允许与顶级导航一起发送，并将与第三方网站发起的GET请求一起发送

#### HTTP/2
HTTP/2更快、更稳定、更简单
+ 帧（frame）：HTTP/2通信的最小单位，每个帧都包含帧头，至少也会标识出当前帧所属的数据流。
+ 消息：与逻辑请求或响应消息对应的完整的一系列帧。
+ 数据流：已建立的连接内的双向字节流，可以承载一条或多条消息。

HTTP/2连接是永久的，而且仅需要每个来源的一个连接。
流控制：阻止 发送方 向 接收方 发送大量数据的机制。
服务器推送能力。

#### HTTPS
https是http经过TSL/SSL加密后得到的

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124110308.png)

## 03 场景分析

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124110955.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124111106.png)

#### 静态资源方案
静态资源方案：缓存 + CDN + 文件名hash
其中：文件名hash可以确保当前文件是最新的

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124112530.png)

通过用户就近性和服务器负载的判断，CDN确保内容以一种极为高效的方式为用户的请求提供服务

#### 跨域

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124115113.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124115305.png)

解决方法：
+ CORS
+ 代理服务器
+ + 同源策略是浏览器的安全策略，不是HTTP的
+ Iframe - 不方便，限制过多，实际场景中使用较少

#### 下一次进入页面为什么能记住登陆状态？

鉴权：
Session + cookie

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124120535.png)

JWT(JSON web token)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124144602.png)

#### 发起请求
1. AJAX之XHR
XHR：XMLHttpRequest
xhr.readyState属性

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124145225.png)

2. AJAX之Fetch
+ XMLHttpRequest的升级版
+ 使用promise
+ 模块化设计，Response，Request，Header对象
+ 通过数据流处理对象，支持分块读取

3. axios库
+ 支持浏览器、nodejs环境
+ 丰富的拦截器

#### 用户体验方面
1. 网络优化

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124151100.png)

2. 稳定性

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124151133.png)

+ 重试是保证稳定的有效手段，但要防止加剧恶劣情况
+ 缓存合理使用，作为最后一道防线

## 05拓展

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124151328.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220124151403.png)

