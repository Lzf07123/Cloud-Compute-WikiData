---
title: "Docker 镜像管理"
description: "Dockerfile 编写、镜像分层构建、多阶段构建、标签管理"
tags: [docker, image, dockerfile, multi-stage-build]
categories: [Docker]
date: 2026-05-07
aliases: ["镜像构建最佳实践"]
---

# 第二阶段：Docker（容器引擎入门）·镜像管理

## 镜像分层构建

- **概念**：每个 Dockerfile 指令产生一个只读层，层之间通过 UnionFS 叠加形成统一文件系统。  
- **实现**：  
  - **UnionFS 类型**：OverlayFS（最常用）、AUFS、btrfs、devicemapper。  
  - **OverlayFS 原理**：使用 lowerdir（只读下层）、upperdir（可写上层）、merged（统一视图）。容器启动时镜像层作为 lowerdir，容器可写层作为 upperdir。  
- **写时复制**：修改文件时，将文件从下层复制到上层再修改，下层保持不变。  

## 多阶段构建

- **概念**：在一个 Dockerfile 中使用多个 `FROM` 指令，前阶段用于编译或构建，后阶段仅复制产物，避免将构建工具和中间文件带入最终镜像。  
- **实现**：每个阶段独立构建，最终镜像只包含最后阶段的层。Docker 构建器在阶段间传递文件（通过 `COPY --from=`）。  

## 镜像内容寻址

每个镜像层由哈希（SHA256）标识，相同内容（例如基础层）可跨镜像共享，节省存储和传输。

## 🔗 相关文档

[Docker 容器生命周期](Docker容器生命周期.md) | [Docker镜像仓库详解](Docker镜像仓库详解.md) | [Docker 实际应用：构建自定义镜像](Docker实际应用-构建自定义镜像.md)
