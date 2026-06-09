---
title: "OpenStack Nova计算服务详解"
description: "全面解析OpenStack核心计算服务Nova的架构、虚拟化支持、虚拟机生命周期及调度策略"
tags: [OpenStack, Nova, 计算服务, 虚拟化, 调度]
categories: [云计算, OpenStack]
date: 2026-05-16
aliases: ["Nova架构详解", "OpenStack计算服务"]
---

# OpenStack Nova计算服务详解

### 一、Nova核心架构概览

Nova是OpenStack的计算编排控制器，采用 **分布式、无状态、消息驱动** 的设计哲学。所有核心组件通过 **消息队列（RabbitMQ）** 通信，状态统一存储在 **SQL数据库** 中。✨

#### 架构全景图

```
                         ┌──────────┐
                         │  Keystone │
                         └────┬─────┘
                              │ AUTH
                         ┌────▼─────┐
  USER ──REST API──►  NOVA-API   │
                         └────┬─────┘
                              │ PUBLISH MSG
                    ┌─────────▼──────────┐
                    │   MESSAGE QUEUE     │ ◄── RABBITMQ
                    │  (OSLO.MESSAGING)  │
                    └──┬───┬──────┬───┬──┘
                       │   │      │   │
              ┌────────┘   │      │   └──────────┐
              ▼            ▼      ▼              ▼
        NOVA-SCHEDULER  NOVA-CONDUCTOR  NOVA-COMPUTE N
              │            │              │
              │            │              └──► LIBVIRT/KVM
              │            │              └──► XEN
              │            │              └──► VMWARE
              │            │
              │            └──► SQL DATABASE
              │
              └──► PLACEMENT API ──► RESOURCE INVENTORY
```

#### 核心组件一览表

| 组件               | 状态类型  | 是否可水平扩展  | 职责一句话                            |
| ------------------ | --------- | --------------- | ------------------------------------- |
| **nova-api**       | 无状态 ✅ | 是 ✅           | RESTful API入口网关，对接Keystone认证 |
| **nova-scheduler** | 无状态 ✅ | 是 ✅           | 决定虚拟机跑在哪台物理机上            |
| **nova-conductor** | 有状态 🔶 | 是 ✅           | 数据库安全代理层，Compute不得直连DB   |
| **nova-compute**   | 有状态 🔶 | 是（每节点1个） | 真实执行虚拟机操作的工作节点          |

---

### 二、四大核心组件深度解析 🧩

#### 1️⃣ nova-api — 全局入口网关

**职责**: 接收并处理用户的RESTful API请求，是外部访问Nova的唯一途径。

```
┌─────────────── NOVA-API ───────────────┐
│                                         │
│  ① 接收 POST /SERVERS 请求              │
│  ② Keystone鉴权验证                     │
│  ③ Quota配额校验                        │
│  ④ DB写入初始记录（VM_STATE=BUILDING）   │
│  ⑤ 发布BOOT消息 → RabbitMQ              │
│  ⑥ 返回202 Accepted                     │
│                                         │
└─────────────────────────────────────────┘
```

**关键特性**:

- 支持OpenStack原生API + Amazon EC2兼容API
- 可通过keepalived + haproxy实现高可用负载均衡
- 响应格式：创建类请求返回 `202 Accepted`（异步非阻塞）

---

#### 2️⃣ nova-scheduler — 调度决策者 🎯

调度算法采用经典的**两步走**模型：

```
ALL COMPUTE NODES
       │
       ▼
┌──────────────────────┐
│  FILTERING 过滤阶段   │ ◄── 筛选满足条件的候选节点
│  RAMFILTER           │     内存是否充足？
│  COREFILTER          │     CPU核数是否够？
│  DISKFILTER          │     磁盘空间是否够？
│  AVAILABILITYZONEFIL │     可用区约束？
│  NUMATOPOLOGYFILTER  │     NUMA拓扑匹配？
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  WEIGHTING 权重阶段   │ ◄── 对候选节点排序选最优
│  RAMWEIGHER          │     内存最大优先（默认）
│  CPUWEIGHER          │     CPU最充裕优先
│  DISKWEIGHER         │     磁盘最大优先
│  GROUPWEIGHER        │     亲和性/反亲和性
└──────────┬───────────┘
           │
           ▼
    TARGET HYPERVISOR
```

**常用过滤器详解**:

