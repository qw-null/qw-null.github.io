---
title: 手写jsonp
date: 2022-09-04 21:34:05
tags: 
- 各种无厘头手写
---
### 手写jsonp的步骤
1. 创建script元素，设置src属性，并插入文档中，同时触发AJAX请求。
2. 返回Promise对象，then函数才行继续，回调函数中进行数据处理。
3. script元素删除清理。

### 代码

```javascript
  function jsonp(url,data={},callback="callback"){
    // 处理json对象，拼接url
    data.callback = callback;
    let params = [];
    for(let key in data){
        params.push(key+'='+data[key]);
    }
    console.log(url+'?'+params.join('&'))
    // 创建script元素
    let script = document.createElement('script');
    script.src = url+'?'+params.join('&');
    document.body.appendChild(script);
    //返回promise
    return new Promise((resolve,reject)=>{
        window[callback] = (data) => {
            try{
                resolve(data);
            }catch(e){
                reject(e);
            }finally{
                // 移除script元素
                script.parentNode.removeChild(script);
            }
        }
    })
  }
```

**如何调用：**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220904222504.png)

### jsonp的原理
利用 `<script>` 标签没有跨域限制的漏洞，网页可以得到从其他来源动态产生的 JSON 数据。JSONP请求一定需要对方的服务器做支持才可以。
#### jsonp的优缺点
+ **优点**是简单兼容性好，可用于解决主流浏览器的跨域数据访问的问题。
+ **缺点**是仅支持get方法具有局限性，不安全可能会遭受XSS攻击。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220904223501.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220904223531.png)