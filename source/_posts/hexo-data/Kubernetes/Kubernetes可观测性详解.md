---
title: "Kubernetes可观测性完整指南"
description: "全面介绍Kubernetes集群的资源监控、日志收集和故障排查解决方案"
tags: [Kubernetes, 可观测性, 监控, 日志, 故障排查]
categories: [容器编排, 运维]
date: 2026-05-14
aliases: ["Kubernetes监控", "K8s日志收集", "容器故障排查"]
---

## 资源监控（Metrics Server）📈

### 安装和配置

- **准备工作**：确保Kubernetes版本1.8及以上，API聚合层必须启用，Kubelet必须启用Webhook认证和授权 ⚙️
- **安装命令**：
  - 下载部署文件：`wget https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`
  - 高可用版本：`kubectl apply -f high-availability-1.21+.yaml`
  - 国内镜像替换：将`registry.k8s.io/metrics-server/metrics-server:v0.7.2`替换为可用镜像地址 🌏

### 核心参数配置

```yaml
args:
  - --kubelet-insecure-tls
  - --cert-dir=/tmp
  - --secure-port=10250
  - --kubelet-preferred-address-types=InternalIP,Hostname,InternalDNS,ExternalDNS,ExternalIP
  - --kubelet-use-node-status-port
  - --metric-resolution=15s
```

### 使用命令

- 查看节点资源使用：`kubectl top nodes`
- 查看Pod资源使用：`kubectl top pods -n <NAMESPACE>`
- 验证安装：`kubectl get pods -n kube-system | grep metrics-server`

## 日志收集（EFK）📝

### EFK架构

- **Elasticsearch**：分布式搜索和分析引擎，用于存储和索引日志数据 🔍
- **Fluentd**：日志收集器，作为DaemonSet部署在每个节点上，收集容器日志 🔄
- **Kibana**：可视化分析平台，提供日志查询和展示界面 📊

### 部署步骤

1. **创建命名空间**：`kubectl create namespace logging`
2. **部署Elasticsearch**：
   - 使用StatefulSet部署ES集群
   - 配置存储卷和环境变量 💾
3. **部署Fluentd**：
   - 作为DaemonSet部署，确保每个节点都有实例
   - 配置RBAC权限访问Kubernetes API 🔐
4. **部署Kibana**：
   - 使用Deployment部署
   - 配置Service和Ingress暴露服务 🌐

### Fluentd配置要点

- 监听路径：`/var/log/containers/*.log`
- 输出目标：Elasticsearch集群
- 使用kubernetes插件增强日志信息

## 常见排查命令🔧

### 基础状态检查

- **查看Pod状态**：`kubectl get pods -n <NAMESPACE>`
- **查看节点状态**：`kubectl get nodes`
- **查看集群事件**：`kubectl get events --all-namespaces`

### 详细信息查询

- **查看Pod详细信息**：`kubectl describe pod <POD-NAME> -n <NAMESPACE>`
- **查看节点详细信息**：`kubectl describe node <NODE-NAME>`
- **查看Service信息**：`kubectl describe svc <SERVICE-NAME> -n <NAMESPACE>`

### 日志查看

- **查看容器日志**：`kubectl logs <POD-NAME> -c <CONTAINER-NAME> -n <NAMESPACE>`
- **查看前一次日志**：`kubectl logs <POD-NAME> --previous`
- **实时跟踪日志**：`kubectl logs -f <POD-NAME>`

### 容器操作

- **进入容器**：`kubectl exec -it <POD-NAME> -n <NAMESPACE> -- /bin/bash`
- **在容器中执行命令**：`kubectl exec <POD-NAME> -- <COMMAND>`
- **文件拷贝**：`kubectl cp <LOCAL-FILE> <POD-NAME>:<PATH>`

### 网络和存储排查

- **端口转发**：`kubectl port-forward <POD-NAME> <LOCAL-PORT>:<POD-PORT>`
- **检查Endpoints**：`kubectl get endpoints <SERVICE-NAME> -n <NAMESPACE>`
- **检查PVC状态**：`kubectl get pvc -n <NAMESPACE>`

### 资源监控

- **查看资源使用情况**：`kubectl top nodes` 和 `kubectl top pods -n <NAMESPACE>`
- **检查API服务器健康**：`kubectl get --raw=/healthz`

### 常见问题排查

- **Pod Pending**：检查资源不足、节点Selector不匹配 ⏳
- **CrashLoopBackOff**：查看应用日志，检查启动命令 🔄
- **ImagePullBackOff**：检查镜像名称和认证配置 🐳
- **Service无法访问**：检查Endpoints和网络策略 🌐

## 🔗 相关文档

{% post_link Kubernetes/Kubernetes核心概念详解 %} | {% post_link Kubernetes/Kubernetes调度与部署详解 %} | {% post_link Kubernetes/Kubernetes资源管理详解 %}