| 过滤器名称                          | 过滤逻辑                    | 典型场景          |
| ----------------------------------- | --------------------------- | ----------------- |
| `RamFilter`                         | 计算节点可用内存 ≥ 规格要求 | 通用场景          |
| `CoreFilter`                        | 可用CPU核数 ≥ 规格要求      | 通用场景          |
| `DiskFilter`                        | 可用磁盘空间 ≥ 规格要求     | 通用场景          |
| `AvailabilityZoneFilter`            | 匹配用户指定的可用区        | 多AZ部署          |
| `NUMATopologyFilter`                | 检查NUMA亲和性              | CPU密集型工作负载 |
| `PciPassthroughFilter`              | 检查PCI透传设备可用性       | GPU/NIC透传       |
| `ImagePropertiesFilter`             | 镜像属性匹配（架构类型等）  | 异构架构          |
| `AggregateInstanceExtraSpecsFilter` | 主机聚合标签匹配            | GPU池/SSD池       |

**调度策略组合示例**:

```bash
# 调度器配置文件 /ETC/NOVA/NOVA.CONF
[DEFAULT]
scheduler_driver = task_based
scheduler_available_filters = nova.scheduler.filters.all_filters
scheduler_default_filters = RamFilter,CoreFilter,DiskFilter,AvailabilityZoneFilter,NUMATopologyFilter
scheduler_weight_classes = nova.scheduler.weights.all_weighers
```

---

#### 3️⃣ nova-conductor — 安全代理层 🛡️

**引入背景**: G版本之前，nova-compute直接操作数据库带来了巨大的安全风险和升级兼容性问题。

**核心价值**: 禁止nova-compute直接访问数据库，所有数据库操作必须经Conductor代理。

```
NOVA-COMPUTE                     NOVA-CONDUCTOR                   SQL DB
    │                                  │                            │
    │── RPC CALL（请求实例信息）──────►   │                            │
    │                                  │── SQL QUERY ──────────────►│
    │                                  │◄── RETURN DATA ────────────│
    │◄── RPC RESPONSE（返回实例对象）───  │                            │
```

**关键职责**:

- **数据库代理**: Compute对数据库的所有读写必须走Conductor
- **复杂流程协调**: 负责创建、冷迁移、热迁移、resize、rebuild等长时间运行流程
- **滚动升级兼容**: 新旧版本Compute节点可通过Conductor通信
- **依赖链**: nova-compute必须依赖nova-conductor启动成功才能正常运行

---

#### 4️⃣ nova-compute — 实际执行者 ⚙️

**部署方式**: 每台物理计算节点上运行一个实例，是Nova的核心"工人"。

```
┌────────────── NOVA-COMPUTE ──────────────┐
│                                           │
│  ┌─────────────┐  ┌──────────────────┐   │
│  │ MQ CONSUMER  │  │ RESOURCE TRACKER │   │
│  │ 消费消息队列  │  │ 资源统计&上报    │   │
│  └──────┬──────┘  └──────────────────┘   │
│         │                                  │
│  ┌──────▼──────┐  ┌──────────────────┐   │
│  │ COMPUTEDRIVER │  │ PERIODIC TASKS   │   │
│  │ 虚拟化驱动层 │  │ 周期性状态同步    │   │
│  └──────┬──────┘  └──────────────────┘   │
│         │                                  │
│  ┌──────▼──────┐                          │
│  │  HYPERVISOR  │                          │
│  │ KVM / XEN .. │                          │
│  └─────────────┘                          │
└───────────────────────────────────────────┘
```

---

### 三、虚拟化支持详解 🖥️

Nova通过 **ComputeDriver驱动模式** 屏蔽底层异构硬件的差异，所有Hypervisor对上层透明：

```
NOVA-COMPUTE
     │
     └── COMPUTEDRIVER（驱动抽象基类）
           ├── LibvirtDriver    → KVM / QEMU（主力生产驱动）
           ├── XenAPIDriver     → Xen XCP
           ├── VMwareVCDriver   → VMware vSphere ESXi
           ├── HyperVDriver     → Microsoft Hyper-V
           ├── IronicDriver     → 裸金属（无虚拟化层）
           └── LXCDriver        → Linux容器
```

#### KVM — 主力生产Hypervisor 🏆

```
USER REQUEST ──► NOVA-COMPUTE ──► LIBVIRT ──► KVM KERNEL MODULE
                           │                  └── INTEL VT-X / AMD-V
                           │
                           └── QEMU（DEVICE EMULATION）
                                ├── VIRTIO-NET（网卡）
                                ├── VIRTIO-BLK（磁盘）
                                └── UEFI / BIOS（固件）
```

**性能关键配置**:

