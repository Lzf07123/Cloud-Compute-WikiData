---
title: "OpenStack Glance镜像服务详解"
description: "全面解析OpenStack Glance镜像服务的核心概念（镜像格式/后端存储/上传下载/镜像共享）、架构设计与生产实践"
tags: [OpenStack, Glance, 镜像服务, 云平台, 存储, Ceph]
categories: [云计算, 技术教程]
date: 2026-05-16
aliases: ["Glance镜像详解", "OpenStack镜像服务", "Glance架构"]
---

# OpenStack Glance镜像服务详解

Glance是OpenStack的**镜像服务核心组件**，提供虚拟机镜像的**发现、注册、检索和交付**能力。采用**元数据与数据分离**的架构设计，通过可插拔的 `glance_store` 适配层实现后端存储的灵活切换。📸

---

## 一、Glance核心架构概览

### 组件全景图

```
                    ┌──────────┐
                    │ Keystone │
                    └────┬─────┘
                         │ AUTH
                    ┌────▼─────┐
  USER ──REST API──► GLANCE-API│
                    └────┬─────┘
                         │
               ┌─────────▼──────────┐
               │    GLANCE.STORE    │ ◄── 存储抽象层
               ├────────────────────┤
               │  元数据 → SQL DB   │ ◄── MySQL（镜像元信息）
               │  镜像数据 → 后端   │ ◄── 可插拔后端存储
               └────────────────────┘
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
   FILE存储           SWIFT             CEPH RBD
  (本地磁盘)       (对象存储)          (分布式存储)
```

### 核心组件一览表

| 组件                | 职责              | 说明                                               |
| ------------------- | ----------------- | -------------------------------------------------- |
| **glance-api**      | RESTful API入口   | 处理镜像查询/上传/删除等所有请求，对接Keystone认证 |
| **glance-registry** | 元数据存储服务    | Newton版本后为可选组件，旧版负责镜像元数据读写     |
| **glance.db**       | 元数据库（MySQL） | 存储镜像ID、名称、格式、checksum、location等元信息 |
| **glance.store**    | 存储后端适配层    | 核心抽象层，实现镜像数据与后端存储的解耦           |

### 架构设计哲学 — 元数据与数据分离

```
镜像 = 镜像元数据 (Metadata) + 镜像数据 (Data)

元数据       → 存储在 glance.db (MySQL)
               ├── id, name, disk_format, container_format
               ├── size, status, visibility
               ├── checksum (MD5), os_hash_value (SHA512)
               ├── properties (自定义属性)
               └── locations (指向后端存储的 URI)

镜像数据     → 存储在后端存储中
               ├── file:///var/lib/glance/images/<UUID>
               ├── swift://account/container/<UUID>
               └── rbd://pool/image/<UUID>
```

**关键设计理念**: Nova 启动虚拟机时根据 location 直接从后端存储读取数据，**不经 glance-api 中转**，避免额外网络跳转和数据拷贝。🚀

---

## 二、镜像格式详解 🗂️

Glance 支持多种镜像格式，覆盖主流虚拟化平台：

|      格式       | 扩展名   | 特性                                                 | 适用场景                          |
| :-------------: | -------- | ---------------------------------------------------- | --------------------------------- |
|    **QCOW2**    | `.qcow2` | QEMU原生格式，稀疏文件（按需分配）、快照链、写时复制 | **最常用**，KVM/QEMU 虚拟化首选   |
|     **RAW**     | `.raw`   | 无格式裸磁盘镜像，性能最优                           | 高性能计算、数据库等I/O密集型场景 |
|    **VMDK**     | `.vmdk`  | VMware虚拟磁盘格式                                   | VMware 虚拟机迁移到 OpenStack     |
|  **VHD/VHDX**   | `.vhd`   | Microsoft Hyper-V 格式                               | 与 Hyper-V 平台互操作             |
|     **ISO**     | `.iso`   | 光盘归档格式                                         | 安装介质注入（如 cloud-init）     |
| **AKI/ARI/AMI** | -        | Amazon kernel/ramdisk/image                          | AWS 镜像兼容                      |
|    **Ploop**    | -        | Virtuozzo/Parallels 格式                             | 容器化虚拟化场景                  |
|   **Docker**    | -        | Docker 容器格式                                      | (已逐步淘汰)                      |

