---
title: "Docker 架构解析"
description: "Docker 客户端、守护进程、镜像、容器、仓库之间的关系"
tags: [docker, architecture, client, daemon]
categories: [Docker]
date: 2026-05-07
aliases: ["Docker组件交互"]
---

# 第二阶段：Docker（容器引擎入门）·Docker 架构解析

## Docker 架构

- **概念**：Docker 采用 C/S 架构，各组件相互协作。  
- **组件作用与实现**：  
  - **客户端（CLI）**：接收用户指令，通过 REST API 与守护进程通信。  
  - **守护进程**：核心后台进程，管理镜像、容器、网络、卷。监听 REST API，处理请求并调用 containerd。  
  - **containerd**：容器运行时管理器，负责镜像传输、存储、容器执行及生命周期管理，通过 gRPC 与 dockerd 交互。  
  - **runc**：OCI 底层运行时，实际创建 Namespace 和 Cgroups 并启动容器进程。  
  - **镜像（Image）**：只读模板，采用分层 UnionFS 存储。  
  - **容器（Container）**：镜像的运行实例，在镜像层之上增加可写层（容器层）。  
  - **仓库（Registry）**：存储和分发镜像的服务端（如 Docker Hub）。

## 🔗 相关文档

{% post_link Docker/Docker容器生命周期 %} | {% post_link Docker/Docker网络模型 %} | {% post_link Docker/Docker数据持久化 %} | {% post_link 容器基础/容器核心概念 %} | {% post_link 容器基础/容器镜像仓库 %} | {% post_link 容器基础/OCI标准概述 %} | {% post_link 学习路线与课程/容器云学习路径 %}
