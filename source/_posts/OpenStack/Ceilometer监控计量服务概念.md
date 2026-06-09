---
title: "OpenStack Ceilometer 监控计量服务概念"
description: "全面介绍OpenStack Ceilometer监控计量服务的核心概念、双采集机制（轮询与通知）、Pipeline数据处理管道及与告警计费系统的集成"
tags: [OpenStack, Ceilometer, 监控, 计量, Telemetry]
categories: [技术文档, 云计算]
date: 2026-05-16
aliases: ["Ceilometer监控", "OpenStack计量服务", "Telemetry数据采集"]
---

# OpenStack Ceilometer 监控计量服务概念

Ceilometer 是 OpenStack Telemetry 项目的**数据收集层**，负责采集云平台中各类资源的计量（Meter）和事件（Event）数据，输出给下游的 Gnocchi（时间序列存储）、Aodh（告警）、CloudKitty（计费）等组件，为性能调优和成本分析提供数据基础📊。

---

## 一、两种数据采集方式

| 方式 | 组件 | 原理 | 特点 |
| --- | --- | --- | --- |
| **Notification（通知）** | `ceilometer-agent-notification` | 监听消息队列（AMQP），消费 Nova/Cinder/Neutron 等服务发出的通知 | ✅ **推荐方式**，被动接收，对 API 无额外负载 |
| **Polling（轮询）** | `ceilometer-polling` | 主动通过 API 或 Hypervisor 定期拉取数据 | 适用于通知无法覆盖的指标（如 VM CPU 利用率） |

### Polling Agent 三种角色

| 代理类型 | 部署位置 | 采集方式 | 功能说明 |
| --- | --- | --- | --- |
| **Compute Agent** | 每个计算节点 | 通过 Hypervisor API（libvirt） | 收集 VM 实例资源使用数据（CPU、内存、磁盘 I/O 等） |
| **Central Agent** | 控制节点 | 通过 OpenStack 服务 REST API | 轮询 Neutron、Cinder、Swift 等服务，采集非实例级别资源数据 |
| **IPMI Agent** | 计算节点（需 IPMI 支持） | 通过 IPMI 协议 / ipmitool | 采集物理服务器传感器数据（温度、电压、风扇转速等） |

> 📝 **等价关系**：`ceilometer-agent-compute` ≡ `ceilometer-polling --polling-namespaces COMPUTE`，Central 和 IPMI 同理。

---

## 二、Pipeline 数据处理管道

采集的原始数据经过 Pipeline 三阶段处理⚙️：

```
Gathering（汇集） ➔ Transforming（转换） ➔ Publishing（发布）
```

### 1. Gathering（数据汇集）

从 Notification Agent 或 Polling Agent 接收原始计量数据与事件。

### 2. Transforming（转换计算）

支持多种转换器处理原始数据，例如：
- **rate-of-change**：将累计值转换为速率
- **arithmetic**：数学计算（CPU Time → CPU 利用率百分比）
- **aggregation**：聚合计算（平均值、最大值、最小值）

### 3. Publishing（发布输出）

处理后的数据可发布至以下目标端：

| 发布目标 | 说明 |
| --- | --- |
| **Gnocchi** | 时间序列数据库（推荐存储后端） |
| **Aodh** | 告警服务 |
| **Kafka** | 消息队列 |
| **HTTP** | REST 端点 |
| **UDP** | UDP 数据包 |
| **File** | 本地文件 |
| **Notifier** | 消息队列重新投递 |

所有 Pipeleine 规则通过 `pipeline.yaml` 配置文件定义，支持灵活的过滤、转换和多重发布策略。

---

## 三、架构组件一览🏗️

| 组件 | 功能说明 |
| --- | --- |
| **ceilometer-polling** | 统一轮询代理，负责主动采集数据 |
| **ceilometer-agent-notification** | 通知代理，监听消息队列消费通知 |
| **Gnocchi** | 时间序列数据存储（替代传统数据库） |
| **Aodh** | 基于计量数据的告警服务 |
| **Panko** | 事件存储（文档型数据） |

### 数据流概览

```
OpenStack Services（Nova / Cinder / Neutron / Glance / Swift / Keystone / Heat）
        │
        ├─── Notification bus（AMQP）──► Notification Agent ──┐
        │                                                     │
        ├─── REST APIs ◄── Central Agent ─────────────────────┤
        │                                                     │
        └─── Hypervisor ◄── Compute Agent ────────────────────┤
                        ◄── IPMI Agent  ──────────────────────┤
                                                              │
                                                              ▼
                                                     Pipeline Manager
                                                              │
                                              ┌───────────────┴───────────────┐
                                              ▼                               ▼
                                          Gnocchi（存储）              Aodh（告警）
                                      CloudKitty（计费）          HTTP/Kafka/文件
```

---

## 四、架构演进🔧

早期 Ceilometer 集采集、存储、告警于一体，单体架构导致性能瓶颈。自 Ocata 版本起完成**解耦**：

```
旧版：Ceilometer（采集 + 存储 + 告警）
新版：Ceilometer（采集） ➔ Gnocchi（存储） + Aodh（告警）
```

**关键变化**：
- `ceilometer-api` 和 `ceilometer-collector` 不再使用
- 存储委托给 **Gnocchi**（时间序列数据库）
- 告警委托给 **Aodh**（独立告警服务）
- 事件存储委托给 **Panko**

---

## 五、生产建议⚠️

- **通知优先**：Notification 机制对 API 服务无额外负载，优先使用
- **轮询间隔**：通过 `pipeline.yaml` 配置合理的 polling 间隔，避免过度请求 API
- **存储后端**：生产环境推荐 **Gnocchi** 替代传统数据库，性能显著提升
- **横向扩展**：Ceilometer 各 Agent 支持多实例部署，按需增加 Worker 节点
- **Jitter 配置**：`shuffle_time_before_polling_task` 参数添加随机抖动，避免惊群效应

---

## 六、技术术语💡

- **Telemetry** — OpenStack 遥测项目组，包含 Ceilometer、Gnocchi、Aodh、Panko 四个子项目
- **Meter** — 计量样本，如 `cpu_util`、`disk.read.bytes`、`network.incoming.bytes`
- **Event** — 事件记录，如虚拟机创建/删除/迁移等操作
- **Pipeline** — 可配置的 YAML 管道，定义数据如何被转换和发布
- **Gnocchi** — 时间序列数据库，Ceilometer 的推荐存储后端
- **Aodh** — 基于 Ceilometer 数据的告警服务，支持阈值、组合等告警规则
- **AMQP** — 高级消息队列协议，Ceilometer 各组件间通过 RabbitMQ 实现异步通信
- **Hypervisor** — 虚拟机监视器（如 KVM/libvirt），Compute Agent 通过其采集 VM 数据

## 🔗 相关文档

{% post_link OpenStack/OpenStack-Ceilometer监控计量详解 %} | {% post_link OpenStack/Nova计算服务概念 %} | {% post_link OpenStack/Keystone认证服务概念 %}
