---
title: "Kubernetes 可观测性"
description: "资源监控、日志收集（如 EFK）、常见排查命令"
tags: [kubernetes, observability, metrics, logging, troubleshooting]
categories: [Kubernetes]
date: 2026-05-07
aliases: ["K8s监控日志"]
---

# 第四阶段：Kubernetes（容器编排）·可观测性

## Metrics Server

- **概念**：采集节点和 Pod 的 CPU、内存等资源指标。  
- **实现**：通过 kubelet 的 cAdvisor 接口（汇总 cgroups 数据）获取指标，暴露给 HPA、`kubectl top` 等。  

## 日志收集

- **概念**：采集容器 stdout/stderr 日志和容器内文件日志。  
- **常见实现（EFK/ELK）**：Fluentd 或 Filebeat 作为 DaemonSet 运行，读取容器日志文件（位于 `/var/log/containers/`），转发到 Elasticsearch 集群，并由 Kibana 可视化。  

## 常见排查机制

- **事件**：Kubernetes 资源状态变化时生成 Event 对象，存储在 API Server 中，可通过 `kubectl describe` 查看。  
- **日志**：kubelet 将容器标准输出日志轮转存储，默认保留近期文件。  
- **exec 调试**：通过 API Server 与 kubelet 的 `/exec` 子资源建立流式连接，在容器内执行命令（基于容器运行时 exec API）。

## 🔗 相关文档

{% post_link Kubernetes/Kubernetes核心概念全景 %} | {% post_link Kubernetes/Kubernetes调度与部署策略 %} | {% post_link Kubernetes/Kubernetes资源管理 %}
