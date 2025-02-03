---
date: 2025-01-21 13:03:06
draft: false
title: '白嫖  Gemini'
slug: "free-use-aistudio"
categories: ["Code"]
tags: 
  - Gemini
summary: "谷歌 Gemini 是所有一线顶级大模型当中唯一一个 API 可以免费白嫖的。本期视频，我们借助互联网大善人 Cloudflare 来中转一下 Gemini API，这样就得到一个国内免费爽用的顶级大模型。有了 API 以后，我们可以进行 AI 编程，可以聊天，可以音视频通话，做各种各样好玩的事情。"
---
谷歌 Gemini 是所有一线顶级大模型当中唯一一个 API 可以免费白嫖的。本期视频，我们借助互联网大善人 Cloudflare 来中转一下 Gemini API，这样就得到一个国内免费爽用的顶级大模型。有了 API 以后，我们可以进行 AI 编程，可以聊天，可以音视频通话，做各种各样好玩的事情。

## 获取 Cloudflare API

1. 登录 Cloudflare ，左侧菜单选择 **Workers 和 Pages**，右侧我们可以看到 **账户 ID **；
   ![![](https://s3.xwoniu.com/blog/posts/bcc985c4ff32229472ec2cb4e00d5884.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)](https://s3.xwoniu.com/blog/posts/bcc985c4ff32229472ec2cb4e00d5884.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)
2. 左侧菜单选择 **管理账户** - **账户 API 令牌**，点击 **创建令牌** （账户资源选择所有账户，选择包含所有区域）
   ![创建令牌](https://s3.xwoniu.com/blog/posts/863050d9da97c786dbba277cba75d2ac.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

## Chat API

Gemini 转OpenAI格式： https://github.com/PublicAffairs/openai-gemini

它的作用是把Gemini的API转换成OpenAI的API格式。我们可以看到，这里提供了一个部署到Cloudflare worker上面的方式。

部署到Cloudflare worker以后，它事实上达成了两个效果：

1. 把API中转到了国内
2. 把Gemini的API转换成更为通用的openai的格式

### 部署

点击 **Deploy with Worker** 这个按钮；

![openai-gemini](https://s3.xwoniu.com/blog/posts/d122101e2a1d291c1f05c23a368bd925.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

填写 **Account ID** 以及 **API Token** 后点击 **Connect Account** 来连接账户

![Connect Account](https://s3.xwoniu.com/blog/posts/0eae7ce25aff728168f2b17e4ea99551.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

点击 **Fork repository** 

![Fork repository](https://s3.xwoniu.com/blog/posts/f9f187aa4393162f2ffe1f61b5257055.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

然后我们需要开启fork出来项目的 Github action 功能

![Github action](https://s3.xwoniu.com/blog/posts/251a7d7bdac9a2d792a3f315e7473db7.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

后我们回到刚才的页面，点击 **workflow enabled**，最后点击 **deploy**。等待 worker 部署完成。

部署完成后我们绑定域名以便后续使用

![image-20250121132159313](https://s3.xwoniu.com/blog/posts/bd19dae6cd72dbb41864ac49bd482111.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

## Gemini API 申请

首先我们需要一个科学上网的环境。

打开：https://aistudio.google.com/ 并登录 Google 账号；

点击左上角 **Get API key** 

![Get API key](https://s3.xwoniu.com/blog/posts/b23abacaae884a93007ef85d31d2a726.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

然后随便填上一个项目，点击**创建**，这样我们就拿到了API密钥。

## API 实战

接下来我们测试下

### Chatbox

ChatBox：https://github.com/Bin-Huang/chatbox

下载安装好后，点击**设置** ，模型选择 **OPENAI api**

**API密钥** 我们还是填写谷歌的 Gemini API Key

**API 域名** 填写我们刚才绑定自己的域名地址。

![ChatBox](https://s3.xwoniu.com/blog/posts/aae4e3e1d29abfcb78f653b91fa51f3a.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

然后我们在下面选择自定义模型，然后填一个模型的名字，这里我填 gemini-2.0-flash-exp，最后点击保存

![ChatBox](https://s3.xwoniu.com/blog/posts/5c4871a45af86b0d33d67c923fef807b.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

### Cursor

[cursor](https://www.cursor.com/) 应该是目前最多人使用的AI编码工具

#### 设置中文

跟 VS code 一样，我们在使用快捷键 Ctrl + Shift + X 在左侧应用商店搜索 `chinese` 安装即可！

