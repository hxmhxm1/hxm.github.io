---
title: vue响应式
date: 2023-07-08 10:39:39
tags:
categories:
  - vue
---

### 一、vue3和vue2区别

#### （1）vue2 中object.defineProperty缺点

●只能监听对象中的某个属性

```js
const person = {age: 3, name: 'hh'}
Object.defineProperty(person, 'age', handles)
handles = {
get(){}
set(newValue){}
}
```

●对对象的添加删除操作，无法劫持，新增set delete

```js
// 当对象person中新增属性时，会没法监听
person.newProperty = '1' ,//不能监听newPorperty属性
```

●对数组中的api无法劫持，重新数组api

```js
arr.push('1');
```

●存在深层嵌套关系，性能问题

#### （2）vue3基于ES6新增的proxy对象实现数据代理以及通过reflect对数据源进行操作

●proxy可以监听整个对象
●proxy（代理）拦截对象中任意属性的变化
●reflect（反射）对源对象的属性进行操作 （Reflect的特性和Object相似）

```js
const person = {age: 3, name: 'hh'}
// 此处只是简写用法，没有定义key，key可以为age或name
new Proxy(person, {
get(){
Reflect.get(person, key)
}
set(newValue){
Reflect.set(person, key, newValue)
}
})
```

### 二、vue3中属性

#### （1）reactive：基于proxy和reflect实现

// 基本使用， 只对对象有用，对其他类型数据不起作用

```js
const state = reactive({ count: 0 })

<div>{{ state.count }}</div>
// 缺点， 解构或者单独赋值后不再具有响应式
const { count } = state
const n = state.count
```

总结：存在2个api缺陷
●只对对象类型有效，而对string，number, boolean无效
●因为vue的响应式系统是通过属性访问进行追踪的，因此我们必须始终保持对该对响应式对象的相同引用，这意味着我们不可以随意替换一个响应式对象，这会导致对初始引用失效（JavaScript 没有可以作用于所有值类型的 “引用” 机制）

```js
let state = reactive({ count: 0 });
// 上面的引用 ({ count: 0 }) 将不再被追踪（响应性连接已丢失！）
state = reactive({ count: 1 });
```

#### (2)ref：原理和Object.defineProperty一样，当传入为对象时，这用reactive再包裹一层

```js
const objectRef = ref({ count: 0 });

// 这是响应式的替换
objectRef.value = { count: 1 };
```

总结：ref() 让我们能创造一种对任意值的 “引用”，并能够在不丢失响应性的前提下传递这些引用
(3)toRef, toRefs

```js
const state = reactive({
foo: 1,
bar: 2
})
const stateAsRefs = toRefs(state)
//想要使用toRef也可以
const foo = toRef(state, 'foo')
/_
stateAsRefs 的类型：{
foo: Ref<number>,
bar: Ref<number>
}
_/

// 这个 ref 和源属性已经“链接上了”
state.foo++
console.log(stateAsRefs.foo.value) // 2

stateAsRefs.foo.value++
console.log(state.foo) //
```
