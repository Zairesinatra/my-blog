---
layout: post
title:  "props-$attr"
date:   2020-04-23 20:35:03 +0800
category: Bughandle
---

## 前言：

项目中难免遇见组件传值。这里回顾一下遇见的问题。

---

## 正文：

项目中组件传值的时候将 `:search-histories="searchHistories"` 写成了 `:search-history="searchHistories"`，通过 devvue-tools 查看发现没有在 `props` 传入，且组件值变为了 undefined，而是传入给了 `$attr` 。

到这里就需要定位回 父组件查看动态绑定的代码行 或 props 进行排查。

```javascript
// 组件传值父组件中的绑定
// 这里要注意 to-son 是子组件在 props 接收的名称,而 toSon 是父组件中数据的名称
:to-son="toSon"
```

OK，问题解决。小结一下，如果给子组件传 `props` 没有进行注册属性，或者父组件传值表示有误，那么 `$attrs` 则会生效。

---

### 温故几点：

* Prop 的大小写 (camelCase vs kebab-case)

HTML 中的 attribute 名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当使用 DOM 中的模板时，camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名：

```vue
Vue.component('blog-post', {
  // 在 JavaScript 中是 camelCase 的
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
```

```vue
<!-- 在 HTML 中是 kebab-case 的 -->
<blog-post post-title="hello!"></blog-post>
```

* Prop 类型

可以以对象形式提供每个 prop 指定的值类型。这时不仅为组件提供了文档，还会在组件遇到错误的类型时从浏览器的 JavaScript 控制台提示。

```vue
props: {
    searchHistories: {
      type: Array,
      required: true
    },
    Promise: Promise,
    callback: Function,
    ...
  }
```

* 传递静态或动态 Prop

```vue
<blog-post title="props-$attr"></blog-post>
```

```vue
<blog-post :title="post.title + ' by ' + post.author.name"></blog-post>
```

* 单向数据流

所有的 prop 都使得其父子 prop 之间形成了一个单向下行绑定：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外变更父级组件的状态，从而导致应用的数据流向难以理解。

&gt; prop 传递的初始值被子组件期望作为一个本地的 prop 数据来使用 => 定义一个本地的 data property 并将这个 prop 用作其初始值：

```javascript
props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter
  }
}
```

&gt; prop 以一种原始的值传入且需要进行转换 => 使用 prop 的值来定义一个计算属性：
```javascript
props: ['size'],
computed: {
  normalizedSize: function () {
    // trim() 方法会从一个字符串的两端删除空白字符
    return this.size.trim().toLowerCase()
  }
}
```
* Prop 验证

为组件的 prop 指定验证要求，如果有一个需求没有被满足，则 Vue 会在浏览器控制台中警告。这在开发复用的组件时尤其有效。

```javascript
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

## 结束：

关于 vue 的组件传值一直是一个重点，有机会多观看几遍指导文档应该印象会更加深刻。

### 以上

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！