---
title: "Kubernetes 存储抽象"
description: "PV（持久卷）、PVC（声明）、StorageClass 动态供应"
tags: [kubernetes, pv, pvc, storageclass, storage]
categories: [Kubernetes]
date: 2026-05-07
aliases: ["K8s持久化存储"]
---

# 第四阶段：Kubernetes（容器编排）·存储抽象

## PV（PersistentVolume）

- **概念**：集群级别的存储资源抽象，由管理员预先提供或通过 StorageClass 动态创建。  
- **实现**：PV 代表某种后端存储（NFS、Ceph、云盘），包含容量、访问模式和回收策略。  

## PVC（PersistentVolumeClaim）

- **概念**：用户对存储资源的请求，Pod 通过 PVC 使用存储。  
- **实现**：PVC 指定所需容量和访问模式，PV 控制器会寻找匹配的 PV 并绑定。绑定后 PVC 成为 PV 的唯一所有者。  

## StorageClass

- **概念**：动态供应存储的模板，定义卷插件类型及参数。  
- **实现**：当 Pod 引用的 PVC 未绑定到现有 PV 且 StorageClass 有 provisioner（如 `kubernetes.io/aws-ebs`），provisioner 会自动创建新存储资源并生成 PV。

## 🔗 相关文档

[Kubernetes 核心概念全景](Kubernetes核心概念全景.md) | [Kubernetes 调度与部署策略](Kubernetes调度与部署策略.md) | [Kubernetes 配置与密文](Kubernetes配置与密文.md)
