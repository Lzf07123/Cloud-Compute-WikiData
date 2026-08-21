---
title: "Kubernetes 调度与部署策略"
description: "NodeSelector、亲和性反亲和性、污点与容忍；滚动更新、回滚、HPA（水平自动扩缩）"
tags: [kubernetes, scheduling, affinity, toleration, rolling-update, hpa]
categories: [Kubernetes]
date: 2026-05-07
aliases: ["K8s调度策略", "自动扩缩"]
---

# 第四阶段：Kubernetes（容器编排）·调度与部署策略

## 调度机制

### NodeSelector

简单节点选择，Pod 的 `.spec.nodeSelector` 匹配节点标签。  

### 亲和性与反亲和性

- **概念**：用更复杂的表达式（In/NotIn/Exists）定义软/硬约束，如 Pod 间亲和（同一拓扑域）或反亲和（互斥）。  
- **实现**：调度器在预选阶段根据节点标签及已有 Pod 拓扑信息过滤节点，在优选阶段打分。  

### 污点与容忍

- **概念**：节点可标记污点（Key=Value:Effect），Effect 包括 NoSchedule（不调度）、PreferNoSchedule（尽量不调）、NoExecute（驱逐已有 Pod）。Pod 通过 tolerations 声明可容忍的污点。  
- **实现**：调度器检查节点污点与 Pod 容忍度是否匹配；NoExecute 污点添加后，taint manager 会驱逐不匹配的 Pod。  

## 部署策略

### 滚动更新

- **概念**：逐步用新版本 Pod 替换旧版本，保证服务不中断。  
- **实现**：Deployment 控制器通过 maxSurge 和 maxUnavailable 参数控制更新速率，创建新 ReplicaSet 并调整副本比例。  

### 回滚

Deployment 保留历史 ReplicaSet，回滚时选择目标版本 ReplicaSet 并反向调整副本数。  

### HPA（水平自动扩缩）

- **概念**：根据 CPU、内存或自定义指标动态调整 Deployment/ReplicaSet 副本数。  
- **实现**：HPA 控制器定期（默认 15 秒）从 Metrics Server（或自定义 API）获取指标，计算所需副本数（公式：期望副本数 = 当前副本数 × (当前指标值/期望指标值)），并更新 scale 子资源。

## 🔗 相关文档

[Kubernetes 核心概念全景](Kubernetes核心概念全景.md) | [Kubernetes 网络模型与实现](Kubernetes网络模型与实现.md) | [Kubernetes 存储抽象](Kubernetes存储抽象.md) | [Kubernetes 配置与密文](Kubernetes配置与密文.md) | [Kubernetes 资源管理](Kubernetes资源管理.md)
