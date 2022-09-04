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