---
title: "OCI 标准概述"
description: "了解 Open Container Initiative 定义的运行时和镜像规范，Docker 和 Kubernetes 都遵循此标准"
tags: [oci, standard, runtime, image-spec]
categories: [容器基础]
date: 2026-05-07
aliases: ["开放容器标准"]
---

# 第一阶段：容器基础·OCI 标准概述

## OCI 标准

- **概念**：Open Container Initiative 制定的开放标准，确保容器运行时和镜像格式的互操作性。  
- **核心规范**：  
  - **镜像规范（image-spec）**：定义容器镜像的清单、索引、层文件系统及配置的 JSON 格式。镜像由一组只读层（tar 归档）和元数据组成，支持内容寻址存储。  
  - **运行时规范（runtime-spec）**：定义容器配置（如 rootfs 路径、挂载点、命名空间、cgroups 参数）和生命周期操作（创建、启动、停止、删除）。  
- **实现生态**：  
  - **runc**：OCI 的参考实现，基于 libcontainer，直接与内核交互创建容器。  
  - **containerd**：高级运行时，管理容器生命周期、镜像传输、存储及执行（调用 runc）。  
  - **CRI-O**：为 Kubernetes CRI 接口实现的轻量级运行时，直接运行 OCI 容器。  
  - 其他实现：gVisor（用户态内核）、Kata Containers（轻量虚拟机）。

## 🔗 相关文档

{% post_link Docker/Docker容器生命周期详解 %} | {% post_link 容器基础/容器核心概念 %}
