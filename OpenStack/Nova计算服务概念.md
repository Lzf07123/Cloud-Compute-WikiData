---
title: "OpenStack Nova计算服务概念"
description: "全面介绍OpenStack Nova计算服务的核心概念，包括nova-api、nova-scheduler、nova-conductor、nova-compute四大组件架构，虚拟化技术支持（KVM/QEMU/Xen），虚拟机生命周期及调度策略"
tags: [OpenStack, Nova, 计算服务, 虚拟化, 调度策略, 云计算]
categories: [云计算组件]
date: 2026-05-16
aliases: ["OpenStack计算服务", "Nova概念解析"]
---

# Nova计算服务概念

Nova 是 OpenStack 中的**核心计算服务**（Compute Service）☁️，负责管理虚拟机（VM）实例的完整生命周期——创建、启动、停止、删除、迁移、快照等。它采用**分布式、无状态、消息驱动**的架构设计，各组件通过消息队列（RabbitMQ）异步通信，通过共享数据库协调状态。**状态保存在数据库中，不在服务进程中。**

---

### 一、四大核心组件架构

#### nova-api — 前端入口 🚪

**角色**：REST API 端点 / 请求编排器

- 接收所有用户/运维操作请求（如 `POST /servers` 创建VM）
- 通过 **Keystone** 进行身份认证 🔐
- 校验请求参数并执行策略（Policy）
- 将实例初始状态写入数据库（`vm_state=BUILDING`）
- 将任务派发至消息队列，由其他服务处理
- **无状态**，可水平扩展

#### nova-scheduler — 调度决策引擎 🧠

**角色**：确定**哪台物理计算节点**运行新创建的虚拟机

采用**两阶段算法**：

**阶段1：Filtering（过滤）**——运行可配置过滤链，剔除不合格主机

| 过滤器                                    | 作用                                                                      |
| ----------------------------------------- | ------------------------------------------------------------------------- |
| `RetryFilter`                             | 跳过之前调度失败的节点                                                    |
| `AvailabilityZoneFilter`                  | 遵循可用域（AZ）约束                                                      |
| `RamFilter` / `CoreFilter` / `DiskFilter` | 检查资源容量（支持超分比 `ram_allocation_ratio`、`cpu_allocation_ratio`） |
| `ComputeFilter`                           | 仅选择 nova-compute 健康的节点                                            |
| `ImagePropertiesFilter`                   | 镜像属性与 Hypervisor 类型匹配                                            |
| `ServerGroupAntiAffinityFilter`           | 实例分散到不同主机（高可用）                                              |
| `ServerGroupAffinityFilter`               | 实例集中到相同主机                                                        |
| `PciPassthroughFilter`                    | 仅选择支持 PCI 透传的节点                                                 |

**阶段2：Weighting（权重打分）**——对候选主机打分，最高分胜出（默认按剩余内存最多优先）

> 自 Newton 版本起，资源清单追踪由独立的 **Placement API** 服务负责。调度器向 Placement 查询分配候选节点，再应用自身的过滤器和权重。

#### nova-conductor — 安全代理与编排器 🛡️

**角色**：数据库访问代理 + 复杂工作流协调器

**引入目的**：解决安全隔离问题——计算节点**永远不能直接访问**中央数据库，防止计算节点被攻破时数据泄露。

| 职责           | 说明                                                                                             |
| -------------- | ------------------------------------------------------------------------------------------------ |
| **数据库代理** | nova-compute 的所有数据库读写通过 RPC 路由到 conductor；计算节点强制 `DISABLE_DB_ACCESS = True`  |
| **任务编排**   | 管理多步骤复杂操作：实例创建、冷迁移、在线迁移、Resize、Rebuild（通过内部 `ComputeTaskManager`） |

> 同样**无状态**，可水平扩展。⚠️ **禁止**部署在 nova-compute 所在节点。

#### nova-compute — 实际执行者 ⚙️

**角色**：真正的 **Hypervisor 管理器**——唯一直接操作虚拟机的组件

- 运行在每个**计算节点**（物理服务器）上
- 监听消息队列获取工作指令
- 调用 Hypervisor 驱动（如 `libvirt.LibvirtDriver` for KVM/QEMU）
- 管理实例全生命周期：创建、销毁、暂停、挂起、Resize、迁移、快照
- 定期向 **Placement API** 报告资源使用情况
- **不能直接访问数据库**——所有 DB 操作经 nova-conductor

---

### 二、完整工作流：创建一台虚拟机 🔄

```
用户 → nova-api          发送 POST /servers（含 flavor、image、network）
nova-api → Keystone      认证 Token
nova-api → 数据库        创建实例记录（vm_state=BUILDING）
nova-api → 消息队列      发送 boot RPC 消息
nova-scheduler ← 队列    拾取 boot 任务
nova-scheduler → Placement  查询 /allocation_candidates（找合适主机）
Placement → nova-scheduler   返回候选主机列表及资源信息
nova-scheduler → 数据库  过滤 + 权重打分，选出最佳主机
nova-scheduler → 消息队列 发送 build_and_run_instance 到目标主机
nova-compute ← 队列      拾取构建任务
nova-compute → conductor RPC 请求完整实例详情
conductor → 数据库        读取实例数据
conductor → nova-compute 返回实例对象
nova-compute → Placement  提交 /allocations（锁定资源）
nova-compute → Libvirt    调用 spawn()——创建磁盘、启动虚拟机
nova-compute → 数据库     更新 vm_state=ACTIVE（经 conductor）
```

> **关键特性**：这是**异步、最终一致性**架构。API 立即返回 `202 Accepted`，实际 VM 创建在后台进行。

