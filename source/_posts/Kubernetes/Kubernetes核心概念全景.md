---
title: "Kubernetes 核心概念全景"
description: "Pod（最小调度单位）、控制器（Deployment/StatefulSet/DaemonSet）、Service（服务发现与负载均衡）、Ingress（七层路由）"
tags: [kubernetes, pod, deployment, service, ingress, controller]
categories: [Kubernetes]
date: 2026-05-07
aliases: ["K8s基础对象"]
---

# 第四阶段：Kubernetes（容器编排）·核心概念全景

## Pod

- **概念**：一组共享存储和网络资源的容器，是 Kubernetes 的最小部署单元。  
- **实现**：Pod 内的容器共享同一个 netns（localhost 通信）、IPC namespace 和 volume。Pod 由 **kubelet** 通过调用容器运行时（如 containerd）创建，并设置 pause 容器（基础架构容器）持有 netns。  

## 控制器

### Deployment

- **概念**：管理无状态应用的期望副本数、滚动更新和回滚。  
- **实现**：Deployment 控制器在控制循环中管理 ReplicaSet（下一级控制器），每个 ReplicaSet 记录一个版本的 Pod 模板。滚动更新时创建新 ReplicaSet，逐步提升其副本数并缩减旧 ReplicaSet。  

### StatefulSet

- **概念**：管理有状态应用，提供稳定的网络标识（Pod 名称固定）和有序部署/伸缩。  
- **实现**：StatefulSet 控制器为每个 Pod 分配序号（0..N-1），并按顺序启停。PVC（持久卷声明）模板自动为每个 Pod 创建对应存储。  

### DaemonSet

- **概念**：确保每个节点（或符合条件的节点）运行一个 Pod 副本。  
- **实现**：DaemonSet 控制器监听节点添加/删除事件，使用 NodeAffinity 和 Pod 的 `spec.nodeName` 调度，由节点上的 kubelet 启动 Pod。  

## Service

- **概念**：为一组 Pod 提供稳定的虚拟 IP 和负载均衡。  
- **实现**：  
  - **ClusterIP**：集群内部虚拟 IP，通过 kube-proxy（iptables 或 IPVS 模式）实现转发。kube-proxy 在每个节点上监听 API Server，创建 iptables 规则，将 Service IP 的访问随机或轮询地指向后端 Pod IP。  
  - **NodePort**：在 ClusterIP 基础上，在每个节点打开一个固定端口（30000-32767），流量经 NodePort 转发到 Service。  
  - **LoadBalancer**：对接云厂商负载均衡器，自动创建外部负载均衡并将流量指向 NodePort。  

## Ingress

- **概念**：七层（HTTP/HTTPS）路由规则，将外部请求转发到集群内 Service。  
- **实现**：需要部署 **Ingress Controller**（如 Nginx Ingress Controller）。Controller 监听 Ingress 资源变化，动态更新自身配置文件（如 Nginx 的 `server` 块），并通过 Pod 的服务地址进行路由。

## 🔗 相关文档

{% post_link Kubernetes/Kubernetes调度与部署策略 %} | {% post_link Kubernetes/Kubernetes网络模型与实现 %} | {% post_link Kubernetes/Kubernetes存储抽象 %} | {% post_link Kubernetes/Kubernetes安全基础 %} | {% post_link Kubernetes/Kubernetes可观测性 %} | {% post_link 学习路线与课程/容器云学习路径 %}
