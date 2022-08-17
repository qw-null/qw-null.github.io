---
title: JS使用Promise实现每隔1s输出123……
date: 2022-08-17 11:49:40
tags:
- 各种无厘头手写
---

使用Promise 实现每隔1s输出1，2，3……

```javascript
var a = 0;
function timeCout(){
    return new Promise((resolve)=>{
        a++;
        console.log(a);
        setTimeout(()=>{
            resolve();
        },1000);
    })
}

async function main(){
    await timeCout();
    main();
}

main();
```
