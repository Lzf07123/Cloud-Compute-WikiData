---
title: "Kubernetes网络模型与实现详解"
description: "深入解析Kubernetes网络模型，包括CNI插件配置、Service类型配置和Ingress Controller配置"
tags: [Kubernetes, 网络, CNI, Service, Ingress]
categories: [云原生]
date: 2026-05-15
aliases: ["K8s网络配置", "容器网络管理"]
---

# Kubernetes网络模型与实现详解 🌐

## CNI 插件配置 🔌

### Flannel 安装与配置 🚀

**CNI（Container Network Interface）** 是Kubernetes标准化的网络插件接口规范，负责容器网络接口的动态配置与管理，实现跨节点Pod间通信和网络资源分配。
**安装命令：**

```bash
# 下载 Flannel 配置文件
wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
# 拉取 Flannel 镜像
docker pull quay.io/coreos/flannel:v0.14.0
# 安装 Flannel
kubectl create -f kube-flannel.yml
# 查看 Flannel 状态
kubectl get pod -n kube-system
# 卸载 Flannel
kubectl delete -f kube-flannel.yml
```

**关键配置参数：**

- 网络配置：`"Network": "10.244.0.0/16"` # Pod网络CIDR
- 后端类型：`"Type": "vxlan"` # 虚拟可扩展局域网
- 部署方式：DaemonSet # 每个节点运行一个实例
- CNI 配置路径：`/etc/cni/net.d/10-flannel.conflist`

### Calico 安装与配置 🐱

**Calico** 是高性能的CNI插件，提供网络策略、BGP路由和细粒度安全控制，支持大规模企业级Kubernetes集群。
**安装命令：**

```bash
# 下载 Calico 配置文件
wget https://docs.projectcalico.org/manifests/calico.yaml
# 安装 Calico
kubectl apply -f calico.yaml
# 查看 Calico 状态
kubectl get pod -n kube-system | grep calico
# 应用自定义配置
kubectl apply -f calico-installation.yaml
```

**关键配置参数：**

- CNI 网络配置：`"name": "k8s-pod-network"` # 网络名称
- 支持网络策略功能 # Pod间访问控制
- etcd 配置（可选） # 分布式键值存储
- MTU 值可调整 # 最大传输单元

## Service 类型配置 🌐

### ClusterIP Service 🏠

**ClusterIP** 是Kubernetes默认的Service类型，为集群内部应用提供虚拟IP地址，实现Pod间负载均衡和服务发现。
**创建命令：**

```bash
# 创建 ClusterIP Service
kubectl create service clusterip my-service --tcp=80:80
# 通过 Deployment 暴露
kubectl expose deployment my-app --name=my-service --port=80 --target-port=8080
# 查看 Service
kubectl get services
```

**配置文件示例：**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-clusterip-service
spec:
  type: ClusterIP # 集群内部访问类型
  selector:
    app: my-app # 选择器匹配Pod标签
  ports:
    - protocol: TCP
      port: 80 # Service端口
      targetPort: 8080 # Pod目标端口
```

### NodePort Service 🚪

**NodePort** 通过每个节点的IP和固定端口向集群外部暴露服务，适合测试环境和小规模部署。
**创建命令：**

```bash
# 创建 NodePort Service
kubectl create service nodeport my-service --tcp=80:80 --node-port=30080
# 通过 Deployment 暴露为 NodePort
kubectl expose deployment my-app --name=my-nodeport-service --port=80 --target-port=8080 --type=NodePort
# 查看 Service
kubectl get services
```

**配置文件示例：**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort # 节点端口访问类型
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30080 # 节点端口（30000-32767）
```

### LoadBalancer Service ⚖️

**LoadBalancer** 通过云提供商的负载均衡器向外部暴露服务，自动分配外部IP，适合生产环境。
**创建命令：**

```bash
# 创建 LoadBalancer Service
kubectl create service loadbalancer my-service --tcp=80:80
# 查看 LoadBalancer Service
kubectl get services
# 查看 LoadBalancer 的外部 IP
kubectl get service my-service -o wide
```

**配置文件示例：**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer # 负载均衡器类型
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

## Ingress Controller 配置 🛣️

### NGINX Ingress Controller 安装 🎯

**Ingress Controller** 是Kubernetes的HTTP/HTTPS路由控制器，基于域名和路径规则将外部流量路由到内部Service，提供七层负载均衡。
**安装命令：**

```bash
# 安装 NGINX Ingress Controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml
# 或者使用官方 manifests
kubectl apply -f deployments/deployment/nginx-ingress.yaml
# 查看 Ingress Controller 状态
kubectl get pods -n ingress-nginx
# 查看 Ingress Controller Service
kubectl get svc -n ingress-nginx
```

**Ingress 资源配置：**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: / # URL重写规则
spec:
  ingressClassName: nginx # Ingress类名
  rules:
    - host: my-app.example.com # 域名
      http:
        paths:
          - path: / # 路径匹配
            pathType: Prefix # 前缀匹配类型
            backend:
              service:
                name: my-service # 后端Service名称
                port:
                  number: 80 # 后端Service端口
```

**NGINX Ingress Controller Service 配置：**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-ingress-lb
  namespace: kube-system
  labels:
    app: nginx-ingress-lb
spec:
  type: LoadBalancer # 负载均衡器类型
  ports:
    - port: 80 # HTTP端口
      targetPort: 80 # 容器端口
      protocol: TCP
      name: http
    - port: 443 # HTTPS端口
      targetPort: 443
      protocol: TCP
      name: https
  selector:
    app: nginx-ingress-lb # 选择器匹配Pod标签
```

## 通用管理命令 🛠️

```bash
# 查看所有 Service
kubectl get services --all-namespaces
# 查看 Service 详细信息
kubectl describe service my-service
# 查看 Service 端点
kubectl get endpoints
# 查看 Ingress 资源
kubectl get ingress --all-namespaces
# 查看 Ingress 详细信息
kubectl describe ingress my-ingress
# 查看 CNI 插件状态
kubectl get pods -n kube-system | grep -E 'flannel|calico'
# 查看节点网络状态
kubectl get nodes -o wide
```

## 注意事项 ⚠️

- 确保所有命令和配置文件准确无误
- 根据实际环境调整网络配置参数
- 定期检查网络组件状态，确保集群网络正常运行

## 🔗 相关文档

{% post_link Kubernetes/Kubernetes核心概念详解 %} | {% post_link Kubernetes/Kubernetes调度与部署详解 %} | {% post_link Kubernetes/Kubernetes安全基础详解 %}
