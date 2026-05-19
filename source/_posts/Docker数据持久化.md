---
title: "Docker 数据持久化"
description: "Volume 和 Bind Mount 的使用场景与区别"
tags: [docker, volume, bind-mount, persistence]
categories: [Docker]
date: 2026-05-07
aliases: ["容器存储", "数据卷"]
---

# 第二阶段：Docker（容器引擎入门）·数据持久化

## Volume

- **概念**：Docker 管理的宿主机目录（默认位于 `/var/lib/docker/volumes/`），独立于容器生命周期。  
- **实现**：Volume 以目录形式存在，可通过 `docker volume` 命令创建和管理。挂载到容器时，实际上是 bind mount 到该目录。Volume 支持驱动程序（如本地、NFS、云存储）实现不同后端。  

## Bind Mount

- **概念**：将宿主机任意路径直接挂载到容器目录。  
- **实现**：通过 Linux 的 `mount` 系统调用将源路径（宿主机）绑定到目标路径（容器内 mount namespace）。依赖宿主机目录结构，不提供跨主机可移植性。  

## 区别本质

Volume 是 Docker 对 Bind Mount 的封装，增加了命名管理、驱动扩展和备份恢复接口；Bind Mount 是直接利用内核特性。
