---
title: "Kubernetes核心概念详解"
description: "详细介绍Kubernetes中的核心概念Pod、Deployment、StatefulSet、DaemonSet、Service、Ingress等，以及相应的kubectl管理命令"
tags: [Kubernetes, 容器编排, kubectl, DevOps, 云原生]
categories: [云原生技术]
date: 2026-05-14
aliases: ["K8s核心概念", "Kubernetes命令大全", "kubectl命令参考"]
---

# 🚀 Kubernetes核心概念详解

## 📦 Pod - 最小部署单元

**概念**：Kubernetes中最小的可部署计算单元，封装一个或多个共享网络和存储的容器。

```bash
# 创建pod
kubectl run <pod-name> --image=<image-name>
# 查看pod列表
kubectl get pods
# 查看pod详情
kubectl describe pod <pod-name>
# 删除pod
kubectl delete pod <pod-name>
# 查看pod日志
kubectl logs <pod-name>
# 进入pod容器
kubectl exec -it <pod-name> -- /bin/sh
# 查看pod资源使用
kubectl top pod
```

## 🎮 Deployment - Pod管理器

**概念**：管理Pod副本，提供自愈、扩缩容、滚动更新和回滚能力。

```bash
# 创建deployment
kubectl create deployment <deployment-name> --image=<image-name>
# 查看deployment列表
kubectl get deployments
# 扩缩容副本数
kubectl scale deployment <deployment-name> --replicas=<number>
# 更新镜像版本
kubectl set image deployment/<deployment-name> <container-name>=<new-image>
# 查看更新状态
kubectl rollout status deployment/<deployment-name>
# 回滚版本
kubectl rollout undo deployment/<deployment-name>
# 删除deployment
kubectl delete deployment <deployment-name>
```

## 🎯 StatefulSet - 有状态应用管理

**概念**：管理有状态应用，保证Pod标识、网络、存储的稳定性。

```bash
# 查看statefulset列表
kubectl get statefulsets
# 扩缩容副本数
kubectl scale statefulset <statefulset-name> --replicas=<number>
# 删除statefulset
kubectl delete statefulset <statefulset-name>
```

## 🔄 DaemonSet - 节点守护进程

**概念**：在每个节点上运行一个Pod副本，适合日志收集、监控等系统级任务。

```bash
# 查看daemonset列表
kubectl get daemonsets
# 删除daemonset
kubectl delete daemonset <daemonset-name>
```

## 🔗 Service - 服务发现与负载均衡

**概念**：为Pod提供稳定的网络访问入口，支持服务发现和负载均衡。

```bash
# 创建service
kubectl expose deployment <deployment-name> --type=<service-type> --port=<port>
# 查看service列表
kubectl get services
# 查看service详情
kubectl describe service <service-name>
# 删除service
kubectl delete service <service-name>
# 查看端点
kubectl get endpoints
```

## 🌐 Ingress - HTTP路由规则

**概念**：配置HTTP/HTTPS路由规则，将外部流量路由到集群内部服务。

```bash
# 创建ingress
kubectl create ingress <ingress-name> --rule=<host>/<path>=<service>:<port>
# 查看ingress列表
kubectl get ingress
# 查看ingress详情
kubectl describe ingress <ingress-name>
# 删除ingress
kubectl delete ingress <ingress-name>
```

## 🛠️ 通用管理命令

### 配置文件管理

```bash
# 应用配置文件
kubectl apply -f <filename.yaml>
# 删除配置文件中的资源
kubectl delete -f <filename.yaml>
# 查看资源配置
kubectl get -o yaml <resource-type> <resource-name>
# 查看所有资源
kubectl get all
```

### 命名空间管理

```bash
# 查看命名空间
kubectl get namespaces
# 创建命名空间
kubectl create namespace <namespace-name>
# 设置默认命名空间
kubectl config set-context --current --namespace=<namespace-name>
# 删除命名空间
kubectl delete namespace <namespace-name>
```

### 故障排查

```bash
# 查看事件
kubectl get events
# 实时跟踪日志
kubectl logs -f <pod-name>
# 在pod中执行命令
kubectl exec -it <pod-name> -- <command>
# 查看节点资源使用
kubectl top nodes
# 查看pod资源使用
kubectl top pods
```

### 节点管理

```bash
# 查看节点
kubectl get nodes
# 标记节点不可调度
kubectl cordon <node-name>
# 恢复节点调度
kubectl uncordon <node-name>
# 驱逐节点上的pod
kubectl drain <node-name>
```

---

## 📝 文档说明

本文档整理了Kubernetes的核心概念和常用命令，涵盖了从基础的Pod管理到高级的服务发现和路由配置。所有命令都遵循小写命令、大写参数的规范，便于在实际工作中快速查阅和使用。

### 核心概念总结

- **Pod**：最小部署单元，容器编排的基础
- **Deployment**：无状态应用管理，支持滚动更新和回滚
- **StatefulSet**：有状态应用管理，保证稳定标识和存储
- **DaemonSet**：节点级守护进程，确保每个节点运行副本
- **Service**：服务发现和负载均衡，提供稳定访问入口
- **Ingress**：HTTP路由规则，管理外部流量访问

## 🔗 相关文档

{% post_link Kubernetes/Kubernetes调度与部署详解 %} | {% post_link Kubernetes/Kubernetes网络模型与实现详解 %} | {% post_link Kubernetes/Kubernetes存储抽象详解 %} | {% post_link Kubernetes/Kubernetes安全基础详解 %} | {% post_link Kubernetes/Kubernetes可观测性详解 %}
