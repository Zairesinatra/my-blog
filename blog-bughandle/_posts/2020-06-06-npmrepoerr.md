---
layout: post
title:  "npm ERR! 404 镜像切换"
date:   2020-06-06 22:34:22 +0800
category: Bughandle
---

## 前言场景：

日常翻车：准备用 Grunt 编译 Bootstrap 的 dist 文件前需要 `npm install` 安装好各个 modules 。然后出现如下报错代码：
<pre>
    $ npm install 
    npm ERR! code E404
    npm ERR! 404 Not Found - GET https://registry.npm.taobao.org/@types/babylon/-/babylon-6.16.4.tgz - [not_found] document not found
    npm ERR! 404 
    npm ERR! 404  '@types/babylon@https://registry.npm.taobao.org/@types/babylon/-/babylon-6.16.4.tgz' is not in the npm registry.
    npm ERR! 404 You should bug the author to publish it (or use the name yourself!)
    npm ERR! 404 
    npm ERR! 404 Note that you can also install from a
    npm ERR! 404 tarball, folder, http url, or git url.
</pre>

---

## 解决方案：

<strong>长话短说，切换镜像。</strong>

查看当前登录状态：

<pre>
$ npm whoami
zairesinatra
</pre>

<img src="/my-blog/blog-bughandle/assets/images/whoami.png">

查看目前是否使用淘宝镜像：

<pre>
$ npm config get registry
https://registry.npm.taobao.org/
</pre>

是则切换为npm仓库：

<pre>
npm config set registry https://registry.npmjs.org
</pre>

此时再使用 `npm install` 安装模块即可。

## 以上

本博客所有文章除特别声明外，均采用 CC BY-SA 4.0 协议 ，转载请注明出处!