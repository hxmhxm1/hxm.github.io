---
title: babel\core.js使用
tags:
categories:
  - 兼容性
---

一、babel、core.js是什么
babel是js的编译器，主要是将ES6这些比较新的语法转成es5这种向后兼容的浏览器可以支持的语法，但是babel没法包含所有的新的api库，比如js比较新的api，
因此需要在babel中增加polyfill贴片，core.js就是包含这种贴片的库
二、如何配置
(1)安装babel
  npm install --save-dev @babel/core @babel/cli
  npm install --save-dev @babel/preset-env
  npm install core-js
(2)在项目根目录配置.babelrc文件
{
  "presets": [
    ["@babel/preset-env", {
      "useBuiltIns": "usage",
      "corejs": 3
    }]
  ]，
  "plugins": [
    {

    }
  ]
}
注：这里的presets是指已经预设的一组polyfill库

在需要转换的页面上面直接引入
import "core-js/stable";
import "regenerator-runtime/runtime";