### QCOW2 vs RAW 深度对比 ⚡

```
QCOW2:
  ├── 创建时仅占用实际使用的空间（稀疏文件）
  ├── 支持快照链（snapshot chain）
  ├── 支持写时复制（copy-on-write, COW）
  ├── 支持 AES 加密（不推荐生产使用）
  ├── 支持压缩（qemu-img convert -c）
  └── 有小幅性能开销（约 5-10%）

RAW:
  ├── 创建时即分配全部空间
  ├── 无格式转换开销，I/O 性能最高
  ├── QEMU/KVM 直通效率最高
  ├── 可通过 qemu-img 转换为任意格式
  └── 占用磁盘空间较大
```

**经验建议**: 日常虚拟机用 **QCOW2**（节省空间、支持快照），高性能数据库类用 **RAW**，VMware 对接用 **VMDK**。✅

---

## 三、镜像上传与下载 📥📤

### 上传镜像

```bash
# 上传 QCOW2 格式镜像（最常用）
openstack image create "cirros-0.6.2" \
  --file cirros-0.6.2-x86_64-disk.img \
  --disk-format qcow2 \
  --container-format bare \
  --public

# 通过 URL 导入（需配置 glance-api 允许外部源）
openstack image create "ubuntu-22.04" \
  --location https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img \
  --disk-format qcow2 \
  --container-format bare \
  --public

# 上传 RAW 格式
openstack image create "centos-stream-raw" \
  --file CentOS-Stream-8-x86_64.raw \
  --disk-format raw \
  --container-format bare

# 上传 ISO 格式（用于安装引导）
openstack image create "windows-server-iso" \
  --file windows-server-2022.iso \
  --disk-format iso \
  --container-format bare
```

**create 参数详解**:

|         参数         | 说明             | 可选值                                |
| :------------------: | ---------------- | ------------------------------------- |
|   `--DISK-FORMAT`    | 磁盘格式         | qcow2 / raw / vmdk / vhd / iso        |
| `--CONTAINER-FORMAT` | 容器封装格式     | bare / ovf / aki / ari / ami / docker |
|      `--PUBLIC`      | 公开可见         | 所有项目可访问                        |
|     `--PRIVATE`      | 私有镜像         | 仅本项目和被共享项目可见              |
|    `--PROTECTED`     | 保护模式         | 禁止删除（需取消保护后方可删除）      |
|     `--PROPERTY`     | 自定义属性       | 如 `--property architecture=x86_64`   |
|      `--STORE`       | 指定存储后端     | 多后端环境下指定目标后端（Stein+）    |
|     `--MIN-DISK`     | 最小磁盘要求     | 启动该镜像所需的最小磁盘 GB           |
|     `--MIN-RAM`      | 最小内存要求     | 启动该镜像所需的最小内存 MB           |
|    `--SIGNATURE`     | 签名文件         | 镜像完整性签名                        |
|       `--TAG`        | Glance元数据标签 | 与元定义配合实现镜像分类              |

### 下载镜像

```bash
# 列出当前可用镜像
openstack image list
#                      └─ 列出所有可见镜像（可加 --PUBLIC / --PRIVATE 筛选）

# 查看镜像详细信息
openstack image show <IMAGE_ID_OR_NAME>
#                      └─ 显示镜像 ID、状态、disk_format、checksum 等完整信息

# 下载镜像到本地
openstack image save --file myimage.qcow2 <IMAGE_ID>
#                      ├─ --file 指定本地保存路径
#                      └─ 注意大镜像需要足够磁盘空间
```

### 镜像格式转换

```bash
# qemu-img 格式转换（在本地操作，非 OpenStack 命令）
qemu-img convert -f qcow2 -O raw source.qcow2 target.raw
#                      ├─ -f 源格式（SOURCE FORMAT）
#                      ├─ -O 目标格式（OUTPUT FORMAT）
#                      └─ 支持 qcow2/raw/vmdk/vhd/qed 等

qemu-img convert -f vmdk -O qcow2 source.vmdk target.qcow2

qemu-img convert -f raw -O qcow2 -c source.raw target.qcow2
#                      └─ -c 压缩目标镜像（COMPRESS）

# 查看镜像信息
qemu-img info image.qcow2
#                      └─ 输出格式类型、虚拟大小、实际占用、快照列表
```

