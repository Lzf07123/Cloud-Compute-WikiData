---
title: "Docker镜像仓库详解"
description: "详细介绍Docker Hub和Harbor私有仓库的推送拉取操作，以及轩辕镜像服务的使用方法和版本差异"
tags: [Docker, Harbor, 轩辕镜像, 容器仓库, 镜像管理]
categories: [容器技术, DevOps]
date: 2026-05-13
aliases: ["Docker仓库操作", "Harbor使用指南", "轩辕镜像配置"]
---

# Docker镜像仓库详解

### Docker Hub推送与拉取操作

**登录Docker Hub**

```bash
docker login
```

输入用户名和密码完成登录
**推送镜像到Docker Hub**

```bash
docker tag local-image-name username/repository:tag
docker push username/repository:tag
```

**从Docker Hub拉取镜像**

```bash
docker pull username/repository:tag
```

### Harbor私有仓库推送与拉取操作

**配置Docker支持私有仓库**
在`/etc/docker/daemon.json`中添加`insecure-registries`配置，然后重启Docker服务
**登录Harbor**

```bash
docker login -u admin -p password harbor地址:port
```

**推送镜像到Harbor**

```bash
docker tag 镜像ID harbor地址:port/项目名/镜像名:版本
docker push harbor地址:port/项目名/镜像名:版本
```

**从Harbor拉取镜像**

```bash
docker pull harbor地址:port/项目名/镜像名:版本
```

### 轩辕镜像服务

**服务概述**
轩辕镜像专注公共仓库场景，整合Docker Hub、Google Container Registry、Kubernetes官方仓库等主流镜像源
**免费版使用方法**

- 配置`registry-mirrors`指向`docker.xuanyuan.me`
- 或直接使用：`docker pull docker.xuanyuan.me/镜像名:tag`
- 仅支持Docker Hub镜像拉取
  **专业版功能**
- 提供专属域名拉取方式：`docker pull ***.xuanyuan.run/镜像名:tag`
- 支持7+个主流镜像仓库，包括：
  - Docker Hub
  - Google Container Registry (GCR)
  - GitHub Container Registry (GHCR)
  - Kubernetes仓库
  - NVIDIA
  - Quay
  - Microsoft
  - Elastic
    等仓库
