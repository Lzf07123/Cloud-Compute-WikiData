---
title: "Kubernetes 配置与密文"
description: "ConfigMap 和 Secret 的注入（环境变量/Volume）"
tags: [kubernetes, configmap, secret, configuration]
categories: [Kubernetes]
date: 2026-05-07
aliases: ["K8s配置管理"]
---

# 第四阶段：Kubernetes（容器编排）·配置与密文

## ConfigMap

- **概念**：存储非敏感的配置数据（键值对或文件）。  
- **实现**：ConfigMap 作为 etcd 中的资源，Pod 可通过环境变量（将数据注入进程环境）或 Volume（挂载为文件或目录）方式获取配置。当 Volume 方式使用时，kubelet 会自动同步更新内容。  

## Secret

- **概念**：存储敏感数据（如密码、Token、SSH 密钥），默认 Base64 编码，支持加密存储。  
- **实现**：Secret 在 etcd 中可开启静态加密（通过 kube-apiserver 的 encryption provider）。Pod 挂载 Secret 为 Volume 时会创建 tmpfs（内存文件系统），避免写入磁盘。

## 🔗 相关文档

[Kubernetes 调度与部署策略](Kubernetes调度与部署策略.md) | [Kubernetes 存储抽象](Kubernetes存储抽象.md) | [Kubernetes 安全基础](Kubernetes安全基础.md)