---

## 四、镜像共享 👥

### 4.1 项目间共享

Glance 提供细粒度的镜像共享机制，支持三种可见性级别：

|    级别     | 权限要求   | 行为                             |
| :---------: | ---------- | -------------------------------- |
| **public**  | admin      | 所有项目自动可见                 |
| **private** | 镜像所有者 | 仅本项目和显式共享的项目可见     |
| **shared**  | 镜像所有者 | 通过 member 机制授予特定项目访问 |

### 4.2 CLI 操作

```bash
# 将镜像共享给指定项目
openstack image set --share --project <TARGET_PROJECT_ID> <IMAGE_ID>
#                      ├─ --share 开启镜像共享
#                      └─ --project 指定目标项目 ID

# 使用 member 管理
openstack image add project <IMAGE_ID> <TARGET_PROJECT_ID>
#                      └─ 将项目添加为镜像的 member

# 查看镜像的共享成员列表
openstack image member list <IMAGE_ID>
#                      └─ 列出所有可访问该镜像的项目及其状态

# 接受共享的镜像（目标项目操作）
openstack image set --accept <IMAGE_ID>
#                      └─ 项目接受共享镜像后即可使用

# 移除共享
openstack image remove project <IMAGE_ID> <TARGET_PROJECT_ID>
#                      └─ 撤销指定项目的镜像访问权限

# 批量查看镜像成员
openstack image member list --image <IMAGE_ID>
```

### 4.3 社区镜像共享最佳实践

- 基础公共镜像（如 Ubuntu Cloud、Cirros）设为 **public**
- 自定义业务镜像设为 **private**，通过 member 精确控制
- 使用 `--PROTECTED` 防止关键镜像被误删 🛡️

---

## 五、后端存储架构 💾

### 5.1 解耦设计 — glance.store 适配层

Glance 的核心架构优势在于 **镜像存储与镜像管理解耦**，通过 `glance.store` 实现插拔式后端：

```
┌─────────────────────────────────────────────────┐
│             用户 / Nova（REST API）               │
├─────────────────────────────────────────────────┤
│                  glance-api                      │
├─────────────────────────────────────────────────┤
│                glance.store 抽象层               │
│  ┌───────────┬───────────┬───────────┬────────┐ │
│  │  FILE     │  SWIFT    │  CEPH RBD │  其它  │ │
│  │  本地文件  │  对象存储  │  分布式存储 │  NFS.. │ │
│  └───────────┴───────────┴───────────┴────────┘ │
├─────────────────────────────────────────────────┤
│               glance.db (MySQL)                 │
│           存储镜像元数据 + locations              │
└─────────────────────────────────────────────────┘
```

### 5.2 后端存储配置

配置位于 `/etc/glance/glance-api.conf`：

```ini
[glance_store]
# 默认存储后端
default_backend = file

# 启用的存储后端列表（多后端支持，Stein+）
enabled_backends = file:file, swift:swift, ceph:rbd

# ─── 本地文件存储 ───
[file]
filesystem_store_datadir = /var/lib/glance/images/
filesystem_store_file_perm = 1

# ─── Swift 对象存储 ───
[swift]
swift_store_container = glance
swift_store_create_container_on_put = True
swift_store_auth_address = http://controller:5000/v3
swift_store_user = service:glance
swift_store_key = <PASSWORD>
swift_store_endpoint_type = internalURL

# ─── Ceph RBD 存储 ───
[rbd]
rbd_store_pool = images
rbd_store_user = glance
rbd_store_ceph_conf = /etc/ceph/ceph.conf
rbd_store_chunk_size = 8
rados_connect_timeout = 30
```

### 5.3 各后端对比

