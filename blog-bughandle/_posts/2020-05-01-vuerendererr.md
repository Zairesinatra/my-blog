---
layout: post
title:  "vuerendererr"
date:   2020-05-01 21:34:22 +0800
category: Bughandle
---

## 前言：

在热更新完成后，将" 收藏"组件定义注册渲染时控制台出现类型报错。

---

## 正文：

项目中出现数据类型错误的组件需要父组件传递是否收藏的 Boolean 类型数据。而子组件中 props 设置代码如下:

```
props: {
  value: {
   type: Boolean,
   required: true
  }
},
```

Console面板报错如下:
[err](https://github.com/Zairesinatra/PictureBed/blob/main/ioblogpic/err.png)

此时考虑到所需数据初始时为空对象，在请求完成后才会存储数据。那么绑定渲染子组件时还未请求成功。这里看就会导致：子组件先渲染，带请求数据未传值进。那么可以尝试在完后请求后再渲染子组件。调整代码中子组件的位置即可修复此 error 。

## 结束：

此 BUG 主要是先定位报错的组件，在考虑数据的渲染顺序，即可解决问题。

### 以上

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！
