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

<img src="https://res.cloudinary.com/dzb9ldnvl/image/upload/v1623072374/blog/vrss_dznlct.png" width="50%" />

## 结束

### 以上