|       特性       |  本地 File   |      Swift      |       Ceph RBD       |
| :--------------: | :----------: | :-------------: | :------------------: |
|    **高可用**    |   ❌ 单点    |    ✅ 多副本    |     ✅ 多副本/EC     |
|    **分布式**    |      ❌      |       ✅        |          ✅          |
|     **性能**     |   ⭐⭐⭐⭐   |     ⭐⭐⭐      |      ⭐⭐⭐⭐⭐      |
|    **扩展性**    | 单机磁盘限制 |    水平扩展     |       水平扩展       |
|   **支持克隆**   |      ❌      |       ❌        |    ✅ (RBD clone)    |
|  **运维复杂度**  |  ⭐（简单）  |     ⭐⭐⭐      |        ⭐⭐⭐        |
|   **典型规模**   | 测试/小规模  |     中规模      |      大规模生产      |
| **数据路径优化** | Nova本地读取 | Nova从Swift拉取 | Nova RBD clone零拷贝 |

**生产推荐**: ✅ **Ceph RBD** > Swift > 本地文件

### 5.4 多后端存储（Stein+）

Stein 版本开始支持**多后端存储**，不同镜像可选不同后端：

```ini
[glance_store]
enabled_backends = fast:ssd, standard:sata, archive:nfs

[ssd]
filesystem_store_datadir = /var/lib/glance/fast_images/

[sata]
filesystem_store_datadir = /var/lib/glance/standard_images/

[nfs]
filesystem_store_datadir = /mnt/nfs/glance_images/
```

创建镜像时指定后端：

```bash
openstack image create "fast-io-image" \
  --file high_perf.qcow2 \
  --store ssd \
  --disk-format qcow2 \
  --container-format bare
```

### 5.5 Location 机制 — 解耦的关键 🔑

Glance 在数据库中记录镜像数据的 **存储位置 URI**，Nova 根据该 URI 直接访问：

```
1. 用户上传镜像 → glance-api 接收数据流
2. glance_store 根据配置写入后端存储
3. 数据库记录 location 和 checksum
4. Nova 启动 VM 时:
   - 从 glance 获取镜像元数据（ID, location, checksum）
   - 根据 location 直接从后端存储读取数据
   - 不需要经过 glance-api 中转（直接访问）
```

**核心优势**: Glance 不参与数据路径 — 虚机启动时 Nova 直接从 Ceph/Swift 拉取镜像数据，这是大规模 OpenStack 部署的关键性能优化。⚡

---

## 六、Ceph RBD 与 Glance 深度集成 🔗

### 6.1 RBD 零拷贝克隆

使用 Ceph RBD 后端时，Nova 可以直接从 Ceph 克隆镜像生成虚拟机磁盘：

```
GLANCE: images pool (rbd)
    │
    ├── image-a (template)
    │
    └── NOVA: rbd clone ──→ vms pool (rbd)
            ├── vm-1-disk (COW snapshot of image-a)
            ├── vm-2-disk (COW snapshot of image-a)
            └── vm-3-disk (COW snapshot of image-a)
```

这意味着：

- **无需实际拷贝镜像数据**，创建虚机秒级完成 🚀
- **写时复制**技术，存储空间按需分配
- 百台虚机从同一镜像启动仅占一份全量空间

### 6.2 Ceph 配置步骤

```bash
# 1. 创建 Ceph 池
ceph osd pool create images 128
ceph osd pool application enable images rbd

# 2. 创建 glance 用户并授权
ceph auth get-or-create client.glance \
  mon 'allow r' \
  osd 'allow class-read object_prefix rbd_children, allow rwx pool=images' \
  -o /etc/ceph/ceph.client.glance.keyring

# 3. 将密钥复制到计算节点
ceph auth get-or-create client.glance | \
  tee /etc/ceph/ceph.client.glance.keyring

# 4. 配置 glance-api.conf
# 见上方 [rbd] 配置段
```

### 6.3 Ceph 场景性能优势

|      指标       |     本地文件     |  Swift   |       Ceph RBD       |
| :-------------: | :--------------: | :------: | :------------------: |
| 100台VM启动时间 |      ~30min      |  ~15min  |       **~30s**       |
|  镜像存量占用   |       100%       |   100%   |    **~1%（COW）**    |
|  Nova读取路径   | NFS/CIFS远程挂载 | HTTP拉取 | **RBD kernel直接读** |
|    快照支持     |        ❌        |    ❌    |      ✅ RBD快照      |

---

## 七、镜像缓存策略 📦

### 7.1 glance-cache 机制

Glance 提供缓存层，将常用镜像缓存到本地磁盘，加速部署：

