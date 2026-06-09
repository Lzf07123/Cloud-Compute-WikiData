---
title: "Docker镜像操作详解"
description: "全面介绍Docker镜像的获取、管理、构建、导入导出等操作命令，包含实用示例和最佳实践指南"
tags: [Docker, 镜像管理, 容器化, DevOps]
categories: [技术文档]
date: 2026-05-13
aliases: ["Docker镜像命令", "容器镜像管理"]
---

# Docker镜像管理详解

Docker 镜像是容器的只读模板，包含应用及其运行环境。本文涵盖常用镜像操作命令、示例和最佳实践。

## 一、获取镜像

### 1. `docker pull` – 拉取镜像

```bash
docker pull [选项] 镜像名[:标签]
```

- 默认标签为 `latest`
- 示例：  
  `docker pull ubuntu:20.04`  
  `docker pull nginx:1.25`

### 2. `docker push` – 推送镜像

```bash
docker push 用户名/镜像名:标签
```

- 需先登录仓库（`docker login`）并为镜像打上正确标签
- 示例：`docker push myrepo/hello:v1`

## 二、管理本地镜像

### 3. `docker images` (或 `docker image ls`) – 列出镜像

```bash
docker images [选项]
```

- `-a`：显示中间层镜像
- `-q`：仅显示镜像 ID
- `--digests`：显示摘要

### 4. `docker rmi` – 删除镜像

```bash
docker rmi [选项] 镜像名[:标签|镜像ID]
```

- `-f`：强制删除（有容器依赖时）
- 清理悬挂镜像：`docker image prune`

### 5. `docker tag` – 打标签

```bash
docker tag 源镜像[:标签] 目标镜像[:标签]
```

- 为镜像添加新名称/仓库路径，不复制内容
- 示例：`docker tag nginx:latest myregistry.com/mynginx:v1`

## 三、构建镜像

### 6. `docker build` – 通过 Dockerfile 构建

```bash
docker build [选项] 路径
```

- `-t 镜像名:标签`：指定名称
- `-f Dockerfile`：指定文件（默认当前目录下 Dockerfile）
- `--no-cache`：忽略缓存
- 示例：`docker build -t myapp:v1 .`

### 7. `docker commit` – 从容器创建镜像（临时调试，不推荐生产）

```bash
docker commit 容器名 新镜像名:标签
```

## 四、镜像导入/导出与文件系统转换

### 8. `docker save` – 保存镜像为 tar 文件（保留层历史）

```bash
docker save -o 文件名.tar 镜像名:标签
```

示例：`docker save -o ubuntu.tar ubuntu:20.04`

### 9. `docker load` – 从 tar 文件加载镜像

```bash
docker load -i 文件名.tar
```

### 10. `docker export` / `docker import` – 容器快照转为镜像

- `docker export 容器名 > container.tar`：导出容器文件系统（丢失历史层）
- `docker import container.tar 新镜像名:标签`：导入为镜像（单层）
  > `save/load` 保留完整分层结构，`export/import` 生成扁平镜像，体积更小但无法追溯构建历史。

## 五、查看镜像详情

### 11. `docker inspect` – 显示镜像元数据（JSON）

```bash
docker inspect 镜像名:标签
```

包含架构、环境变量、开放端口、入口点等。

### 12. `docker history` – 查看镜像构建历史层

```bash
docker history 镜像名:标签
```

每层的创建命令与大小，有助于优化镜像。

## 六、清理与空间回收

### 13. `docker image prune` – 删除未使用的镜像

- `-a`：删除所有不被容器引用的镜像
- `-f`：跳过确认  
  示例：`docker image prune -a`

### 14. `docker system df` – 查看磁盘占用（镜像、容器、卷）

## 典型工作流示例

```bash
# 拉取基础镜像
docker pull node:18-alpine

# 构建自定义镜像
docker build -t mynodeapp:1.0 .

# 标记以便推送到仓库
docker tag mynodeapp:1.0 mydockerhub/mynodeapp:1.0

# 登录并推送
docker login
docker push mydockerhub/mynodeapp:1.0

# 另一台机器拉取并运行
docker pull mydockerhub/mynodeapp:1.0

# 导出离线包
docker save -o app.tar mynodeapp:1.0
# 加载离线包
docker load -i app.tar
```

## 安全与最佳实践

- 避免使用 `latest` 标签，指定具体版本号
- 删除镜像前确认没有依赖容器（包括停止的容器），必要时加 `-f`
- 生产环境推荐使用 `Dockerfile` + `docker build`，而非 `docker commit`
- 定期运行 `docker image prune` 清理无用镜像

## 🔗 相关文档

{% post_link Docker/Docker容器生命周期详解 %} | {% post_link Docker/Docker镜像仓库详解 %} | {% post_link Docker/Docker实际应用-构建自定义镜像 %}
