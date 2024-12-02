---
title: loadsh
date: 2023-07-08 10:36:03
tags: 'loadsh'
categories:
  - 插件使用
---

## loadsh

### ●omit 从对象中过滤属性

```js
_.omit(object, [props]);
var object = { a: 1, b: '2', c: 3 };
const res = omit(object, ['a', 'c']);
// 结果：{'b': '2'}
```

### ●pickBy 从对象中筛选出某个属性

```js
pickBy(object, function)
var object = { 'a': 1, 'b': '2', 'c': 3}
const res = pickBy(object, (value: any) => typeof(value)==='number')
//结果： {'a': 1, 'c': 2}
```
