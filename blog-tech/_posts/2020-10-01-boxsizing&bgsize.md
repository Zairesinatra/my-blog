---
layout: post
title:  "box-sizing与background-size"
date:   2020-10-01 21:29:13 +0800
category: Tech

---

盒模型与图像问题遇见比较频繁，而且都可以有多重选择方式，选择不正确通常会导致布置内容时遇到许多陷阱。这里拿出来对照记忆。

---

### **`box-sizing`：**

- **`content-box`:**

默认情况下，如果元素有任何边框（`border`）或内边距（`padding`），则默认会被添加到元素的 `width` 和 `height` 以呈现在屏幕。

意思就是如果 Parent container 刚好包裹住 Child container 那么由于默认的 `box-sizing: content-box;` 会导致  Child container 偏移出 Parent container 刚好包裹的状态。

- **`border-box`:**

`border-box `告诉浏览器为元素的宽度和高度设定的指定值中已经会包括任何可能会后续设置的边框和填充。

那么可以理解为，如果 Parent container 刚好包裹住 Child container ，且设置了 `box-sizing: border-box;` 那么此时增加任何边框（`border`）或内边距（`padding`）不会使  Child container 偏移出 Parent container 刚好包裹的状态。

---

### **`background-size`：**

`background-size` 属性设置元素的背景图像的大小。没有被背景图像覆盖的空间被 `background-color` 属性填充，背景颜色将在具有透明度/半透明的背景图像后面可见。

- **`background-size: contain;`：**

在不裁剪或拉伸图像的情况下，在其容器内尽可能大地缩放图像，且可能**会导致平铺出现**（如果容器大于图像），除非该 `background-repeat` 属性设置为`no-repeat`。

- **`background-size: cover;`：**

尽可能大地缩放图像以填充容器，必要时拉伸图像。如果图像的比例与元素不同，则将其垂直或水平裁剪，以便不留空白。简单来说就是哪个方向存在留白，则将图像伸缩以铺满容器，而超过的的部分进行裁剪。

---

## 结束

结两个传送阵，直观对比一下不同设置的效果：[`box-sizing`](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)、[`background-size`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-size)

### 以上

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！