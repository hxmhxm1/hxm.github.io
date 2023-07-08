---
title: 二次封装el-input
date: 2023-07-08 10:58:04
tags:
categories:
  - UI组件
---

### 二次封装el-input

#### v-model实现数据双向版本

```js
<input v-model="searchText" />
//等效于
<input
  :value="searchText"
  @input="searchText = $event.target.value"
/>

//当在一个组件中使用时
<CustomInput
  :modelValue="searchText"
  @update:modelValue="newValue => searchText = newValue"
/>
//等效于
<CustomInput v-model="searchText" />

//CustomInput.vue
<script setup>
defineProps(['modelValue'])
defineEmits(['update:modelValue'])
</script>

<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>


//例子
<UserName
  v-model:first-name="first"
  v-model:last-name="last"
/>
<script setup>
defineProps({
  firstName: String,
  lastName: String
})

defineEmits(['update:firstName', 'update:lastName'])
</script>

<template>
  <input
    type="text"
    :value="firstName"
    @input="$emit('update:firstName', $event.target.value)"
  />
  <input
    type="text"
    :value="lastName"
    @input="$emit('update:lastName', $event.target.value)"
  />
</template>
```
