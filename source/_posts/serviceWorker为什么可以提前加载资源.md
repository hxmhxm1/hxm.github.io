---
title: 为什么serviceWorker可以提前加载资源
tags:
categories:
  - 网络代理
---

### 一、service worker 和 axios ajax都是异步请求，那为什么说service worker可以预加载资源，减少页面首屏空白时间

    * service worker是在浏览器中重新开启一个线程，拦截和处理网络请求、实现离线访问、资源缓存等功能
    * ajax 或 axios用于发送异步请求，通常在页面主线程中执行，用户获取数据或与服务器进行通信
    * service worker可以在页面加载时预先加载所需的资源（html\css\js文件等），这些资源可以在用户访问时从缓存中快速获取，减少首屏空白时间，相比之下，axios/ajax通常适用于页面加载后根据用户的动态获取数据，而无法像service worker这样提前缓存整个资源文件

### 二、service worker功能

    * 网络请求
    * 离线访问
    * 资源缓存、自定义资源缓存
    * 后台同步
    * 安全性

### 三、service worker如何使用
