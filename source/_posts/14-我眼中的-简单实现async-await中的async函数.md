---
title: 【我眼中的】 - 【14】简单实现async/await中的async函数
date: 2022-08-31 23:18:04
categories:
- 我眼中的系列
tags:
- 我眼中的系列
---
`async` 函数的实现原理，就是将 `Generator` 函数和自动执行器，包装在一个函数里。
```javascript
function _async (genF) {
  return new Promise(function (resolve, reject) {
    const gen = genF();
    function step (nextF) {
      let next;
      try {
        next = nextF();
      } catch (e) {
        return reject(e);
      }
      if (next.done) {
        return resolve(next.value);
      }
      Promise.resolve(next.value).then(
        function (v) {
          step({
            function () {
              return gen.next(v);
            }
          });
        },
        function (e) {
          step({
            function () {
              return gen.throw(e);
            }
          });
        }
      );
    }
    step(function () {
      return gen.next(undefined);
    });
  });
}
```