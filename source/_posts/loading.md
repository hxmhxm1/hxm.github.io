---
title: loading组件
date: 2023-08-20
tags:
categories:
  - UI组件
---

```html
<div class="loading"></div>

<style>
  .loading {
    width: 30px;
    height: 30px;
    border: 2px solid #000;
    border-top-color: transparent;
    border-radius: 100%;

    animation: circle infinite 0.75s linear;
  }

  /* 转转转动画 */
  @keyframes circle {
    0% {
      transform: rotate(0);
    }
    100% {
      transform: rotate(360deg);
    }
  }
</style>
```

```html
<div class="loading">
  <div></div>
  <div></div>
  <div></div>
</div>
<style>
  .loading {
    width: 1.5625rem;
    height: 1.5625rem;
    display: block;
    font-size: 0;
    color: #000;
    position: relative;
  }

  .loading > div {
    display: inline-block;
    float: none;
    background-color: currentColor;
    border: 0 solid currentColor;
    position: absolute;
    top: 0;
    left: 0;
    width: 0.5rem;
    height: 0.5rem;
    border-radius: 100%;
  }

  .loading > div:nth-child(1) {
    animation: ball-triangle-path-ball-one 3s 0s ease-in-out infinite;
  }

  .loading > div:nth-child(2) {
    animation: ball-triangle-path-ball-two 3s 0s ease-in-out infinite;
    background: rgba(179, 179, 180, 1);
  }

  .loading > div:nth-child(3) {
    animation: ball-triangle-path-ball-tree 3s 0s ease-in-out infinite;
    background: rgba(114, 113, 115, 1);
  }

  @keyframes ball-triangle-path-ball-one {
    0% {
      transform: translate(0, 220%);
    }

    17% {
      opacity: 0.5;
    }

    33% {
      opacity: 1;
      transform: translate(110%, 0);
    }

    50% {
      opacity: 0.5;
    }

    66% {
      opacity: 1;
      transform: translate(220%, 220%);
    }

    83% {
      opacity: 0.5;
    }

    100% {
      opacity: 1;
      transform: translate(0, 220%);
    }
  }

  @keyframes ball-triangle-path-ball-two {
    0% {
      transform: translate(110%, 0);
    }

    17% {
      opacity: 0.5;
    }

    33% {
      opacity: 1;
      transform: translate(220%, 220%);
    }

    50% {
      opacity: 0.5;
    }

    66% {
      opacity: 1;
      transform: translate(0, 220%);
    }

    83% {
      opacity: 0.5;
    }

    100% {
      opacity: 1;
      transform: translate(110%, 0);
    }
  }

  @keyframes ball-triangle-path-ball-tree {
    0% {
      transform: translate(220%, 220%);
    }

    17% {
      opacity: 0.5;
    }

    33% {
      opacity: 1;
      transform: translate(0, 220%);
    }

    50% {
      opacity: 0.5;
    }

    66% {
      opacity: 1;
      transform: translate(110%, 0);
    }

    83% {
      opacity: 0.5;
    }

    100% {
      opacity: 1;
      transform: translate(220%, 220%);
    }
  }
</style>
```

#### 主要是使用了css中关键帧动画

- @keyframes keyName {
  0%:{
  transform: rotate/ translate
  }
  100%: {

  }
  }

- animation: keyName(关键帧名称) time(周期) infinate(循环） linear(线性变化)
- animation-delay: time（时间
- transform: rotate(旋转) translate(位移)

注：引用文章链接
<https://juejin.cn/post/7037036742985121800#heading-1>
