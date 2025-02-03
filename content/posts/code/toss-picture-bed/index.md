---
date: 2025-01-15 10:28:06
draft: false
title: '折腾图床的心酸历程'
categories: ["Daily"]
tags: 
  - 图床
summary: "从刚开始接触 Hexo 的时候，就开始使用 ` 图床` 来存放博客图片，以提高访问速度，中间折腾过不少解决方案，也遇到了很多坑。本文用来记录一下！"
---
从刚开始接触 Hexo 的时候，就开始使用 ` 图床` 来存放博客图片，以提高访问速度，中间折腾过不少解决方案，也遇到了很多坑。本文用来记录一下！

## 图床

### Cloudflare R2

鼎鼎大名的 Cloudflare R2 确在我这里出现了问题：

- 使用自定义域名无法访问（需要挂梯子）
- 移动端蜂窝数据下可以访问，连接 WIFI 也无法访问

包括通过 Cloudflare Pages 托管的 Hexo 也出现了上述的问题，关闭域名解析的**代理**图标后解决了。

### Bitiful

后来发现了缤纷云高性能对象存储 + CDN。实名认证后有50 GB 的存储空间以及30 GB 的 HTTPS流量。

自带的媒体处理功能 CoreIX，可以通过在 Url 末尾（或在 GetObject 接口中）携带图片处理的相关参数的方式使用。

## 解决方案：

一直使用 Typora 来写作文章，大概流程是：

![写作流程](https://s3.xwoniu.com/blog/posts/c53560b8a525c298997cd6186134010b.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

### 压缩

使用的 Piclist 自带的功能，可以实现图片格式转化、压缩、水印等功能：

![Piclist](https://s3.xwoniu.com/blog/posts/e57f37c62be80f1563637a8ed80a1bc3.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

Piclist 水印功能只能调取本地图片，切无法实现位置、模糊度等功能

### Rename

博客文章大多来自手机、网盘、截图或者网络，图片名称鱼龙混杂，作为强迫症患者，图片上传到对象存储前必须做一波**重命名**。

刚开始使用的 uTools 的批量重命名插件：

![uTools Rename](https://s3.xwoniu.com/blog/posts/608893043183874020c5755bc8357a39.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

但是发生了一件离奇的事情，就是这个 AI 重命名随机字符竟然会重复，导致上传到存储对象的同文件夹的图片被覆盖了，幸亏发现的早。

### 上传

同**压缩**，使用 Typora + Piclist 来实现图的自动上传，但是 Piclist 自带的 AWS S3 无法实现在图片 Url 末尾添加图片处理的相关参数，一直在手动实现。

后来发现了 [picgo-plugin-s3](https://github.com/wayjam/picgo-plugin-s3) 插件，通过 `urlSuffix` 参数完美的实现了这一点。

同时发现了 [picgo-plugin-squoosh](https://github.com/JolyneAnasui/picgo-plugin-squoosh) 插件，除了压缩功能外，还支持使用图片 `MD5` 来重命名，这就很方便了。

