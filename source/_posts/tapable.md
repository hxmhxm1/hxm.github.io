---
title: tapable
tags: webpack实现原理tapable
categories:
  - 打包工具
---

## tapable是什么

控制事件流中的一个库，也是webpack中plugin中使用的一个库，
webpack中的plugin是在打包过程中的不同阶段去执行的一些钩子

## 如何使用

  tap(注册时调用)
  call(调用时触发)
  syncHook举例

  ```js
     const { SyncHook } = require('tapable');
      // 实例化同步钩子
      const syncHook = new SyncHook(['name']);
      // 注册事件
      syncHook.tap('A', (name) => {
        console.log('A:', name);
      });
      syncHook.tap('B', (name) => {
        console.log('B:', name);
      });
      // 触发事件
      syncHook.call('breeze');
      // 输出结果
      // A: breeze
      // B: breeze

  ```
