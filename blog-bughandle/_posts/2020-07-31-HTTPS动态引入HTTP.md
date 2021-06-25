---
layout: post
title:  "HTTPS页面动态引入HTTP资源"
date:   2020-07-31 21:29:13 +0800
category: Bughandle
---

## 报错场景：

日常翻车：站点挂 Ubuntu 服务器后出现如下报错代码：
<pre>
Mixed Content: The page at 'https://nav.zairesinatra.com/' was loaded over HTTPS, but requested an insecure script 'http://cdn.jsdelivr.net/npm/meting@1.2.0/dist/Meting.min.js'. This request has been blocked; the content must be served over HTTPS.
</pre>
<img src="/my-blog/blog-bughandle/assets/images/httperr.png">

---

## 原因分析：

> If you are really upset with Chrome browsers warnings that your HTTPS enabled website contains unsecured third-party contents that sometimes force your users to close the tab, *Google has solved this problem for you.*

详述点击<a href="https://thehackernews.com/2015/04/disable-mixed-content-warning.html">此页面</a>。其实比较明朗了，随着 Chrome 浏览器的版本更新，为确保 HTTPS 网站不受不安全的 HTTP 资源危害，让任何 HTTPS 页面从未加密的 HTTP URL 加载任何资源时，Chrome 浏览器都会 block request。（曾经的 Chrome 操作为——挂锁上以黄色三角形的形式标记“**混合内容警告**”）。这也就会导致引入一个 `.js` 文件时被直接 block 掉，用 `<iframe>`引入也被拒掉。当然在 HTTPS 里发 AJAX 请求 HTTPS 不会被 block ，而只是请求 HTTP 资源时，会被直接 block 掉。

OK，让我进一步阐述一下为什么会有风险存在。如果我的网站启用了 HTTPS ，但我的网站的页面正在加载通过常规、明文 HTTP URL 检索的内容（例如图像），则可以认为这次连接只是部分加密。对安全网页上的未加密的HTTP内容可能被黑客访问，以及可以通过修改*人在这方面的中间人*（MITM）攻击，这会导致不安全的连接。这中页面学名也叫 `a mixed content page`。

---

## 解决方案：

<strong>长话短说，升级不安全资源。</strong>详见谷歌<a href="https://blog.chromium.org/2015/04/chrome-43-beta-web-midi-and-upgrading.html">官方文章</a>

```html
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">
```

`<head>` 元素与 `<body>` 元素不同，其内容不会在浏览器中显示，且具有保存页面 元数据 的作用。而 HTML 文档的元数据由 `<meta>` 标签提供。

------

## 以上

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！