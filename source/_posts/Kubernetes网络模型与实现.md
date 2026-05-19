---
title: "Kubernetes 网络模型与实现"
description: "CNI 插件、Service 的 ClusterIP/NodePort/LoadBalancer 类型、Ingress Controller"
tags: [kubernetes, network, cni, service, ingress, calico, flannel]
categories: [Kubernetes]
date: 2026-05-07
aliases: ["K8s网络原理"]
---

# 第四阶段：Kubernetes（容器编排）·网络模型与实现

## CNI 插件

- **概念**：容器网络接口，Kubernetes 通过 CNI 配置 Pod 网络。  
- **实现**：kubelet 在创建 Pod 时调用 CNI 可执行文件（如 Calico、Flannel），传递 JSON 配置。插件负责分配 IP、创建 veth 对、设置路由及网络策略。  
  - **Flannel**：提供简单 Overlay 网络（VXLAN/host-gw），节点间通过隧道或直接路由转发 Pod 网络。  
  - **Calico**：基于 BGP 的三层路由方案，同时支持 NetworkPolicy 以 iptables 或 eBPF 实现。  

## Service 网络实现

- **iptables 模式**：Service 的每个端口生成若干 iptables 规则（PREROUTING、OUTPUT 等），随机或轮询选后端 Pod IP。  
- **IPVS 模式**：使用 Linux 内核 IPVS 模块，支持更多负载均衡算法（rr、wrr、lc、sh 等），性能优于 iptables。  

## Ingress Controller 实现

以 Pod 形式运行，监听 Ingress 资源，生成负载均衡器配置（如 Nginx 的 `nginx.conf`），并将外部流量代理到对应 Service。通常作为 NodePort 或 LoadBalancer 类型暴露。  
