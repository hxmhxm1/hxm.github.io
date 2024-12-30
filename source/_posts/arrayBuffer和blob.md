---
title: arrayBuffer和blob
tags: arrayBuffer和blob
categories:
  - 数据类型
---

## 什么是arrayBuffer

* arrayBuffer对象表示原始的二进制数据缓冲区
* 不能直接操作arrayBuffer,只能通过类型化数组或者DataView。它们会将缓存区中的数据表示为特定格式，并通过这些特定格式来读取缓冲区中的内容
* 是可以转移的对象，存储在内存中

## 什么是blob

* blob表示不可变的、原始数据的类文件对象，存储在磁盘上
* File接口继承blob的功能并将其扩展，应用到用户系统的文件上
* 是浏览器默认的二进制文件类型，常用于音频视频文件

## arrayBuffer和blob关

* 都是处理二进制数据的对象
* 可以相互转化
  arrayBuffer -> blob     new Blob([arrayBuffer], {type: 'application/json'})
  blob -> arrayBuffer
    const reader = new FileReader()
    reader.addEventListener('loadend', () => {
      // reader.result 包含被转化类型化数组的blob中的内容
    })
    reader.readAsArrayBuffer(blob)

## axios网络请求接收二进制数据配置

  axios.get('', {responseType: 'arrayBuffer'})
  需要增加responseType: 'arraybuffer', 不然浏览器会默认以blob的形式区接收数据，但是这个数据其实是arraybuffer，会造成解析数据异常

## 什么是类型化数组

* 类似数组的结构，提供了一种用于在内存中访问原始二进制数据的机制
* 该数组没有继承Array.prototype属性，使用Array.isArray()访问是false, 不能使用pop push等方法
* 常用于音频编辑和WebGL中
* 是强类型数据 Int8Array() Unit8Array()