```bash
# 缓存管理命令
glance-cache-manage --host=controller list-cached
#                      └─ 列出当前缓存的所有镜像

glance-cache-manage --host=controller queue-image <IMAGE_ID>
#                      └─ 将指定镜像加入缓存队列

glance-cache-manage --host=controller delete-cached-images
#                      └─ 清除所有缓存镜像

glance-cache-manage --host=controller fetch-all
#                      └─ 立即拉取所有已排队的缓存镜像
```

### 7.2 缓存配置

```ini
# /etc/glance/glance-cache.conf
[ DEFAULT]
image_cache_dir = /var/lib/glance/image-cache
image_cache_driver = sqlite
image_cache_max_size = 10737418240
#                        └─ 缓存最大容量（10GB）
```

---

## 八、镜像签名与安全验证 🔒

### 8.1 镜像签名

OpenStack Mitaka 版本开始支持镜像签名验证：

```bash
# 1. 生成签名
openssl dgst -sha512 -sign private_key.pem \
  -out image.signature image.qcow2

# 2. 上传签名镜像
openstack image create "signed-image" \
  --file image.qcow2 \
  --sign-key-url http://keyserver/signing-key.pem \
  --sign-cert-url http://keyserver/certificate.pem \
  --signature image.signature \
  --disk-format qcow2 \
  --container-format bare
```

### 8.2 镜像完整性校验

```bash
# 查看镜像的 checksum 和哈希值
openstack image show <IMAGE_ID> -c checksum -c os_hash_algo -c os_hash_value

# 本地校验
sha512sum downloaded-image.qcow2
#                      └─ 对比与 glance image show 输出的 os_hash_value 是否一致
```

---

## 九、镜像生命周期管理 📋

### vm_image 状态机

```
                    ┌──────────┐
                    │ queued   │ ── 镜像元数据已创建，数据未上传
                    └────┬─────┘
                         │
                    ┌────▼─────┐
                    │ saving   │ ── 正在上传镜像数据
                    └────┬─────┘
                         │
                    ┌────▼─────┐
              ┌─────►  active  │ ── 镜像可用
              │     └────┬─────┘
              │          │
         ┌────▼───┐  ┌──▼──────┐
         │ deactivated│  │pending_delete│── 软删除
         └──────────┘  └─────────┘
                              │
                         ┌────▼─────┐
                         │  deleted │ ── 已删除（不可恢复）
                         └──────────┘
```

### 常用管理命令

```bash
# 列出镜像
openstack image list
#                      ├─ --PUBLIC 仅显示公共镜像
#                      ├─ --PRIVATE 仅显示私有镜像
#                      └─ --STATUS ACTIVE 按状态筛选

# 编辑镜像属性
openstack image set --name NEW_NAME \
  --property architecture=arm64 \
  --property hypervisor_type=kvm \
  <IMAGE_ID>

# 删除镜像
openstack image delete <IMAGE_ID_OR_NAME>
#                      └─ 若镜像被保护，需先去除 --PROTECTED

# 镜像去激活/重新激活（不删除但禁止使用）
openstack image deactivate <IMAGE_ID>
openstack image reactivate <IMAGE_ID>

# 查看镜像使用情况（哪些虚拟机在使用）
openstack server list --image <IMAGE_ID>
```

---

## 十、生产部署最佳实践 ✅

| 实践                 | 说明                                          |
| -------------------- | --------------------------------------------- |
| **生产用 Ceph RBD**  | 避免本地文件存储的单点故障，利用 COW 提升性能 |
| **上传前校验格式**   | `qemu-img info` 确认格式正确，避免启动失败    |
| **使用 SHA512 校验** | 镜像完整性验证，防止数据损坏                  |
| **设置 protected**   | 防止关键镜像被误删 🛡️                         |
| **细粒度共享**       | 敏感镜像设为 private + member 精确赋权        |
| **镜像签名**         | 对关键镜像启用签名验证                        |
| **清理未使用镜像**   | 定期清理废弃镜像，释放存储空间                |
| **监控 glance 日志** | `/var/log/glance/api.log` 关注上传/下载错误   |
| **多后端策略**       | 高频镜像放 SSD，归档镜像放 HDD/NFS            |
| **启用镜像缓存**     | 多计算节点场景显著加速部署                    |

---

## 十一、版本演进趋势 🚀

