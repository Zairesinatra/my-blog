---
layout: post
title:  "JavaScript's array traversal methods"
date:   2020-10-24 20:10:24 +0800
category: Tech

---

## 前言：

不论算法还是开发中都不可避免的需要循环历遍，用到的不同方法可能会导致不同的执行效率与结果，本文将讨论四种常见的历遍方法：`for (let i = 0; i < arr.length; i++)`、`for (let i in arr)`、`for (const v of arr)`、`arr.forEach((v, i) => { func })`

---

## 正文：

### 基础细节：

先是最需要注意的小细节，使用小括号的形参默认在 `for (let i = 0; i < arr.length; i++)` 和 `for (let i in arr)` 中是访问数组的下标，而不是实际的数组元素值：

```js
const zsarr = [5,4,3,2,1,0]
for (let i = 0; i < zsarr.length; i++) {
    console.log(zsarr[i]); // 5 4 3 2 1 0
    console.log(i); // 0 1 2 3 4 5
}

for (let i in zsarr) {
    console.log(arr[i]); // 5 4 3 2 1 0
    console.log(i); // 0 1 2 3 4 5
}
```

而在`for (const v of arr)`、`arr.forEach((v) => { func })`，则是访问的数组的元素值：

```js
const zsarr = [5,4,3,2,1,0]
for (const v of zsarr) {
    console.log(v); // 5 4 3 2 1 0
}
zsarr.forEach((v, i) => console.log(v)); // 5 4 3 2 1 0
```

这里注意使用 `arr.forEach((v) => { func })` ，加入的第二形参则是访问数组的下标：

```js
const zsarr = [5,4,3,2,1,0]
zsarr.forEach((v, i) => console.log(i)); // 0 1 2 3 4 5
```

### 特殊属性：

- `for (let i in arr)`不会忽略非数字属性：

[使用`for(let i in arr)`遍历数组风险](https://stackoverflow.com/questions/500504/why-is-using-for-in-with-array-iteration-a-bad-idea)。而其他历遍都会忽略添加属性。

```js
// 数组是可以添加属性的Object
const zsarr = ["a", "b", "c"];
zsarr.test = "Less used for in";
for (let i in zsarr) {
    console.log(zsarr[i]); // a b c Less used for in
}
```

- 数组中 空元素 与 `undefined` 的处理不同：

`for (let i in arr)` 与 `arr.forEach((v, i) => { func })` 会跳过空元素；而对于 `undefined` 上述方式历遍都不会跳过。

```js
// 方法一
const zsarr = [1, ,3];
// 方法二
const const zyarr = [1, 2, 3];
arr[5] = 5;
const zs = [1, undefined, 3]
```

- `forEach`遍历时的 `this` ：

```js
// 非严格模式
const zsarr = [1];
zsarr.forEach(function() {
    console.log(this === globalThis); // true
});
zsarr.forEach(() => {
    console.log(this); // {}
});
```

```js
// 严格模式
"use strict";
const zsarr = [1];
zsarr.forEach(function() {
    console.log(this == undefined); // true
});
zsarr.forEach(() => {
    console.log(this); // {}
});
```

## 结束：

如果不需要关注索引问题， `for (const v of arr)`是最稳定的遍历方式，特例较少，支持 break （`forEach不支持`）。若出现需要考虑索引的情况可以使用 `Array.prototype.entries()` 返回一个新的[数组迭代器对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/entries)，该对象包含数组中每个索引的键/值对。

```js
const zsarr = ['a', 'b', 'c'];
for (const [index, element] of zsarr.entries()) {
  console.log(index, element);
}
// 0 'a'
// 1 'b'
// 2 'c'
```

```js
var zsarr = ['a', 'b', 'c'];
var iterator = zsarr.entries();
for (let e of iterator) {
  console.log(e);
}
// [0, 'a']
// [1, 'b']
// [2, 'c']
```

### 以上

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！