---
date: 2024-06-01 11:47:06
draft: false
title: 'Visual Studio Code 配置'
categories: ["Code"]
tags: 
  - Visual Studio Cod
summary: "作为目前主力开发以及编辑工具，VSCode每天都在使用，下面列举本人为了方便开发而使用安装的一些插件以及美化。"
---
作为目前主力开发以及编辑工具，VSCode每天都在使用，下面列举本人为了方便开发而使用安装的一些插件以及美化。

## 插件

### [Git Emoji Commit 中文版](https://marketplace.visualstudio.com/items?itemName=maixiaojie.git-emoji-zh)

在现代软件开发流程中，Git 已成为版本控制的主流工具，而良好的 Git 提交消息规范不仅能提升代码库的可读性，还能加强团队间的协作效率。Git Git Emoji Commit 中文版是一个专为中文开发者设计的 Git 提交信息增强工具，它提供了一套清晰易懂、富有表现力的 emoji 算法，以助于提升你的 Git 提交体验。

**BUG解决**

该软件目前有个BUG，选中 emoji 后输入框无法显示。

解决方案（Windows)：

1. 打开文件夹：`C:\Users\用户名\.vscode\extensions`，进入带插件名以及版本号的文件夹；
2. 打开并编辑`./out/extension.js`文件第23行左右；
3. 替换下面代码保存并重启 VSCode 即可：

查找：

```bash
 return repository.rootUri.path === uri._rootUri.path;
```

替换：

```bash
 return repository.rootUri.path === uri.rootUri.path;
```

### [CodeSnap](https://marketplace.visualstudio.com/items?itemName=adpyke.codesnap)

 # DSM7.1 AME版本3.0.1-2004curl -L http://code.imnks.com/ame3patch/ame71-2004.py | python# DSM7.2 AME版本3.1.0-3005curl -L http://code.imnks.com/ame3patch/ame72-3005.py | pythonbash

### [Git Graph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)

查看存储库的Git图，并从图中轻松执行Git操作。可配置为您想要的样子！

### [One Dark Pro](https://marketplace.visualstudio.com/items?itemName=zhuangtongfa.Material-theme)

一个好看的主题
