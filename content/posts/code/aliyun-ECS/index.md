---
date: 2025-01-16 10:35:55
draft: false
title: 'Aliyun ECS'
slug: "win11-apps"
categories: ["Code"]
tags: 
  - Aliyun
  - ECS
summary: "昨天晚上打算部署一个新的服务时，阿里云 ECS 突然不能访问了，SSH 也无法连接，VNC 也无法连接。今天上午联系客服才知道内存拉爆了！"
---
昨天晚上打算部署一个新的服务时，阿里云 ECS 突然不能访问了，SSH 也无法连接，VNC 也无法连接。今天上午联系客服才知道内存拉爆了！

这台 ECS 安装了 1Pane 面板，其他服务都是通过 Docker 安装的，所以关闭了 Docker 后，重启服务器，解决了！

![ECS AI 助手](https://s3.xwoniu.com/blog/posts/8319b526d5ec050e39c178ea2025c6f9.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

登录 1Panel 后关闭了一些服务，看了下内存使用情况：

![内存使用情况](https://s3.xwoniu.com/blog/posts/01f572e0185f31c7024de0ff7437ec76.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

**总结** ：ECS 这种轻量服务器不要同时安装数据库，最好再备一台服务器，专门用来当数据库
