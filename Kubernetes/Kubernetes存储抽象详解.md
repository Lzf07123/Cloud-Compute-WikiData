---
title: "Kubernetes存储抽象详解"
description: "深入解析Kubernetes存储抽象体系的核心组件PV、PVC和StorageClass，包括概念解析、配置方法和管理命令"
tags: [Kubernetes, 存储, PV, PVC, StorageClass, 持久化]
categories: [云原生, 容器技术]
date: 2026-05-13
aliases: ["K8s存储", "持久卷配置", "动态存储供应"]
---

# 核心概念解析 📚

## 一、PV（持久卷）💾

PV是集群级别的存储资源，代表实际的存储设备或存储空间，如NFS、Ceph、云硬盘等。

### 主要特性

- **独立生命周期**: 即使Pod被删除，PV中的数据仍然保留
- **访问模式支持**:
  - ReadWriteOnce（单节点读写）
  - ReadOnlyMany（多节点只读）
  - ReadWriteMany（多节点读写）

### 静态PV创建示例

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-pv
spec:
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: nfs
  mountOptions:
    - hard
    - nfsvers=4.1
  nfs:
    path: /opt/k8s-pods/data
    server: 192.168.179.102
```

## 二、PVC（持久卷声明）📝

PVC是用户对存储资源的请求，定义了所需存储的大小、访问模式等属性。

### 核心作用

- 作为存储的"接口"，封装底层存储复杂性
- 使开发者无需关心具体存储实现
- 一个PV只能被一个PVC绑定

### PVC创建示例

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 2Gi
  storageClassName: nfs
```

## 三、StorageClass（存储类）⚙️

StorageClass是实现动态存储供应的核心组件，定义了如何创建PV的模板。

### 关键字段

- **provisioner**: 存储供应者
- **parameters**: 存储参数
- **reclaimPolicy**: 回收策略
- **volumeBindingMode**: 绑定模式

### StorageClass创建示例

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: nfs-client-storageclass
provisioner: nfs-storage
parameters:
  archiveOnDelete: "false"
reclaimPolicy: Delete
allowVolumeExpansion: true
volumeBindingMode: Immediate
```

---

# 存储供应模式 🔄

## 静态供应模式 📦

由管理员预先创建PV，然后PVC进行绑定。

### 创建命令

```bash
kubectl create -f pv.yaml
kubectl create -f pvc.yaml
kubectl create -f storageclass.yaml
```

## 动态供应模式 🚀

通过StorageClass自动创建PV，大大简化存储管理。

### 创建命令

```bash
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl apply -f storageclass.yaml
```

---

# PV生命周期状态 📊

PV的生命周期包括四种状态：
| 状态 | 描述 |
|------|------|
| **Available** | 还未与某个PVC绑定 |
| **Bound** | 已与某个PVC绑定 |
| **Released** | 绑定的PVC已经删除，资源已释放，但未被集群回收 |
| **Failed** | 自动资源回收失败 |

---

# 回收策略 ♻️

回收策略决定了PVC删除后PV的处理方式：
| 策略 | 描述 |
|------|------|
| **Retain** | PV不会被自动删除，数据依然保留 |
| **Delete** | PV会被自动删除，关联的存储资源也会被清理 |
| **Recycle** | 对卷执行基本清理操作，但已被弃用 |

---

# 完整创建方法 🔧

## 动态供应完整配置

### RBAC权限配置

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: nfs-client-provisioner
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: nfs-client-provisioner-runner
rules:
  - apiGroups: [""]
    resources: ["persistentvolumes"]
    verbs: ["get", "list", "watch", "create", "delete"]
  - apiGroups: [""]
    resources: ["persistentvolumeclaims"]
    verbs: ["get", "list", "watch", "update"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: run-nfs-client-provisioner
subjects:
  - kind: ServiceAccount
    name: nfs-client-provisioner
roleRef:
  kind: ClusterRole
  name: nfs-client-provisioner-runner
  apiGroup: rbac.authorization.k8s.io
```

### NFS Provisioner部署

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nfs-client-provisioner
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nfs-client-provisioner
  template:
    metadata:
      labels:
        app: nfs-client-provisioner
    spec:
      serviceAccountName: nfs-client-provisioner
      containers:
        - name: nfs-client-provisioner
          image: quay.io/external_storage/nfs-client-provisioner:latest
          volumeMounts:
            - name: nfs-client-root
              mountPath: /persistentvolumes
          env:
            - name: PROVISIONER_NAME
              value: nfs-storage
            - name: NFS_SERVER
              value: 192.168.179.102
            - name: NFS_PATH
              value: /opt/k8s
      volumes:
        - name: nfs-client-root
          nfs:
            server: 192.168.179.102
            path: /opt/k8s
