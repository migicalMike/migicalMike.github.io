---
title: 从零做一个静态网页：计算机网络实验一记录
date: 2026-09-22 10:20:00
updated: 2026-09-22 10:20:00
description: 实验一要求做一个能公开访问的静态网页。这篇记录写我用 HTML + CSS + Hexo 完成它的完整过程，包括目录结构、样式表组织方式，以及怎么查看网页用的 HTTP 协议版本。
categories:
  - 学习记录
tags:
  - HTML
  - CSS
  - Hexo
  - 计算机网络
cover: /img/hero-campus.svg
top_img: /img/hero-campus.svg
---

实验一的题目是「静态网页制作」，要求做一个主题明确、内容充实的网页，
里面至少有图片和三个超链接，并且要能公开访问。

我选的主题是学校介绍。这篇记录把整个过程梳理一遍，
也顺便把「怎么查看网页的 HTTP 版本号」这件事写清楚。

<!-- more -->

## 一、实验要求

- 制作一个主题明确、内容充实的网页。
- 发布一篇新的博客文章（就是你正在看的这篇）。
- 检查网页使用的 HTTP 协议版本。
- 在网页中加入至少三个超链接，并包含图片。

网页主题可以在自我介绍、家乡介绍、学校介绍、生活分享、研究分享中选择，
页面框架可以自行决定，**不要求必须使用 Hexo**。

## 二、目录结构

一个静态网站通常由多个文件共同组成，我采用的是最经典的划分方式：

```text
site/
├─ index.html       # 默认首页
├─ images/          # 图片
├─ css/             # 样式表
└─ js/              # JavaScript
```

把结构、样式、资源分开，好处是改样式不用动内容，换图片不用碰代码。

## 三、HTML：先把内容组织好

HTML 是超文本标记语言，浏览器读取标签，把文字、图片、链接显示成网页。
一个最小的页面包含文档声明、头部信息和页面主体：

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>我的主页</title>
  </head>
  <body>
    <h1>欢迎访问我的主页</h1>
    <p>这是正文内容。</p>
  </body>
</html>
```

本次用到的标签主要有：

| 标签 | 作用 |
| --- | --- |
| `<h1>` 至 `<h6>` | 标题 |
| `<p>` | 段落 |
| `<a>` | 超链接 |
| `<img>` | 图片 |
| `<ul>`、`<ol>` | 无序列表、有序列表 |
| `<div>` | 内容分区 |
| `<table>` | 表格，与 `<tr>`、`<th>`、`<td>` 配合 |

有两个细节值得注意：

1. **图片一定要写有意义的 `alt`。** 图片加载失败时，`alt` 是唯一能告诉用户这里原本是什么的信息。
2. **外链需要新标签页打开时，用 `target="_blank"` 的同时要加 `rel="noopener"`**，
   否则新页面可以通过 `window.opener` 反向操作原页面。

## 四、CSS：内容和表现分离

样式推荐放在独立文件里，用 `<link>` 关联：

```html
<link rel="stylesheet" href="css/style.css">
```

选择器决定样式作用在谁身上：

- `p` 是 HTML 标签选择器；
- `.note` 是 class 选择符，可以重复使用；
- `#main` 是 id 选择符，同一页面中应保持唯一。

为了让页面在手机上也正常阅读，我给主体内容加了最大宽度，并写了断点：

```css
body {
  max-width: 960px;
  margin: 0 auto;
  padding: 24px;
  line-height: 1.7;
}

img {
  max-width: 100%;
  height: auto;
}
```

`max-width: 100%` 这一条很关键，它保证图片在小屏幕上不会被撑破容器。

## 五、怎么查看网页的 HTTP 协议版本

这是实验里最有「计算机网络」味道的一步。方法是：

1. 打开已经发布出去的网页；
2. 按 `F12` 打开开发者工具；
3. 切到「网络」或 “Network” 面板；
4. 刷新网页；
5. 在表头区域启用「协议」或 “Protocol” 列；
6. 看主文档请求对应的协议值。

常见显示值：`http/1.1` 表示 HTTP/1.1，`h2` 表示 HTTP/2，`h3` 表示 HTTP/3。

各版本的主要区别：

| 版本 | 主要特点 |
| --- | --- |
| HTTP/1.0 | 默认短连接，每次请求后通常关闭 TCP 连接 |
| HTTP/1.1 | 默认持久连接，支持 Host、缓存控制、分块传输与范围请求 |
| HTTP/2 | 二进制分帧、多路复用、HPACK 头部压缩 |
| HTTP/3 | 基于 QUIC 与 UDP，支持连接迁移，减少队头阻塞影响 |

> ⚠️ 注意：用 `localhost` 本地预览时看到的协议**不能代表**公网访问的协议。
> 必须对发布后的网址检查，结果才有效。

## 六、小结

做完这一遍，最大的感受是：静态网页看起来简单，但要把结构、样式、资源分清楚，
把可访问性和移动端适配都考虑到，还是需要一些刻意的设计。

工具方面，我用了 [Hexo](https://hexo.io/zh-cn/docs/) 来管理文章和生成静态页面，
主题是 [Butterfly](https://butterfly.js.org/)，通过 GitHub Pages 对外发布。
按照实验说明，框架也可以完全自己手写——只要最终网页能被公开访问即可。

参考链接：

- [Hexo 官方文档](https://hexo.io/zh-cn/docs/)
- [GitHub Pages 文档](https://docs.github.com/zh/pages)
- [MDN HTML 指南](https://developer.mozilla.org/zh-CN/docs/Learn_web_development/Core/Structuring_content)
