---
date: 2025-01-10 11:01:10
draft: false
title: 'Hello Blinko'
categories: ["Code"]
tags: 
  - Blinko
summary: "Memos 的 S3 储存太拉跨了，用群友的话就是作者的每次更新都是毁灭性的！进而了解到了 [Blinko](https://blinko.mintlify.app/) "
---
Memos 的 S3 储存太拉跨了，用群友的话就是作者的每次更新都是毁灭性的！进而了解到了 [Blinko](https://blinko.mintlify.app/) 。

> Blinko 是一个**创新的**开源项目，专为想要快速捕捉和组织转瞬即逝的想法的个人而设计。Blinko 允许用户在想法出现的那一刻无缝记下想法，确保不会丢失任何创意火花。

blinko 是老外写的一个开源笔记项目，**具体特点如下**：

- AI增强笔记检索
- 数据存储在本地，不容易丢失
- 支持Markdown编辑器和纯文档
- 代码开源可放心使用

![blinko](https://s3.xwoniu.com/blog/posts/c7d0d0fd54008961e82c310750175a36.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

Blinko 所有的公开**闪念**和**笔记**全部展示在 share 页面，例如：https://b.xwoniu.com/share

![Blinko](https://s3.xwoniu.com/blog/posts/bc2486a12dabe045281aaff57f935023.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

## 介绍

区别与 memos，blinko 拥有 blinko（闪念）以及 note（笔记）两种模式，可以互相切换。

- **目的**：专为快速捕获和临时信息而设计
- **主要特点**：
  - [自动存档](https://blinko.mintlify.app/settings/task#2-schedule-archive-blinko)功能
  - 非常适合临时提醒
  - 非常适合短格式内容
- **使用案例**：
  - 每日任务和提醒
  - 快速的想法和想法
  - 临时信息
  - 不需要长期存放的会议记录

- **目的**：为永久保留知识和详细文档而构建
- **主要特点**：
  - 富文本格式
  - 分层组织
  - 永久存储
- **使用案例**：
  - 项目文档
  - 研究结果
  - 学习资料
  - 重要参考文件

> **简单总结：**
>
> **闪念**：快速思考、临时信息、自动存档
>
> **笔记**：长篇内容、永久存储、知识库

## 部署

### 创建安装目录

```bash
sudo -i

mkdir -p /root/data/docker_data/blinko

cd /root/data/docker_data/blinko
```
接着我们来编辑下docker-compose.yml

```bash
vim docker-compose.yml
```

```bash
networks:
  blinko-network:
    driver: bridge

services:
  blinko-website:
    image: blinkospace/blinko:latest
    container_name: blinko-website
    environment:
      NODE_ENV: production
      NEXTAUTH_URL: http://localhost:1111
      NEXT_PUBLIC_BASE_URL: https://notes.gugu.ovh       #改成自己的域名
      NEXTAUTH_SECRET: uNG9%&Nce8z^Yev  #自己设置一个密码
      DATABASE_URL: postgresql://postgres:password@postgres:5432/postgres  #password改成自己的密码，和下方POSTGRES_PASSWORD的一样
    depends_on:
      postgres:
        condition: service_healthy
    # Make sure you have enough permissions.
    volumes:
      - ./blinko:/app/.blinko 
    restart: always
    logging:
      options:
        max-size: "10m"
        max-file: "3"
    ports:
      - 3000:1111     # 3000可以自己修改成没有用过的端口
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:1111/"]
      interval: 30s 
      timeout: 10s   
      retries: 5     
      start_period: 30s 
    networks:
      - blinko-network

  postgres:
    image: postgres:14
    container_name: blinko-postgres
    restart: always
    ports:
      - 5432
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password     #记得改一个密码
      TZ: Asia/Shanghai
    healthcheck:
      test:
        ["CMD", "pg_isready", "-U", "postgres", "-d", "postgres"]
      interval: 5s
      timeout: 10s
      retries: 5
    networks:
      - blinko-network

```
- `NEXTAUTH_URL`：指定应用程序的基本 URL，通常是已部署网站的根 URL，用于身份验证回调和重定向，一般保持默认http://localhost:1111即可

- `NEXT_PUBLIC_BASE_URL`：定义应用程序的公共基础 URL，用作前端和 API 请求的基础路径。一般改成自己的最后访问的域名即可

- `NEXTAUTH_SECRET`：用于加密会话和身份验证令牌的秘密密钥，确保用户数据安全。自己设置一个密码

- `DATABASE_URL`：用于连接和访问 blinko 数据库的数据库连接URL

  同样，修改完成之后，可以在英文输入法下，按 `i` 修改，完成之后，按一下 `esc`，然后 `:wq` 保存退出

### 查看端口是否被占用

查看端口是否被占用（以 3000 为例）：

```bash
lsof -i:3000  #查看 3000 端口是否被占用，如果被占用，重新自定义一个端口
```
如果啥也没出现，表示端口未被占用，我们可以继续下面的操作了～

如果出现：

```bash
bash: lsof: command not found
```

运行：

```bash
apt install lsof  #安装 lsof
```
如果端口没有被占用（被占用了就修改一下端口，比如改成 8381，注意 docker 命令行里和防火墙都要改

### 启动 blinko

```bash
cd /root/data/docker_data/blinko

docker compose up -d   # 注意，老版本用户用 docker-compose up -d
```
等待拉取好镜像，出现 `done` 的字样之后，

理论上我们就可以输入 http://ip:3000 访问了。

## 使用教程

### AI

1. 注册 [硅基流动](https://cloud.siliconflow.cn/i/xZWCeGsV)，在 API 秘钥中 **API 密钥** 
2. 启用 Blinko ai，
3. AI 服务商：选择 OpenAI；
   人工智能模型：填写 `Qwen/Qwen2.5-7B-Instruct`
   嵌入式模型：填写 `BAAI/bge-m3`
   API Key：为硅基流动 API 密钥；
   接口地址：https://api.siliconflow.cn/v1

### 导入

blinko 支持从 memos 导入数据，这就很方便了。

![blinko](https://s3.xwoniu.com/blog/posts/a83d18aa7a50a862926c3b8360ec48aa.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

从本地导入前请把 **存储** 改为本地。

![blinko](https://s3.xwoniu.com/blog/posts/bd9ec4585191940b627f10c45107b049.webp?mark=blog/Xwoniu-LOGO.png&mark-pos=0.96,0.96&mark-pct=0.2&mark-alpha=0.5)

### 更新 Blinko

这个项目后续应该也会有更新，所以提供一个更新的方式：

```bash
cd /root/data/docker_data/blinko

docker compose pull

docker compose up -d    # 请不要使用 docker compose stop 来停止容器，因为这么做需要额外的时间等待容器停止；docker compose up -d 直接升级容器时会自动停止并立刻重建新的容器，完全没有必要浪费那些时间。

docker image prune  # prune 命令用来删除不再使用的 docker 对象。删除所有未被 tag 标记和未被容器使用的镜像
```

提示：

```bash
WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N]
```

输入 `y` 并回车

### 卸载 Blinko

同样进入安装页面，先停止所有容器：

```bash
cd /root/data/docker_data/blinko

docker compose down

cd ..

rm -rf /root/data/docker_data/blinko  # 完全删除
```

可以卸载的很干净了！

## 参阅

- [Blinko Doc](https://blinko.mintlify.app/introduction)
- https://github.com/blinko-space/blinko.
- https://blog.laoda.de/archives/docker-compose-install-blinko