---

### 三、虚拟化技术支持 💻

| Hypervisor         | 驱动方式         | 说明                                                     |
| ------------------ | ---------------- | -------------------------------------------------------- |
| **KVM**            | libvirt          | **最常用**，基于内核的虚拟化，将 Linux 转变为 Hypervisor |
| **QEMU**           | libvirt          | 机器模拟器，与 KVM 配合提供设备模拟和 I/O 操作           |
| **Xen**            | libvirt / XenAPI | Type-1 Hypervisor，支持 libvirt 驱动或 XenAPI            |
| **VMware vCenter** | VMwareAPI        | 集成 vSphere 虚拟化环境                                  |
| **Hyper-V**        | HyperVDriver     | Windows Server 虚拟化平台                                |
| **Ironic**         | IronicDriver     | **裸金属**管理（非虚拟化，直接管理物理机）               |
| **LXC/LXD**        | LXDDriver        | Linux 容器虚拟化                                         |

#### 关键虚拟化机制

- **KVM/QEMU 模式**: nova-compute 通过 `libvirt` 库调用 KVM/QEMU，libvirt 提供统一 API 屏蔽底层差异
- **Emulator Threads 策略**（Pike+ 版本）: `hw:cpu_emulator_threads` 规格标签可设置：
  - `share`（默认） — 模拟器线程与 vCPU 共享物理 CPU
  - `isolate` — 模拟器线程固定在专用物理 CPU（适合实时性要求高的场景）
- **Xen 特殊处理**: 重启 Xen 虚拟机时会先后发射 `STOPPED` 和 `STARTED` 事件，Nova 延迟处理 `STOPPED` 事件避免误停 VM

---

### 四、调度策略详解 🎯

#### 过滤阶段核心过滤器

| 过滤器                          | 作用场景                     |
| ------------------------------- | ---------------------------- |
| `RetryFilter`                   | 防止失败节点被重新选中       |
| `AvailabilityZoneFilter`        | 将实例调度到指定可用域       |
| `RamFilter`                     | 检查内存是否充足（含超分比） |
| `DiskFilter`                    | 检查磁盘空间                 |
| `CoreFilter`                    | 检查 vCPU 是否充足           |
| `ComputeCapabilitiesFilter`     | 根据计算节点额外特性过滤     |
| `ImagePropertiesFilter`         | 确保镜像与 Hypervisor 兼容   |
| `ServerGroupAntiAffinityFilter` | 实现实例反亲和性分散         |
| `ServerGroupAffinityFilter`     | 实现实例亲和性集中           |
| `NumInstancesFilter`            | 限制单节点最大实例数         |
| `IoOpsFilter`                   | 避免 I/O 繁忙节点            |
| `PciPassthroughFilter`          | 仅选择支持 PCI 透传节点      |

#### 权重打分

过滤后的候选主机进入权重阶段，默认策略：

- `ram_weight_multiplier`（默认 1.0）— 剩余内存越多权重越高
- 可自定义：`cpu_weight_multiplier`、`disk_weight_multiplier`

---

### 五、Cells v2 架构（生产部署） 🏗️

对于大规模部署（数百上千计算节点），Nova 使用 **Cells v2** 横向分区：

| 层级               | 组件                                                                    |
| ------------------ | ----------------------------------------------------------------------- |
| **全局（Global）** | nova-api、Placement API、全局数据库（记录 Cell 映射）                   |
| **每个 Cell**      | Cell 数据库（实例元数据）、Cell 消息队列、scheduler、conductor、compute |

- **Cell0** — 特殊 Cell，存放调度失败实例记录，避免污染主数据库
- 每个 Cell 拥有**独立的消息队列和数据库**，避免单点瓶颈
- 实现真正**水平扩展**能力

---

### 六、组件特性对比

| 服务               | 主要职责                     | 有状态？   | 可扩展？     | 数据库访问   |
| ------------------ | ---------------------------- | ---------- | ------------ | ------------ |
| **nova-api**       | REST 端点、请求校验          | 否         | 水平扩展     | 是（读写）   |
| **nova-scheduler** | 主机选择（过滤+权重）        | 否         | 水平扩展     | 是（只读）   |
| **nova-conductor** | 数据库代理、任务编排         | 否         | 水平扩展     | 是（读写）   |
| **nova-compute**   | VM 生命周期、Hypervisor 调用 | 是（本地） | 增加计算节点 | 经 conductor |

---

### 七、使用场景

- **虚拟机全生命周期管理** 🖥️：从创建、运行、迁移到销毁
- **多 Hypervisor 统一管理** 🔧：同一 Nova 集群可混用 KVM、VMware、Xen 等
- **弹性伸缩** 📈：水平扩展计算节点，结合调度策略优化资源利用率
- **高可用部署** 🛡️：Cells v2 + Anti-Affinity 策略实现跨物理机/跨可用域部署

### 一句话概括

**nova-api** 是入口，**nova-scheduler** 决定在哪运行，**nova-conductor** 安全代理数据库，**nova-compute** 是真正干活的。四者通过消息队列 + Placement API 协作，支撑起 OpenStack 的计算服务能力。

## 🔗 相关文档

[OpenStack Nova计算服务详解](OpenStack-Nova计算服务详解.md) | [OpenStack Keystone认证服务概念](Keystone认证服务概念.md) | [OpenStack Glance 镜像服务概念](Glance镜像服务概念.md) | [OpenStack Neutron网络概念](Neutron网络服务概念.md) | [OpenStack Cinder 块存储服务概念](Cinder块存储服务概念.md)
