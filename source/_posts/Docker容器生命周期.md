---
title: "Docker 容器生命周期"
description: "run/start/stop/rm、进入容器、日志查看、资源限制（--cpu/--memory）"
tags: [docker, container, lifecycle, resource-limit]
categories: [Docker]
date: 2026-05-07
aliases: ["容器状态管理"]
---

# 第二阶段：Docker（容器引擎入门）·容器生命周期管理

## 生命周期状态

已创建、运行中、已暂停、已停止、已删除。  

## 实现机制

- **创建**：准备 rootfs、配置 Namespace 和 Cgroups，但不启动进程。  
- **启动**：通过 runc 执行容器进程（通常是 init 进程），设置网络和挂载点。  
- **运行**：容器进程在隔离环境中运行，守护进程通过 `wait` 或 `ptrace` 监控状态。  
- **停止**：发送 SIGTERM 信号，等待进程退出，若超时则 SIGKILL。然后清理 Namespace 和 Cgroups。  
- **资源限制**：通过 Cgroups 在容器启动前或运行时动态设置 CPU、内存、IO 配额。
