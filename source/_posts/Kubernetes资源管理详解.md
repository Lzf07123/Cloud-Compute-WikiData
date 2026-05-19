---
title: "Kubernetes资源管理详解"
description: "全面介绍Kubernetes声明式YAML管理、kubectl常用命令和Namespace隔离机制"
tags: [Kubernetes, K8s, 资源管理, YAML, kubectl, Namespace]
categories: [云原生, 容器编排]
date: 2026-05-15
aliases: ["K8s资源管理", "Kubernetes命令指南"]
---

## 声明式YAML管理 📄

### YAML文件基本结构

YAML文件主要包含四个部分：

1. **apiVersion**：指定API版本
2. **kind**：资源类型（如Deployment、Service、Pod等）
3. **metadata**：元数据（名称、命名空间、标签等）
4. **spec**：资源规格配置

### YAML语法规则

- 大小写敏感 ⚠️
- 使用缩进表示层级关系，不支持Tab键
- 缩进空格数目不重要，相同层级左对齐即可
- 使用#号表示注释 💬
- 符号字符后缩进一个空格（冒号、逗号、横杠等）

### 声明式管理命令

```bash
kubectl create -f XXXX.YAML  # 从YAML文件创建资源
kubectl apply -f XXXX.YAML  # 应用配置变更，支持更新
kubectl delete -f XXXX.YAML  # 根据YAML文件删除资源
```

## kubectl常用命令 ⚡

### 基础命令

```bash
kubectl get pod  # 列出所有Pod
kubectl get pod -n NAMESPACE  # 列出指定命名空间的Pod
kubectl get pod -o wide  # 显示详细信息包括IP和节点
kubectl get all  # 列出所有资源
kubectl describe pod POD-NAME  # 显示Pod详细信息
kubectl logs POD-NAME  # 查看Pod日志
kubectl logs -f POD-NAME  # 实时跟踪日志输出
```

### 资源管理命令

```bash
kubectl create -f YAML-FILE  # 从文件创建资源
kubectl apply -f YAML-FILE  # 应用配置变更
kubectl delete -f YAML-FILE  # 删除文件中定义的资源
kubectl edit RESOURCE-TYPE RESOURCE-NAME  # 编辑资源配置
kubectl scale deployment DEPLOY-NAME --replicas=3  # 扩缩容到3个副本
```

### 部署管理命令

```bash
kubectl rollout status deployment/DEPLOY-NAME  # 查看部署状态
kubectl rollout history deployment/DEPLOY-NAME  # 查看部署历史
kubectl rollout undo deployment/DEPLOY-NAME  # 回滚到上一版本
kubectl set image deployment/DEPLOY-NAME CONTAINER-NAME=IMAGE:TAG  # 更新容器镜像
```

### 故障排查命令

```bash
kubectl exec -it POD-NAME -- bash  # 进入容器交互式shell
kubectl cp LOCAL-FILE POD-NAME:/path  # 在本地和容器间复制文件
kubectl top pod  # 查看Pod资源使用情况
kubectl top nodes  # 查看节点资源使用情况
```

## Namespace隔离 🏗️

### Namespace概念

Namespace是K8s中用于隔离和组织资源的机制，将物理集群划分为多个逻辑部分，每个部分有自己的一组资源 🔄

### 默认Namespace

- **default**：所有未指定Namespace的对象默认分配
- **kube-system**：K8s系统创建的资源 🖥️
- **kube-public**：可被所有人访问的资源 🌐
- **kube-node-lease**：节点心跳维护 💓

### Namespace管理命令

```bash
kubectl create namespace NAMESPACE-NAME  # 创建命名空间
kubectl get namespace  # 列出所有命名空间
kubectl get ns  # 列出命名空间的简写形式
kubectl delete namespace NAMESPACE-NAME  # 删除命名空间
kubectl config set-context --current --namespace=NAMESPACE-NAME  # 设置当前上下文的默认命名空间
```

### Namespace隔离特性

1. **资源对象隔离**：Service、Deployment、Pod等资源在不同Namespace中相互隔离 📦
2. **资源配额隔离**：可以限制不同Namespace的CPU、内存使用量 📊
3. **权限控制**：通过RBAC对不同Namespace进行权限管理 🔐
4. **网络隔离**：默认情况下不同Namespace的Pod可以互相访问，可通过NetworkPolicy实现网络隔离 🌐

### 网络隔离配置

通过NetworkPolicy实现Namespace间网络隔离：

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all # 拒绝所有网络流量
  namespace: target-ns # 应用到目标命名空间
spec:
  podSelector: {} # 选择所有Pod
  policyTypes:
    - Ingress # 入站流量
    - Egress # 出站流量
  ingress: [] # 空数组表示拒绝所有入站流量
  egress: [] # 空数组表示拒绝所有出站流量
```

### 资源配额管理

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: resource-quota # 配额名称
  namespace: prod # 应用到生产环境命名空间
spec:
  hard: # 硬性限制
    requests.cpu: "1" # CPU请求总量不超过1核
    requests.memory: "1Gi" # 内存请求总量不超过1GB
    limits.cpu: "2" # CPU限制总量不超过2核
    limits.memory: "4Gi" # 内存限制总量不超过4GB
    pods: "10" # Pod数量不超过10个
```

### 实际应用场景

1. **按环境划分**：dev、test、prod 🌍
2. **按团队划分**：不同团队使用不同Namespace 👥
3. **按项目划分**：每个项目独立Namespace 📁
4. **多租户隔离**：实现资源隔离和权限控制 🏢
