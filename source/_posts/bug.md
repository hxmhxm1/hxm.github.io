---
title: bug
date: 2023-07-08 10:55:28
tags:
categories:
  - vue
---

v-for中设置:key="index"，在数组中使用splice方法，结果造成数据删除或增加数据时混乱，这是因为虚拟dom时会将旧和新的vnode进行比较，但是两者之间的key又没有发生变化，造成了数据混乱，所以尽量不要使用index作为key
![](../images/9.png)
![](../images/10.png)
