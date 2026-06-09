---
title: "Kubernetes 安全基础"
description: "RBAC（角色/ServiceAccount/ClusterRole）、PodSecurityPolicy（或 Pod Security Admission）、NetworkPolicy"
tags: [kubernetes, security, rbac, pod-security, networkpolicy]
categories: [Kubernetes]
date: 2026-05-07
aliases: ["K8s安全机制"]
---

# 第四阶段：Kubernetes（容器编排）·安全机制

## RBAC（基于角色的访问控制）

- **概念**：通过 Role（namespace 范围）或 ClusterRole（集群范围）定义权限（verbs + resources），再通过 RoleBinding/ClusterRoleBinding 将角色授予用户、Group 或 ServiceAccount。  
- **实现**：API Server 在请求处理前检查访问者的身份（认证阶段）和授权（RBAC 鉴权模块），查看是否有匹配的 RoleBinding。  

## ServiceAccount

- **概念**：Pod 使用的身份标识，对应一个 Secret（Token）挂载到 Pod 内。  
- **实现**：Pod 在创建时自动挂载 ServiceAccount 的 JWT Token，Pod 内应用可使用该 Token 访问 API Server。  

## PodSecurityPolicy（已废弃）及其替代 Pod Security Admission

- **概念**：Pod 运行时安全策略，如禁止特权容器、限制 root 权限、只读根文件系统等。  
- **实现**：PSA 利用命名空间标签（`pod-security.kubernetes.io/enforce`）设置三个级别，API Server 在 Pod 创建或更新时根据策略校验 Pod Spec。  

## NetworkPolicy

- **概念**：Pod 之间或 Pod 与外部的网络访问控制规则。  
- **实现**：需要网络插件支持（Calico、Cilium、Weave 等）。插件根据 NetworkPolicy 资源生成 iptables 规则或 eBPF 程序，在节点上根据 Pod 标签和 ipBlock 过滤流量。

## 🔗 相关文档

{% post_link Kubernetes/Kubernetes核心概念全景 %} | {% post_link Kubernetes/Kubernetes网络模型与实现 %} | {% post_link Kubernetes/Kubernetes配置与密文 %}
