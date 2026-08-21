---
title: "Docker Compose 多容器编排入门"
description: "使用 Docker Compose 定义多服务（如 Web + Redis），了解服务依赖与网络互通"
tags: [docker, compose, orchestration, multi-container]
categories: [Docker]
date: 2026-05-07
aliases: ["Compose文件", "服务编排"]
---

# 第二阶段：Docker（容器引擎入门）·多容器编排

## 多容器编排入门

- **概念**：通过声明式 YAML 文件定义多容器应用的服务、网络、卷等依赖关系，单命令实现整体启动和停止。  
- **实现**：  
  - Compose 解析 YAML 文件，生成每个服务的容器配置。  
  - 自动创建项目专属的桥接网络，服务名解析为容器 IP（通过内部 DNS）。  
  - 依赖关系（`depends_on`）仅控制启动顺序，不保证健康。  
  - Compose v3 支持定义多种部署模式（swarm 模式除外，单纯 Compose 是单机编排）。

## 🔗 相关文档

[Docker 网络模型](Docker网络模型.md) | [Docker 数据持久化](Docker数据持久化.md) | [Docker多容器编排详解](Docker多容器编排详解.md)
