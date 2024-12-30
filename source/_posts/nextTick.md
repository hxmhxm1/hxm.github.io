---
title: nextTick
tags: nextTick
categories:
  - vue中api
---

## 是什么，如何使用

  在页面下一次渲染完成后执行的回调函数
  nextTick((callBack)).then(() => {})

## 实现原理

* 背景：js是单线程的，但是存在任务队列机制可以去处理异步任务；任务队列中又分为宏任务和微任务，其中微任务优先级高于宏任务；
  当执行栈中的任务执行完了之后，会把任务队列中的微任务全部执行，然后再取宏任务放到执行栈中执行，执行完了之后再重新执行所有微任务，以此形成循环
* nextTick也是利用了这个原理，具体实现是通过promise(微任务)，setTimeOut(宏任务)实现的
  * 判断浏览器是不是支持promise,支持的话创建一个promise,把回调放到promise.then(() => {})中
  * 判断是不是支持mutationObserve,这个方法会在页面dom元素发生更改后执行，这也可以保证在下一次页面渲染后立即执行
  * 如果上面方法都不支持的话，就用setTimeOut(() => callBack(), 0)代替