```bash
# /ETC/NOVA/NOVA.CONF 虚拟化配置
[libvirt]
virt_type = kvm
cpu_mode = host-passthrough
cpu_model_extra_flags = pcid,ssbd,mds=on
images_type = qcow2
live_migration_permit_post_copy = true
live_migration_permit_auto_converge = true
```

**技术优势**:

- 利用 Intel VT-x / AMD-V 硬件虚拟化扩展，性能接近裸机
- 支持 NUMA 亲和性、CPU pinning、大页内存（Huge Pages）
- 通过 libvirt 统一管理（virsh命令行或API）

#### QEMU — 设备模拟器 🛠️

- **与KVM的关系**: QEMU提供设备模拟+用户态接口，KVM提供硬件加速，二者常组合使用（qemu-kvm）
- **纯模拟模式**: 无KVM加速时可纯软件模拟（性能较低，常用于开发测试）
- **2026.1 Gazpacho亮点**:
  - `live_migration_parallel_connections` 并行热迁移
  - IOThread默认启用，磁盘I/O从vCPU线程卸载
  - UEFI固件自动选择（支持Secure Boot / AMD SEV）
  - QEMU AIO模式可配置（支持NFS/FC/iSCSI卷）

#### Xen — 裸机型Hypervisor 🛡️

- **架构**: Type-1（裸机型），直接运行在硬件上
- **组成**: Domain 0（管理域）+ Domain U（用户虚拟机）
- **驱动**: 通过 `XenAPIDriver` 或 `LibvirtDriver`（Xen模式）集成
- **特点**: 安全性极高，半虚拟化性能优越，市场份额较少

#### 其他虚拟化平台

| 平台             | 集成方式                          | 适用场景               |
| ---------------- | --------------------------------- | ---------------------- |
| VMware vSphere   | `VMwareVCDriver` → vCenter API    | 企业VMware存量迁移     |
| Hyper-V          | `HyperVDriver` → WMI / PowerShell | Windows生态整合        |
| Ironic（裸金属） | `IronicDriver` → PXE + IPMI       | 高性能计算、DB裸机部署 |
| LXC              | `LXCDriver` → LinuxContainers     | 轻量容器化场景         |

---

### 四、虚拟机完整生命周期 📋

#### 创建流程详解（核心链路）

以 `POST /servers` 创建虚拟机为例，展示全链路15步交互：

```
STEP 1: 用户发起请求
  POST /SERVERS
  └── NOVA-API 接收
       ├── KEYSTONE 鉴权
       ├── QUOTA 校验
       ├── DB写入: VM_STATE=BUILDING
       └── BOOT消息 → RABBITMQ（RPC CAST）

STEP 2: 调度决策
  NOVA-SCHEDULER 消费消息
       ├── PLACEMENT API 查询可用资源
       ├── FILTER + WEIGHT 选最优节点
       └── BUILD_AND_RUN_INSTANCE → 目标节点（RPC CAST）

STEP 3: 实例化执行
  NOVA-COMPUTE 接收消息
       ├── RPC CALL → NOVA-CONDUCTOR 获取数据
       ├── PLACEMENT API 资源预留
       ├── GLANCE 下载镜像 → 创建磁盘
       ├── NEUTRON 配置网络（创建PORT、分配IP）
       ├── CINDER 对接云硬盘（若有）
       └── HYPERVISOR SPAWN() → 启动VM

STEP 4: 同步状态
  NOVA-COMPUTE → DB更新
       ├── VM_STATE = ACTIVE
       └── TASK_STATE = NONE
```

#### 生命周期状态机 🔄

```
                    ┌──────────┐
                    │ BUILDING │
                    └────┬─────┘
                         │
                    ┌────▼─────┐
         ┌──────────► ACTIVE   ◄──────────┐
         │          └────┬─────┘          │
         │               │                │
    ┌────▼───┐    ┌──────▼──────┐   ┌─────▼─────┐
    │ PAUSED │    │ RESIZED     │   │  STOPPED  │
    └────┬───┘    └──────┬──────┘   └─────┬─────┘
         │               │                │
         │          ┌────▼─────┐    ┌─────▼─────┐
         │          │ VERIFY   │    │  SHELVED  │
         │          └──────────┘    └─────┬─────┘
         │                               │
         │                          ┌────▼─────┐
         └──────────────────────────►  DELETED │
                                     └──────────┘
```