|      版本       | 核心变化                                    |
| :-------------: | ------------------------------------------- |
|     Newton      | 引入 glance_store 架构，registry 变为可选   |
|      Ocata      | 新增镜像去激活功能（deactivate/reactivate） |
|      Pike       | 支持多后端存储                              |
|     Queens      | 镜像导入/导出流程标准化                     |
|      Stein      | **多后端正式稳定**，image import API 成熟   |
|      Train      | 镜像签名验证增强                            |
|     Wallaby     | 支持镜像加密（使用 Barbican）               |
|  2025.1 Epoxy   | 强化镜像加密与签名校验                      |
| 2026.1 Gazpacho | 持续优化 Ceph 集成与性能提升                |

---

## 💡 技术解析

- **术语**: **glance_store** — Glance 的存储抽象层，独立的 oslo 库（`glance_store`），所有后端存储通过该插件化接口接入。这是实现镜像与后端存储解耦的关键架构组件。所有后端实现只需实现核心 CRUD 接口即可无缝集成。
- **术语**: **QCOW2** — QEMU Copy-On-Write v2，支持稀疏文件（仅分配实际写入的块）、内部快照链（snapshot chain）、写时复制克隆（backing file）、AES 加密和 zlib 压缩。相较于 QCOW（v1），QCOW2 支持快照和更大的虚拟磁盘。
- **术语**: **Copy-on-Write (COW)** — 写时复制技术，克隆时不复制源数据，仅写入新数据时才按需分配空间。Glance + Ceph 场景的核心性能保障，秒级创建百台虚机。
- **术语**: **RBD Clone** — Ceph RBD 的快照克隆机制，基于 COW 技术从镜像快照直接创生虚拟机磁盘，无需拷贝源数据，是 Nova 与 Glance 协同优化的核心亮点。
- **术语**: **Location (存储位置)** — glance.db 中记录镜像数据实际存储位置的 URI 字段（如 `rbd://pool/image/uuid`），Nova 根据该 URI 直接从后端读取镜像数据，实现 Glance 不参与数据路径。
- **术语**: **Container Format** — 镜像的容器封装格式，`bare` 表示无封装（裸镜像），`ovf` 表示 OVF 标准封装；`aki/ari/ami` 对应 AWS 内核/ramdisk/机器镜像。
- **术语**: **Multibackend (多后端)** — Stein+ 版本功能，`enabled_backends` 配置多个存储后端，创建镜像时通过 `--STORE` 参数指定目标后端，实现不同存储策略（SSD 高性能 / SATA 大容量 / NFS 归档）。
- **术语**: **Glance Cache** — 将远端存储的热门镜像缓存到本地磁盘的加速机制，通过 `glance-cache-manage` 管理，适用于 Swift 后端的多计算节点场景。Yoga 版本后已标记废弃，推荐使用 Ceph RBD 替代。
- **命令**: `openstack image create` — 创建并上传镜像，`--FILE` 指定本地路径，`--DISK-FORMAT` 指定 qcow2/raw/vmdk/iso，`--CONTAINER-FORMAT` 指定 bare/ovf，`--PUBLIC/--PRIVATE` 控制可见性，`--PROPERTY` 添加自定义元数据。
- **命令**: `openstack image save` — 将远程镜像下载到本地，`--FILE` 指定保存路径。注意大镜像需确保本地磁盘空间充足，且下载路径不宜跨网络（建议在计算节点或 controller 节点直接操作）。
- **命令**: `openstack image set --share` — 跨项目共享镜像，`--PROJECT` 指定目标项目 ID，目标项目需 `openstack image set --accept` 接受后方可使用。
- **命令**: `qemu-img convert` — 磁盘镜像格式转换工具，`-f` 指定源格式，`-O` 指定目标格式，`-c` 启用压缩（仅目标为 qcow2 时有效）。支持的格式对：qcow2↔raw↔vmdk↔vhd↔qed。

## 🔗 相关文档

[OpenStack Glance 镜像服务概念](Glance镜像服务概念.md) | [OpenStack Nova计算服务详解](OpenStack-Nova计算服务详解.md) | [OpenStack Keystone详解](OpenStack-Keystone详解.md) | [OpenStack Cinder块存储详解](OpenStack-Cinder块存储详解.md)
