---
date: 2025-01-08 21:01:49
draft: false
title: '黑群晖配置'
slug: "Synology-Configuration"
categories: ["Code"]
tags: 
  - Synology
summary: "记录一些自己折腾群晖 NAS 的过程！"
---
记录一些自己折腾群晖 NAS 的过程！

## 基础配置

### 套件源

**套件中心** - **设置** - **套件来源** - **新增** ：

```
https://spk7.imnks.com/
```
<p class="note note-info">DSM6.x套件源已于 2024.3.15 停止服务！</p>

### 启动 SSH 功能

![启动 SSH 功能](https://s3.xwoniu.com/blog/posts/b7bfaf3b7cd97f3e5b9f019da0baf798.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

## 洗白

在某宝购买了黑群晖洗白码，包含了 SN + MAC 码，NAS 关机的情况下，拔出启动U盘，插入电脑！

打开并编辑 `user-config` 文件，填入对应的 SN + MAC 码，U盘插入 NAS，开机即可！

## Advanced Media Extensions

先卸载 Advanced Media Extensions ，然后关机（不是重启），几分钟后再开机并重新安装 Advanced Media Extensions 。

1. 使用 Putty 或者其他 SSH 工具登录 NAS：

   ```bash
   ssh root@192.168.1.110 # 连接 NAS 
   sudo -i # 获取临时 root
   ```

2. 根据自己的 DSM 版本选择下列命令并执行：

   ```bash
   # DSM7.1 AME版本3.0.1-2004
   curl -L http://code.imnks.com/ame3patch/ame71-2004.py | python
   # DSM7.2 AME版本3.1.0-3005
   curl -L http://code.imnks.com/ame3patch/ame72-3005.py | python
   ```