| vm_state   | 含义                   | 触发操作      |
| ---------- | ---------------------- | ------------- |
| `BUILDING` | 正在构建               | `nova boot`   |
| `ACTIVE`   | 正常运行中             | 创建成功      |
| `PAUSED`   | 已暂停（内存保留）     | `nova pause`  |
| `STOPPED`  | 已关机（磁盘保留）     | `nova stop`   |
| `RESIZED`  | 规格调整中             | `nova resize` |
| `SHELVED`  | 已搁置（释放计算资源） | `nova shelve` |
| `ERROR`    | 出错状态               | 操作失败      |
| `DELETED`  | 已删除                 | `nova delete` |

**常用管理命令**:

```bash
# 创建并启动虚拟机
openstack server create --flavor FLAVOR_ID --image IMAGE_ID --network NET_ID VM_NAME

# 查看虚拟机列表
openstack server list

# 暂停/恢复
openstack server pause VM_NAME
openstack server unpause VM_NAME

# 关机/开机
openstack server stop VM_NAME
openstack server start VM_NAME

# 搁置/取消搁置（释放资源保留磁盘）
openstack server shelve VM_NAME
openstack server unshelve VM_NAME

# 删除
openstack server delete VM_NAME

# 查看控制台日志
openstack console log show VM_NAME

# 获取VNC控制台URL
openstack console url show VM_NAME
```

---

### 五、Placement API — 资源管理独立服务 🗂️

**引入版本**: Newton版本从nova-scheduler中剥离

**核心功能**:

```
PLACEMENT API
     │
     ├── RESOURCE PROVIDER  （资源提供者）→ 物理计算节点
     ├── INVENTORY          （库存）       → CPU/RAM/DISK总量
     ├── ALLOCATION         （分配）       → 已分配给哪些实例
     └── TRAIT              （特征标签）    → GPU/NUMA/加密等标记
```

- **统一资源管理**: 不仅服务于Nova，也服务于Cyborg（加速器）、Magnum（容器）等
- **资源类型**: CPU、内存、磁盘、GPU、FPGA、NUMA节点、PCI设备
- **Trait标签**: 支持自定义硬件特征标签，实现trait-based调度

---

### 六、Cells v2 — 大规模部署架构 🌐

**解决痛点**: 单点RabbitMQ和MySQL在高并发场景下成为瓶颈。

```
                ┌────────────────────────────────┐
                │       API CELL（全局控制面）      │
                │  NOVA-API + PLACEMENT API       │
                │  全局CELL映射表 DB                │
                └──────┬──────────────┬───────────┘
                       │              │
         ┌─────────────▼──┐    ┌─────▼─────────────┐
         │  CELL 1 DC-A   │    │  CELL 2 DC-B      │
         │  CELL DATABASE  │    │  CELL DATABASE     │
         │  CELL MQ        │    │  CELL MQ           │
         │  NOVA-SCHEDULER │    │  NOVA-SCHEDULER    │
         │  NOVA-CONDUCTOR │    │  NOVA-CONDUCTOR    │
         │  NOVA-COMPUTE N │    │  NOVA-COMPUTE N    │
         └────────────────┘    └────────────────────┘
```

**核心优势**:

- **Cell0**: 特殊Cell，记录失败或未调度实例，保护主数据库
- **隔离性**: 每个Cell独立MQ+DB，单点故障不扩散
- **水平扩展**: 新增Cell即可扩展集群规模
- **跨数据中心**: 适用于多地域部署

---

### 七、RabbitMQ通信模型详解 📨

Nova的RPC通信基于 `oslo.messaging` 库，支持三种交换模式：

| Exchange类型     | 路由模式 | 用途                         |
| ---------------- | -------- | ---------------------------- |
| `nova`           | Topic    | 主体通信：指向特定组件队列   |
| `reply.<UUID>`   | Direct   | 同步RPC call的结果返回       |
| `compute.fanout` | Fanout   | 广播消息（如调度器缓存刷新） |

**路由Key示例**:

```bash
# 路由到特定计算节点
ROUTING_KEY: COMPUTE.HOSTNAME01  →  HOST01的NOVA-COMPUTE队列

# 路由到调度器
ROUTING_KEY: SCHEDULER           →  NOVA-SCHEDULER队列

# 路由到控制器
ROUTING_KEY: CONDUCTOR           →  NOVA-CONDUCTOR队列
```

---

### 八、2025-2026 版本演进 🚀

| 版本                | 发布日期 | 类型     | 核心亮点                      |
| ------------------- | -------- | -------- | ----------------------------- |
| **2025.1 Epoxy**    | 2025.04  | SLURP ✅ | 支持跳跃升级，打好基础        |
| **2025.2 Flamingo** | 2025.10  | 常规     | 机密计算、一次性设备透传      |
| **2026.1 Gazpacho** | 2026.04  | SLURP ✅ | 9大新特性，重点对标VMware替换 |

