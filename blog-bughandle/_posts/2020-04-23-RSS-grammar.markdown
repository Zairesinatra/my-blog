---
layout: post
title:  "RSS Grammer"
date:   2020-04-23 21:36:03 +0800
category: Bughandle
---

## 前言

今天把本站的 RSS 中 feed.xml 补全时写出了 BUG ，这里记录一下。

## 正文

在 `_posts` 中 markdown 命名与内部文件里 `title` 里写入了 `&` 符号。在后续测试 RSS 中出现报错。

```
error on line 12 at column 22: xmlParseEntityRef: no name

Below is a rendering of page up to the first error.
```
<img src="https://res.cloudinary.com/dzb9ldnvl/image/upload/v1623072159/blog/xmlerror_f3vtko.png" width="" />

原因是 XML 文本中的某处有一个杂散的 `&`（与号字符）。记住各处都需要更改回来。

更改完毕上传 Github 进行订阅测试。这里使用通过 homebrew 安装的 vienna 即可。

<img src="https://res.cloudinary.com/dzb9ldnvl/image/upload/v1623072374/blog/vrss_dznlct.png" width="50%" />

[vienna](http://localhost:4000/my-blog/blog-bughandle/assets/images/vienna.png)

## 结束

总结一下还是多注意 xml 的语法。

### 以上

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！