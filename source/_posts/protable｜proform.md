---
title: protable｜proform
date: 2023-07-08 10:53:12
tags:
categories:
  - UI组件
---

### ●protable用于数据展示，const ref = useRef<actionType>()

用于多选框清楚、表单重新加载（重置，筛选

### ●proform用于数据录入 , const ref = useRef<formInstance>()

表单内容清空

#### ●search选择框中

```js
// 是一个枚举对象map或者object
fieldProps: {
  multiple: true}
valueEnum: {
  '': {text: ''}
}

//
valueType: 'select',
fieldProps: {
  multiple: true,
  options: [{label:'', value:''}]
}
```

1、valueEnum是枚举类型，可以将数值转化成你想要的值，如果hideInSearch也会是选择框
2、fieldProps:{