#### 2026.1 Gazpacho 亮点 🌟

**高频重点功能**:

```ini
# 并行热迁移配置
[libvirt]
live_migration_permit_post_copy = true
live_migration_permit_auto_converge = true
live_migration_parallel_connections = 3
```

| 特性                    | 影响               | 技术要点                                    |
| ----------------------- | ------------------ | ------------------------------------------- |
| 🔄 **并行热迁移**       | 大幅提升迁移速度   | QEMU multifd多连接传输，需QEMU≥10.1.0       |
| 🔐 **vTPM热迁移**       | Windows 11迁移支持 | Barbican托管TPM密钥，RPC传输                |
| ⚡ **IOThread默认启用** | 磁盘I/O性能提升    | 每个VM独立IOThread，RT实例CPU绑定           |
| ⏳ **异步挂载卷**       | API响应速度提升    | microversion 2.101，HTTP 202非阻塞          |
| 🧵 **原生线程化**       | 服务性能与稳定性   | scheduler/api默认原生线程，eventlet逐步退役 |
| 🛑 **优雅关闭**         | 零中断升级         | SIGTERM处理，二次RPC Server保障             |
| 🔧 **UEFI自动选择**     | 简化固件配置       | libvirt自动选型，支持Secure Boot/AMD SEV    |
| 📜 **OpenAPI全覆盖**    | API可观测性提升    | JSON Schema覆盖所有端点                     |
| 🏷️ **Trait调度**        | 精准匹配硬件特征   | 自定义标签匹配GPU、FPGA、加密加速器         |

---

### 九、架构设计哲学总结 🎯

| 设计原则                 | 具体实现                                      |
| ------------------------ | --------------------------------------------- |
| **无状态与水平扩展**     | nova-api/scheduler无状态，状态全部存储在MySQL |
| **异步通信与最终一致性** | 大量使用RPC cast而非call，高吞吐量优先        |
| **驱动化架构**           | ComputeDriver抽象层，屏蔽底层Hypervisor差异   |
| **资源供应分离**         | Placement API独立管理资源，调度与资源解耦     |
| **安全代理模式**         | nova-conductor作为DB代理，防线前移            |
| **分片隔离**             | Cells v2分片部署，故障域隔离                  |
| **滚动升级兼容**         | conductor屏蔽DB差异，支持混合版本部署         |

---

### 💡 技术解析

- **术语**: **RPC Cast vs Call** — Cast是单向异步（fire-and-forget），发送后立即返回；Call是双向同步，等待响应。Nova内部默认用Cast保证吞吐量，Compute查询数据时用Call同步等待。
- **术语**: **Placement API** — Newton版本从Scheduler剥离的独立资源追踪服务，统一管理CPU/内存/磁盘/GPU/NUMA的分配与释放。
- **术语**: **Resource Tracker** — nova-compute内部资源统计模块，周期性刷新本节点可用资源并上报Placement API。
- **术语**: **Cells v2** — 大规模部署的分片架构，每个Cell独立MQ + DB，Cell0记录失败实例，API Cell负责全局路由。
- **术语**: **NUMA** — 非统一内存访问架构，CPU访问本地内存vs远端内存延迟不同。NUMA感知调度确保vCPU和内存分配到同一NUMA节点。
- **术语**: **vTPM** — 虚拟可信平台模块，为虚拟机提供硬件级安全能力。Windows 11强制要求TPM 2.0。
- **术语**: **Trait-based Scheduling** — 通过自定义特征标签匹配工作负载与硬件特性的调度策略，如标记计算节点是否包含GPU、FPGA、加密加速器。
- **命令**: `openstack server create` — 创建虚拟机实例，常用参数：`--FLAVOR` 指定规格，`--IMAGE` 指定镜像，`--NETWORK` 指定网络，`--KEY-NAME` 指定密钥对，`--AVAILABILITY-ZONE` 指定可用区。
- **命令**: `openstack server shelve` — 搁置虚拟机，释放计算资源但保留磁盘和内存快照，节省资源的同时可随时恢复。

## 🔗 相关文档

{% post_link OpenStack/Nova计算服务概念 %} | {% post_link OpenStack/OpenStack-Keystone详解 %} | {% post_link OpenStack/OpenStack-Neutron网络服务详解 %} | {% post_link OpenStack/OpenStack-Cinder块存储详解 %}
