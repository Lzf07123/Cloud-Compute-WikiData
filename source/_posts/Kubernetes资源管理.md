---
title: "Kubernetes 资源管理"
description: "声明式 YAML、kubectl 常用命令、Namespace 隔离"
tags: [kubernetes, resource, yaml, kubectl, namespace]
categories: [Kubernetes]
date: 2026-05-07
aliases: ["K8s资源操作"]
---

# 第四阶段：Kubernetes（容器编排）·资源管理

## 声明式 YAML

- **概念**：通过 YAML 或 JSON 文件描述资源的期望状态，Kubernetes 持续调谐实际状态以匹配期望。  
- **实现**：API Server 接收并验证 YAML，将其存储到 etcd。各控制器从 API Server Watch 资源变化，执行调谐逻辑。  

## kubectl 与 API

- **概念**：命令行工具通过 API Server 的 RESTful 接口进行 CRUD 操作。  
- **实现**：kubectl 读取 kubeconfig 获取集群 API 地址和认证凭据，发送 HTTP 请求（JSON 或 Protobuf）。  

## Namespace

- **概念**：逻辑隔离的多租户环境，资源名称在同一 Namespace 内唯一。  
- **实现**：API Server 在处理请求时根据 Namespace 字段过滤资源，etcd 中的资源 key 包含 Namespace 前缀。RBAC 和 NetworkPolicy 可基于 Namespace 做访问控制。  
