---
title: JS实现红绿灯
date: 2022-08-15 14:25:29
tags:
- 各种无厘头手写
---
> 输了就是输了，这说明什么呀，小朋友，还得练

### 题目要求：
实现红绿灯，黄灯亮1s，红灯亮2s，绿灯亮3s，一直循环改变颜色。

### 解决方法：
三种方法：①setTimeout ②Promise + async/await

#### setTimeout方式
```javascript
function changeColor(color){
  console.log('改变颜色为：',color);
}

function main(){
  changeColor('yellow');
  setTimeout(() => {
    changeColor('red');
    setTimeout(() => {
      changeColor('green');
      setTimeout(main, 3000);
    }, 2000); 
  }, 1000);
}

main();
```

#### Promise + async/await方式
```javascript
function changeColor(color){
  console.log("改变颜色为"+color);
}

function timer(color,delay){
    return new Promise((resolve)=>{
        changeColor(color);
        setTimeout(()=>{
            resolve();
        },delay);
    })

}
async function main(){
    await timer('yellow',1000);
    await timer('red',2000);
    await timer('green',3000);
    main();
}
main();
```

