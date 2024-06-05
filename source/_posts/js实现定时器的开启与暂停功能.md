---
title: js实现定时器的开启与暂停功能
date: 2022-08-15 19:58:51
tags:
- 各种无厘头手写
---

#### 定时器类
```javascript
class myTime{
  /**
   * @param {Function} cb 回调函数
   * @param {Number} sec 执行间隔时间ms
   */
  constructor(cb,sec){
    this.cb = cb;
    this.sec = sec;
    this.timer = null;
  }

  // 运行
  run(){
    this.timer = setInterval(this.cb , this.sec);
  }

  //暂停
  stop(){
    clearInterval(this.timer);
    this.timer = null;
  }
}
```

#### 使用场景：页面倒计时
```javascript
let count = 10;
let timer = new myTime(()=>{
  if(count === 0){
    timer.stop();
    timer = nul;;
  }else{
    console.log(count);
    count--;
  }
},1000)

timer.run()
```
可以实现页面从10倒计时到1