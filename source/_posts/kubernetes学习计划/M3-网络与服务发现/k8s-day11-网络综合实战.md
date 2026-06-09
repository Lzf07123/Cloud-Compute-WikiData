---
title: Day 11 - 网络综合实战
module: M3-网络与服务发现
day: 11
duration: 240 分钟
level: 核心
prerequisites:
  - M3 Day 08-10 全部完成
objectives:
  - 构建完整的三层网络架构
  - Service + Ingress + NetworkPolicy 联动
  - 排查复杂网络问题
  - 理解跨命名空间网络隔离
tags:
  - 综合实战
  - 三层网络
  - 网络排错
  - 跨命名空间
---

# 📘 Day 11：网络综合实战

## 🎯 今日目标

- [ ] 在一个场景中综合使用 Service + Ingress + NetworkPolicy
- [ ] 排查跨命名空间的网络故障
- [ ] 构建生产级网络架构

---

## 🧠 理论精讲（10 分钟）

### 网络排错方法论

```
1. 确认 Pod 是否 Running?
2. 确认 Pod 是否有 IP?
3. 确认 Service 是否有 Endpoint?
4. 确认 DNS 是否解析正常?
5. 确认 NetworkPolicy 是否阻止?
6. 从近到远逐层测试
```

### 常用网络调试命令

```bash
# DNS 测试
nslookup <service-name>

# TCP 连通性测试
nc -zv <host> <port>

# HTTP 测试
wget -q -O- http://<host>:<port>

# 网络包抓取（需额外工具）
tcpdump -i any port <port>
```

---

## 🔧 动手实操（150 分钟）

### 练习 11.1：三层网络架构构建

```bash
# 创建独立命名空间
kubectl create ns net-lab

# === 第 1 层：Ingress 入口层 ===
kubectl create deploy frontend -n net-lab --image=nginx:alpine --replicas=2
kubectl expose deploy frontend -n net-lab --port=80 --name=frontend-svc

# === 第 2 层：API 中间层 ===
kubectl create deploy api -n net-lab --image=httpd:alpine --replicas=3
kubectl expose deploy api -n net-lab --port=80 --name=api-svc

# === 第 3 层：数据层 ===
kubectl run db -n net-lab --image=redis:7-alpine --port=6379
kubectl expose pod db -n net-lab --port=6379 --name=db-svc

# === Ingress 规则 ===
cat <<EOF | kubectl apply -f -
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: net-lab-ingress
  namespace: net-lab
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: lab.example.com
    http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: api-svc
            port:
              number: 80
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend-svc
            port:
              number: 80
EOF

# 验证：curl -H "Host: lab.example.com" <NodeIP>:30080/
```

---

### 练习 11.2：跨命名空间网络通信验证

```bash
# 1. 从 net-lab 中的 Pod 访问其他命名空间的服务
kubectl run jumpbox -n net-lab --image=busybox:1.36 -- sleep 3600

# 测试 DNS 解析
kubectl exec jumpbox -n net-lab -- nslookup db-svc.net-lab.svc.cluster.local

# 测试 db
kubectl exec jumpbox -n net-lab -- nc -zv db-svc.net-lab 6379

# 2. 从 default 命名空间访问 net-lab
kubectl run cross-test --image=busybox:1.36 --rm -it --restart=Never -- \
  wget -q -O- http://api-svc.net-lab
```

---

### 练习 11.3：网络故障排查演练

```bash
# 故障 1：Service selector 不匹配
# 创建一个选错标签的 Service
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Service
metadata:
  name: broken-svc
  namespace: net-lab
spec:
  selector:
    app: non-existent       # 故意选一个不存在的标签！
  ports:
  - port: 80
EOF

kubectl get endpoints broken-svc -n net-lab
# ENDPOINTS: <none>（问题就在这里！）

kubectl describe svc broken-svc -n net-lab

# 修复：修改 selector
kubectl patch svc broken-svc -n net-lab --type=merge -p '{"spec":{"selector":{"app":"api"}}}'
kubectl get endpoints broken-svc -n net-lab
# 现在有 Endpoint 了

# 故障 2：NetworkPolicy 阻断排查
cat <<EOF | kubectl apply -f -
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: isolate-db
  namespace: net-lab
spec:
  podSelector:
    matchLabels:
      run: db
  policyTypes:
  - Ingress
  # 空 Ingress = 拒绝所有
EOF

# 测试 db 连接
kubectl exec jumpbox -n net-lab -- nc -zv db-svc 6379 -w 5
# 超时！（被 NetworkPolicy 阻断）

# 排查
kubectl get networkpolicy -n net-lab
kubectl describe networkpolicy isolate-db -n net-lab

# 修复
kubectl delete networkpolicy isolate-db -n net-lab

# 故障 3：DNS 无法解析
# 检查 CoreDNS
kubectl get pods -n kube-system -l k8s-app=kube-dns
# 如果 Pod 不 Running → 检查日志
```

---

## 🏆 赛题模拟（40 分钟）

> ⚠️ 严格限时 **40 分钟**

**题目：生产级三层网络架构**

```
【场景】搭建一个博客系统的网络层，包含前端、API、数据库三个层次。

【操作要求】

1. 创建命名空间 blog-system

2. 部署三层应用：
   a. 前端层：Deployment blog-frontend（nginx:alpine，2 副本）
      ClusterIP Service frontend-svc
   b. API 层：Deployment blog-api（httpd:alpine，3 副本）
      ClusterIP Service api-svc
   c. 数据库层：Pod blog-db（redis:7-alpine）
      ClusterIP Service db-svc

3. 配置 Ingress：
   - 对外暴露 30080，域名 blog.example.com
   - / → frontend-svc
   - /api → api-svc
   - 配置 rewrite-target: /

4. 配置 NetworkPolicy：
   a. 默认拒绝所有入站流量
   b. frontend-svc → api-svc:80
   c. api-svc → db-svc:6379
   d. Ingress Controller（ingress-nginx 命名空间）→ frontend-svc:80
   e. 所有 Pod → CoreDNS UDP 53

5. 验证：
   - 外部可访问 http://<NodeIP>:30080/ 和 /api
   - 前端能访问 API
   - API 能访问 DB
   - 前端**不能**直接访问 DB
   - DB **不能**访问外网

【评分标准】
- 三层应用正确部署（20 分）
- Ingress 路由正确（20 分）
- NetworkPolicy 隔离正确（40 分）
- 验证全面（20 分）
```
