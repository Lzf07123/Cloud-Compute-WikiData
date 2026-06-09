---
title: "OpenStack Glance 镜像服务概念"
description: "全面介绍OpenStack Glance镜像服务的镜像格式、上传下载共享操作、后端存储方案及镜像与存储解耦架构"
tags: [OpenStack, Glance, 镜像服务, 云平台, IaaS]
categories: [技术文档, 云计算]
date: 2026-05-16
aliases: ["Glance镜像服务", "OpenStack镜像管理", "镜像服务组件"]
---

# OpenStack Glance 镜像服务概念

Glance 是 OpenStack 的核心组件之一，提供虚拟机镜像的**发现、注册、存储和分发**能力，为 Nova 计算服务提供启动镜像资源。

## 一、镜像格式

### 1. 磁盘格式（Disk Format）

Glance 支持多种磁盘格式，以适应不同的虚拟化平台和使用场景：

| 格式            | 说明                                        | 典型场景        |
| --------------- | ------------------------------------------- | --------------- |
| **QCOW2**       | QEMU 写时复制格式，支持动态分配、快照、压缩 | 生产环境最常用  |
| **RAW**         | 裸磁盘镜像，无压缩、无封装，性能最高        | 高性能场景      |
| **VMDK**        | VMware 虚拟机磁盘格式                       | VMware 迁移集成 |
| **VHD/VHDX**    | Microsoft Hyper-V 磁盘格式                  | Hyper‑V 环境    |
| **ISO**         | 光盘镜像，包含可引导文件系统                | OS 安装盘       |
| **VDI**         | Oracle VirtualBox 格式                      | VirtualBox 用户 |
| **PLOOP**       | Virtuozzo 容器格式                          | OS 容器运行     |
| **AKI/AMI/ARI** | Amazon 内核/机器/RAMDisk 镜像               | AWS 相关服务    |

### 2. 容器格式（Container Format）

与磁盘格式配合使用，描述镜像是否封装：

| 容器格式 | 说明                   |
| -------- | ---------------------- |
| `bare`   | 无容器封装，最常用     |
| `ovf`    | OVF 封装，含虚拟机描述 |
| `ova`    | OVF 打包为单个文件     |
| `docker` | Docker 容器格式        |

## 二、镜像生命周期状态

| 状态             | 说明                           |
| ---------------- | ------------------------------ |
| `queued`         | 镜像元数据已创建，数据尚未上传 |
| `saving`         | 镜像数据正在上传中             |
| `active`         | 镜像上传成功，可用状态         |
| `killed`         | 上传失败或镜像文件不可读       |
| `deleted`        | 镜像标记删除，数据仍保留       |
| `pending_delete` | 镜像即将删除，不可恢复         |

## 三、镜像操作命令

### 1. 上传镜像

```bash
openstack image create --disk-format QCOW2 --container-format BARE --file ./IMAGE.QCOW2 镜像名
glance image-create --disk-format QCOW2 --container-format BARE --file ./IMAGE.QCOW2 --name 镜像名
```

- `--disk-format`：指定磁盘格式（QCOW2 / RAW / VMDK / ISO）
- `--container-format`：指定容器格式（常用 `bare`）
- `--file`：指定本地镜像文件路径
- `--public` / `--private`：设置镜像可见性

### 2. 下载镜像

```bash
openstack image save --file ./OUTPUT.QCOW2 镜像ID
glance image-download --file ./OUTPUT.QCOW2 镜像ID
```

### 3. 列出与查询镜像

```bash
openstack image list
openstack image show 镜像ID
glance image-list
```

- `--status ACTIVE`：筛选状态
- `--disk-format QCOW2`：按格式筛选

### 4. 删除镜像

```bash
openstack image delete 镜像ID
glance image-delete 镜像ID
```

### 5. 共享镜像

```bash
openstack image set --shared 镜像ID
openstack image add project 镜像ID 租户ID
```

- 通过 RBAC 策略控制租户间共享，支持 `member` 角色授权
- 镜像所有者可将镜像共享给指定租户

## 四、镜像导入高级特性

Glance 支持多种导入方式以满足不同场景：

```bash
# Web-Download 导入（从URL直接下载到后端存储）
openstack image create --disk-format QCOW2 --container-format BARE --import --uri HTTPS://EXAMPLE.COM/IMAGE.QCOW2 镜像名

# Glance-Direct 导入（任务方式导入）
glance md-namespace-list
```

- **Web-Download**：直接从 URL 拉取镜像到后端，不经过客户端，适合大文件
- **Glance-Direct**：基于任务的任务型导入，支持进度追踪
- **Copy-Image**：在不同存储后端间复制镜像（multi‑store 场景）

## 五、后端存储

Glance 架构将**镜像元数据**与**镜像实际数据**分离，支持动态切换后端：

| 后端             | 说明                                        | 生产推荐度 |
| ---------------- | ------------------------------------------- | ---------- |
| **本地文件系统** | 默认简单存储，适合测试环境                  | ⭐         |
| **Swift**        | OpenStack 原生对象存储，Glance 默认后端之一 | ⭐⭐⭐     |
| **Ceph RBD**     | 分布式块存储，性能与可靠性兼备              | ⭐⭐⭐     |
| **Cinder**       | OpenStack 块存储服务                        | ⭐⭐       |
| **S3**           | Amazon 对象存储，兼容 S3 API                | ⭐⭐       |
| **NFS**          | 网络文件系统（Red Hat 不推荐生产使用）      | ⭐         |

### 镜像与后端存储解耦

Glance 的核心设计理念是将镜像的**元数据**（存于 MySQL/PostgreSQL）与**实际数据**（存于后端存储）彻底分离，带来以下优势：

- **后端可切换**：无需修改镜像元数据即可更换存储后端
- **Multi‑Store 多后端**：可同时挂载多个存储后端，实现异构存储管理
- **存储扩展灵活**：从本地文件到 Ceph/Swift 可平滑迁移

## 六、架构组件

| 组件              | 说明                                     |
| ----------------- | ---------------------------------------- |
| `glance-api`      | 提供 RESTful API，处理上传/下载/查询请求 |
| `glance-registry` | 旧版元数据服务（新版已合并至 API）       |
| **Database**      | MySQL/PostgreSQL，存储镜像元数据         |
| **Image Store**   | 后端存储驱动层，对接各类存储系统         |

## 生产最佳实践

- 生产环境推荐 **QCOW2 + Ceph RBD** 组合
- 使用 `--public` 或 `--shared` 管理跨租户共享权限，避免镜像重复上传
- 大镜像导入优先使用 **Web-Download** 模式，避免客户端网络瓶颈
- 定期清理 `killed` / `deleted` 状态的镜像以释放后端存储空间
- 多存储场景下可通过 **Copy-Image** 实现镜像在不同后端间的迁移

## 🔗 相关文档

{% post_link OpenStack/OpenStack-Glance镜像服务详解 %} | {% post_link OpenStack/Nova计算服务概念 %} | {% post_link OpenStack/Keystone认证服务概念 %}
