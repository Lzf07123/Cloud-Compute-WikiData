---
title: "Docker 网络模型"
description: "bridge、host、none、container 模式，以及端口映射"
tags: [docker, network, bridge, host, port-mapping]
categories: [Docker]
date: 2026-05-07
aliases: ["容器网络模式"]
---

# 第二阶段：Docker（容器引擎入门）·容器网络模型

## Bridge（默认）

- **概念**：每个容器连接到虚拟桥接设备（docker0），通过 NAT 实现与外网通信。  
- **实现**：Docker 创建名为 `docker0` 的虚拟以太网桥，为每个容器创建 veth pair（一端在容器 netns，另一端挂到 docker0）。容器间通过桥转发通信。出站流量通过 iptables 的 MASQUERADE 规则做源地址转换。  

## Host

- **概念**：容器直接使用宿主机网络栈，不创建 netns。  
- **实现**：容器启动时不设置独立 netns，而是加入宿主机 netns，因此容器端口直接监听宿主机网络。  

## None

- **概念**：容器拥有独立 netns 但无任何网络接口，完全隔离。  
- **实现**：仅创建 netns 但不分配 veth，用于需要自定义网络配置的场景。  

## Container

- **概念**：新容器与已有容器共享 netns。  
- **实现**：设置 `net` namespace 为指定容器的 PID 对应的 netns。  

## 端口映射

- **实现**：通过 iptables 的 DNAT（目标地址转换）规则，将访问宿主机特定端口的流量重定向到容器的 IP 和端口（容器 IP 是 bridge 网络内的地址）。

## 🔗 相关文档

{% post_link Docker/Docker架构解析 %} | {% post_link Docker/Docker容器生命周期 %} | {% post_link Docker/Docker-Compose多容器编排入门 %}
