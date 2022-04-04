---
title: Vue
date: 2020-11-29 22:36:23
tags:
  - Vue
categories:
  - [前端, Vue]
---

[Vue.js](https://v3.cn.vuejs.org/)

[Vue.js](https://cn.vuejs.org/)

[Home | Vue Router](https://router.vuejs.org/zh/)

## 备忘录

- `template` 是透明的

## 组件基础

### 模板语法

{{}}

v-bind ------ :

v-on ------- @

- 动态参数

- 修饰符

### Data 计算属性 侦听器

### 条件和列表渲染

`v-if` `v-else-if` `v-else`

```html
<li v-for="value in myObject">
  {{ value }}
</li>


<li v-for="(value, name, index) in myObject">
  {{ index }}. {{ name }}: {{ value }}
</li>

<li v-for="(item, index) in items" :key="item.id"/>
```

#### :key

提高速度 不要用 index 涉及到 diff 算法

#### 关于数组

Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括：

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

你可以打开控制台，然后对前面例子的 `items` 数组尝试调用变更方法。比如 `example1.items.push({ message: 'Baz' })`。

变更方法，顾名思义，会变更调用了这些方法的原始数组。相比之下，也有非变更方法，例如 `filter()`、`concat()` 和 `slice()`。它们不会变更原始数组，而**总是返回一个新数组**。当使用非变更方法时，可以用新数组替换旧数组：

```javascript
example1.items = example1.items.filter(item => item.message.match(/Foo/))
```

你可能认为这将导致 Vue 丢弃现有 DOM 并重新渲染整个列表。幸运的是，事实并非如此。Vue 为了使得 DOM 元素得到最大范围的重用而实现了一些智能的启发式方法，所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。

有时，我们想要显示一个数组经过过滤或排序后的版本，而不实际变更或重置原始数据。在这种情况下，可以创建一个计算属性，来返回过滤或排序后的数组。

在计算属性不适用的情况下 (例如，在嵌套的 `v-for` 循环中) 你可以使用一个方法：

[列表渲染 | Vue.js](https://v3.cn.vuejs.org/guide/list.html#%E6%98%BE%E7%A4%BA%E8%BF%87%E6%BB%A4-%E6%8E%92%E5%BA%8F%E5%90%8E%E7%9A%84%E7%BB%93%E6%9E%9C)

#### 数组范围

```html
<span v-for="n in 10" :key="n">{{ n }} </span>
```

#### 其他

**不**推荐在同一元素上使用 `v-if` 和 `v-for`

### 事件处理 表单

v-model

特殊变量 `$event` 为原始dom事件

## 组件深入

### 组件注册

```js
export default {
  name: "App",
  components: {
    ConditionAndList,
    ParentComponent,
  },
};
```

全局

```
app.use(Component)
```

### props

v-bind: 简写:

```javascript
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']


props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // 或任何其他构造函数
}
```

静态的字符串可以直接用 props 使用，动态和其他类型都要:

v-bind="obj" obj 绑定所有props

在  script 中用camelCase，template 中自动转换为 kebab-case

没有声明的 props 会挂到子组件根元素里

用 this.$attrs 获取

不想挂到根元素 inheritAttrs: false

没有根元素报错

### 自定义事件

```
this.$emit('myEvent')
```

```
<my-component @my-event="doSomething"></my-component>
```

可以通过 `emits` 选项在组件上定义发出的事件。

```
app.component('custom-form', {
  emits: ['inFocus', 'submit']
})
```

当在 `emits` 选项中定义了原生事件 (如 `click`) 时，将使用组件中的事件**替代**原生事件侦听器。

### 父子组件传值

父→子 props

子→父 emit