```

### 创建动态供应组件

```bash
kubectl apply -f rbac.yaml
kubectl apply -f nfs-provisioner.yaml
kubectl apply -f storageclass.yaml
kubectl apply -f dynamic-pvc.yaml
```

---

# Pod使用PVC创建方法 🐳

## Pod使用PVC的YAML示例

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pv-pod
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - name: nginx-pv-storage
          mountPath: "/usr/share/nginx/html"
  volumes:
    - name: nginx-pv-storage
      persistentVolumeClaim:
        claimName: nfs-pvc
```

## StatefulSet使用PVC模板

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
              name: web
          volumeMounts:
            - name: www
              mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
    - metadata:
        name: www
      spec:
        accessModes: ["ReadWriteOnce"]
        storageClassName: "nfs-client-storageclass"
        resources:
          requests:
            storage: 1Gi
```

---

# 管理命令汇总 🛠️

## 资源创建命令

```bash
kubectl create -f <yaml-file>           # 创建资源
kubectl apply -f <yaml-file>            # 应用配置
kubectl apply -f <directory>            # 应用目录下所有配置
```

## 资源查看命令

```bash
kubectl get pv,pvc,sc                   # 查看所有存储资源
kubectl get pv -o wide                  # 查看PV详细信息
kubectl get pvc -o wide                 # 查看PVC详细信息
kubectl describe sc <sc-name>           # 查看StorageClass详情
kubectl get pv --sort-by=.spec.capacity.storage  # 按容量排序PV
```

## 资源删除命令

```bash
kubectl delete pv <pv-name>             # 删除PV
kubectl delete pvc <pvc-name>           # 删除PVC
kubectl delete sc <sc-name>             # 删除StorageClass
kubectl delete -f <yaml-file>           # 删除配置文件中的资源
kubectl delete pod,pvc,pv --all        # 删除所有相关资源
```

## 故障排查命令

```bash
kubectl describe pvc <pvc-name>         # 查看PVC详细状态
kubectl describe pv <pv-name>           # 查看PV详细状态
kubectl logs <provisioner-pod>          # 查看Provisioner日志
kubectl get events --sort-by=.metadata.creationTimestamp  # 查看事件
kubectl exec -it <pod-name> -- /bin/bash  # 进入容器调试
```

## 存储资源扩展命令

```bash
kubectl patch pvc <pvc-name> -p '{"spec":{"resources":{"requests":{"storage":"10Gi"}}}}'  # 扩展PVC容量
kubectl get pv,pvc -o wide              # 查看扩展后的状态
```

---

# 不同存储类型创建示例 💿

## 本地存储PV创建

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: local-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Delete
  storageClassName: local-storage
  local:
    path: /mnt/disks/ssd1
  nodeAffinity:
    required:
      nodeSelectorTerms:
        - matchExpressions:
            - key: kubernetes.io/hostname
              operator: In
              values:
                - node1
```

## 云存储PV创建（AWS EBS示例）

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: aws-ebs-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Delete
  storageClassName: aws-ebs
  awsElasticBlockStore:
    volumeID: vol-0123456789abcdef0
    fsType: ext4
```

## Ceph RBD PV创建

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: ceph-rbd-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: ceph-rbd
  rbd:
    monitors:
      - "192.168.1.100:6789"
      - "192.168.1.101:6789"
      - "192.168.1.102:6789"
    pool: rbd
    image: foo
    fsType: ext4
    readOnly: false
    user: admin
    secretRef:
      name: ceph-secret
```

---

# 动态供应工作流程 🔄

动态供应机制的工作流程为：

1. **用户创建PVC并指定StorageClass** 📝
2. **Kubernetes根据StorageClass调用对应的provisioner创建PV** ⚙️
3. **PV自动绑定到PVC** 🔗
4. **Pod通过PVC使用存储资源** 🐳

---

# 最佳实践建议 ✨

1. **存储类规划**: 根据应用需求创建多个StorageClass，如高性能存储、标准存储、归档存储等
2. **回收策略选择**: 生产环境建议使用Retain策略以避免数据意外丢失
3. **容量规划**: 合理设置PV容量，避免资源浪费或不足
4. **监控告警**: 定期检查PV/PVC状态，设置存储使用率告警
5. **备份策略**: 为重要数据制定定期备份计划

## 🔗 相关文档

[Kubernetes核心概念详解](Kubernetes核心概念详解.md) | [Kubernetes调度与部署机制详解](Kubernetes调度与部署详解.md) | [ConfigMap配置与密文管理详解](Kubernetes配置与密文详解.md)
