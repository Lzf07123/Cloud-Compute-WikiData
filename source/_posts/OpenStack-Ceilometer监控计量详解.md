---
title: "OpenStack Ceilometer（监控计量）详解"
description: "全面介绍OpenStack Ceilometer监控计量服务的数据采集机制（轮询与通知）、Pipeline处理、Gnocchi时序存储、Aodh告警集成及CloudKitty计费系统"
tags: [OpenStack, Ceilometer, 监控, 计量, Gnocchi, Aodh, CloudKitty, Telemetry]
categories: [技术文档]
date: 2026-05-16
aliases: ["Ceilometer监控服务", "OpenStack遥测详解", "Telemetry服务"]
---

# OpenStack Ceilometer（监控计量）详解

### 一、Ceilometer 概述 📊

Ceilometer 是 OpenStack Telemetry（遥测）服务的核心组件，采用 **Agent 架构**，所有服务均可水平扩展。核心职责是**采集云平台中所有资源的计量数据**，为监控、告警、计费、性能分析提供数据基础。

**Telemetry 生态组件**：

| 组件           | 职责                               |
| -------------- | ---------------------------------- |
| **Ceilometer** | 数据采集（Polling + Notification） |
| **Gnocchi**    | 时序数据存储（Time-Series DBaaS）  |
| **Aodh**       | 告警服务（基于阈值触发动作）       |
| **Panko**      | 事件存储（已逐步废弃）             |
| **CloudKitty** | 计费引擎（Rating-as-a-Service）    |

---

### 二、数据采集两大机制 🔄

#### 1. Notification Agent（通知代理）— 推荐方式 ✅

**原理**：OpenStack 各组件在执行操作时向消息队列发送通知，Notification Agent 监听并转换为 Samples/Events。

**工作流程**：

1. 各服务发通知到消息队列（topic: `notifications.info`、`notifications.sample` 等）
2. Notification Agent 加载 `ceilometer.notification` 命名空间的 Listener 插件
3. 根据 `event_type` 过滤，分发给对应 Endpoint 处理
4. 通过 Pipeline 进行转换（Transform）和发布（Publish）

**Meter 配置示例（meters.yaml）** 📋

```yaml
metric:
  - name: "disk.read.bytes"
    event_type: "compute.instance.create.end"
    type: "cumulative"
    unit: "B"
    volume: "$.payload.size"
    resource_id: "$.payload.instance_id"
```

| 特性     | 说明                          |
| -------- | ----------------------------- |
| 触发方式 | 被动监听消息队列              |
| 数据来源 | 其他 OpenStack 服务发出的通知 |
| 负载影响 | 无额外负载（推荐）            |
| 适用场景 | 操作事件（创建/删除/快照等）  |

#### 2. Polling Agent（轮询代理）— 补充方式 ⏱️

**原理**：对于通知无法覆盖的指标（如 VM CPU/内存使用率等资源使用数据），Polling Agent **定期主动轮询** 获取。

**三种 Polling Agent**：

| Agent             | 命名空间                  | 部署位置     | 采集目标                                                     |
| ----------------- | ------------------------- | ------------ | ------------------------------------------------------------ |
| **Compute Agent** | `ceilometer.poll.compute` | 每个计算节点 | 轮询本地 Hypervisor（libvirt/KVM），采集 VM CPU/内存/磁盘 IO |
| **Central Agent** | `ceilometer.poll.central` | 控制节点     | 轮询各 OpenStack 服务 REST API（网络、存储等）               |
| **IPMI Agent**    | `ceilometer.poll.ipmi`    | 计算节点     | 轮询 IPMI 传感器数据、硬件功耗                               |

> **注意**：Kilo 版本起统一为 `ceilometer-polling`，通过 `--polling-namespaces` 参数区分。

**Polling 配置示例（polling.yaml）** 📋

```yaml
sources:
  - name: "compute_source"
    interval: 300 # 轮询间隔（秒）
    meters:
      - "cpu_util"
      - "memory.resident"
      - "disk.read.bytes"
    resources:
      - "list of resource urls"
    discovery:
      - "local_instances"
```

| 特性     | 说明                                |
| -------- | ----------------------------------- |
| 触发方式 | 主动定时轮询                        |
| 数据来源 | Hypervisor / REST API / IPMI        |
| 负载影响 | 对 API 服务有负载（需合理配置间隔） |
| 适用场景 | 资源持续使用数据（CPU/内存/磁盘）   |

---

### 三、Pipeline 数据处理管道 📦

无论是 Notification 还是 Polling 采集的数据，都经过 Pipeline 统一处理：

```
采集（Agent）→ 转换（Transform）→ 发布（Publish）→ 目标存储
```

#### 1. 数据转换（Transform）

支持的转换器：

| 转换器              | 说明         | 典型场景       |
| ------------------- | ------------ | -------------- |
| **rate_of_change**  | 计算变化速率 | CPU 使用率增量 |
| **delta**           | 计算差值     | 网络流量增量   |
| **unit_conversion** | 单位换算     | byte → MB      |
| **arithmetic**      | 算术运算     | 聚合计算       |
| **accumulator**     | 累积计算     | 累积流量       |

#### 2. 数据发布（Publish）— 支持多发布者

| Publisher      | 目标                   | 典型场景             |
| -------------- | ---------------------- | -------------------- |
| **gnocchi**    | Gnocchi 时序数据库     | 推荐，长期存储与查询 |
| **notifier**   | 消息队列               | 外部系统消费         |
| **http/https** | REST API               | 对接第三方系统       |
| **kafka**      | Apache Kafka           | 高吞吐流处理         |
| **prometheus** | Prometheus Pushgateway | 容器化监控           |
| **file**       | 本地文件               | 调试                 |
| **udp**        | UDP 数据包             | 轻量传输             |

#### 3. Pipeline 配置示例（pipeline.yaml） 🔧

```yaml
sources:
  - name: "cpu_source"
    interval: 300
    meters:
      - "cpu_util"
    sinks:
      - "cpu_sink"
sinks:
  - name: "cpu_sink"
    transformers:
      - name: "rate_of_change"
    publishers:
      - "gnocchi://"
      - "notifier://"
```

---

### 四、Gnocchi 时序数据存储 🗄️

Gnocchi 替代了 Ceilometer 早期数据库存储后端，提供高性能时序数据存储和查询。

| 特性                           | 说明                                      |
| ------------------------------ | ----------------------------------------- |
| **归档策略（Archive Policy）** | 定义数据的保留周期和聚合粒度              |
| **查询性能**                   | O(1) 时间复杂度查询                       |
| **存储空间**                   | 可预测、可控                              |
| **高可用**                     | 支持水平扩展                              |
| **资源类型**                   | 支持自定义 `resource_type` 和 `attribute` |

#### 归档策略示例

```bash
gnocchi archive-policy create \
  --back-window 0 \
  --granularity 60:1h \
  --granularity 3600:30d \
  --granularity 86400:365d \
  name: 'low-resolution'
```

**归档策略解释**：

| 粒度              | 保留时长 | 说明               |
| ----------------- | -------- | ------------------ |
| `60s`（1 分钟）   | 1 小时   | 最细粒度，实时监控 |
| `3600s`（1 小时） | 30 天    | 中期趋势分析       |
| `86400s`（1 天）  | 365 天   | 长期容量规划       |

#### 常用查询命令

```bash
gnocchi measures show -r RESOURCE_ID -m METRIC_NAME
gnocchi measures show --aggregation mean -r RESOURCE_ID -m cpu_util
gnocchi resource list --type instance
gnocchi status
```

---

### 五、Aodh 告警系统集成 🚨

Aodh 基于 Ceilometer 采集、Gnocchi 存储的数据，提供灵活的阈值告警服务。

#### 告警类型

| 告警类型                                     | 说明                     |
| -------------------------------------------- | ------------------------ |
| `gnocchi_aggregation_by_resources_threshold` | 按资源聚合阈值（最常用） |
| `gnocchi_aggregation_by_metrics_threshold`   | 按指标聚合阈值           |
| `event`                                      | 事件类型告警             |
| `combination`                                | 组合多个告警条件         |
| `threshold`                                  | 简单阈值告警（兼容旧版） |

#### 支持的动作

| 动作类型 | 格式            | 说明                                            |
| -------- | --------------- | ----------------------------------------------- |
| 日志     | `log://`        | 记录告警日志                                    |
| Webhook  | `webhook://URL` | HTTP/HTTPS 回调（对接邮件、短信、PagerDuty 等） |
| 消息队列 | `notifier://`   | 发送到消息队列                                  |
| 复合     | 组合多个动作    | 同时触发多种通知                                |

#### 告警创建示例

```bash
aodh alarm create \
  --type gnocchi_aggregation_by_resources_threshold \
  --name cpu_high \
  --metric cpu_util \
  --threshold 80 \
  --comparison-operator ge \
  --evaluation-periods 3 \
  --period 300 \
  --aggregation-method avg \
  --resource-type instance \
  --query '{"=": {"id": "INSTANCE_UUID"}}' \
  --alarm-action 'webhook://https://hooks.example.com/alarm' \
  --ok-action 'webhook://https://hooks.example.com/ok'
```

**参数说明**：

| 参数                       | 含义                 |
| -------------------------- | -------------------- |
| `--threshold 80`           | 阈值 80%             |
| `--comparison-operator ge` | 大于等于触发         |
| `--evaluation-periods 3`   | 连续 3 次触发        |
| `--period 300`             | 评估周期 300s        |
| `--alarm-action`           | 告警触发时执行的动作 |
| `--ok-action`              | 告警恢复时执行的动作 |

---

### 六、CloudKitty 计费系统集成 💰

#### 计量计费三步骤

| 步骤                   | 组件                 | 说明                   |
| ---------------------- | -------------------- | ---------------------- |
| **① Metering（计量）** | Ceilometer → Gnocchi | 采集并存储资源使用数据 |
| **② Rating（定价）**   | CloudKitty           | 根据计费策略计算费用   |
| **③ Billing（出账）**  | 外部计费系统         | 生成账单，提供 API     |

#### CloudKitty 架构

```
Ceilometer/Gnocchi → Tenant Fetcher → Collector → Rating Engine → Storage → Dashboard/API
```

| 模块               | 功能                                   |
| ------------------ | -------------------------------------- |
| **Tenant Fetcher** | 获取所有租户信息                       |
| **Collector**      | 从 Ceilometer/Gnocchi 拉取资源使用数据 |
| **Rating Engine**  | 计费引擎，支持多种模块                 |
| **Storage**        | 存储计费结果（InfluxDB 等）            |
| **Dashboard**      | Horizon 面板可视化                     |

#### Rating Engine 计费模块

| 模块         | 说明                                    | 适用场景                      |
| ------------ | --------------------------------------- | ----------------------------- |
| **hashmap**  | 基于标签/项目的固定定价，通过键值对匹配 | 标准定价，按规格/镜像固定价格 |
| **pyscript** | Python 脚本自定义策略，灵活度最高       | 复杂计费逻辑，差异化定价      |
| **noop**     | 无操作，返回零费用                      | 测试用                        |

#### Dynamic Pollster 高级采集

对于自定义计费场景，Ceilometer 提供 **Dynamic Pollster**，可在运行时动态定义新采集指标，无需重启代理：

```yaml
# polling.yaml — dynamic pollster 示例：采集卷的 IOPS
sources:
  - name: "volume_iops"
    interval: 60
    meters:
      - "volume.iops"
```

> 配合 CloudKitty 的 hashmap 规则，可实现**基于标签（tag）的差异计费**（如按项目、按客户级别、按操作系统类型差异化定价）。

---

### 七、整体数据流总览 🌐

```
┌──────────────────────────────────────────────────────────────────┐
│                     OpenStack 各组件                               │
│  Nova / Glance / Cinder / Neutron / Swift / Keystone / Heat      │
└───────┬────────────────────────────────────────┬─────────────────┘
        │ 发送通知                                 │ 提供 REST API
        ▼                                         ▼
┌────────────────────┐                  ┌──────────────────────────┐
│ Notification Agent  │                  │ Polling Agent             │
│ （监听消息队列）      │                  │ （compute/central/ipmi）  │
│ ↓ 转换为 samples     │                  │ ↓ 轮询生成 samples        │
└─────────┬──────────┘                  └──────────┬───────────────┘
          │                                        │
          └──────────┬──────────────────────────────┘
                     ▼
          ┌──────────────────────┐
          │   Pipeline 处理       │
          │ （Transform + Publish）│
          └──────────┬───────────┘
                     ▼
     ┌───────────────┬───────────────┬───────────────┐
     ▼               ▼               ▼               ▼
  Gnocchi         Aodh           CloudKitty       外部系统
  （时序存储）     （阈值告警）     （计费引擎）      （Kafka/HTTP）
     │               │               │
     ▼               ▼               ▼
  监控面板         告警通知         账单/报表
  （Grafana）    （邮件/Webhook）   （客户计费）
```

---

### 八、性能调优 🚀

| 优化项               | 建议                                     | 说明                                     |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| **采集方式选择**     | 优先使用 Notification                    | 减少 API 负载，仅在必要时开启 Polling    |
| **Polling 间隔**     | 根据指标重要性设置 60s-600s              | CPU 使用率可 60s，磁盘容量可 600s        |
| **Pipeline 批处理**  | 增大 `batch_size`                        | 提高发布效率（默认 100 可增至 500-1000） |
| **Gnocchi 归档策略** | 合理配置聚合粒度                         | 细粒度短期、粗粒度长期，控制存储成本     |
| **水平扩展**         | 增加 Polling Agent、Gnocchi-metricd 实例 | 根据数据量线性扩展                       |
| **Kafka 缓冲**       | 引入 Kafka 发布者                        | 解耦采集与存储，应对流量峰值             |
| **消息队列优化**     | 使用 RabbitMQ 或 Kafka 集群              | 提高消息吞吐能力                         |

**Pipeline 批处理配置优化**：

```ini
[notification]
batch_size = 500
batch_timeout = 5

[collector]
workers = 4
```

---

### 九、常用 CLI 命令 🖥️

#### Ceilometer 命令

```bash
ceilometer meter-list
ceilometer sample-list -m cpu_util
ceilometer statistics -m cpu_util -p 3600
```

#### Gnocchi 命令

```bash
gnocchi metric list
gnocchi measures show -r RESOURCE_ID -m cpu_util
gnocchi archive-policy list
gnocchi resource list --type instance
```

#### Aodh 命令

```bash
aodh alarm list
aodh alarm show ALARM_ID
aodh alarm update --name ALARM_ID --threshold 90
aodh alarm delete ALARM_ID
```

#### CloudKitty 命令

```bash
openstack rating module list
openstack rating hashmap service create --name compute
openstack rating hashmap field create --service compute \
  --key flavor --type flat
openstack rating hashmap mapping create \
  --field-id FIELD_ID --value m1.small --cost 0.5
```

---

### 十、最佳实践 💡

1. **优先使用 Notification 采集**，减少对 OpenStack API 的额外负载
2. **合理配置 Polling 间隔**，重要指标（CPU）用短间隔，次要指标（磁盘容量）用长间隔
3. **使用 Gnocchi 归档策略**控制存储成本，细粒度数据短期保留，粗粒度数据长期保留
4. **开启 Pipeline 批处理**，增大 `batch_size` 提升吞吐量
5. **引入 Kafka 缓冲层**，解耦采集与存储，应对大规模集群流量峰值
6. **水平扩展 Agent**，Compute Agent 随计算节点自动扩展，Central Agent 根据 API 负载调整
7. **告警策略设置连续评估周期**（`evaluation-periods`），避免误告警
8. **CloudKitty 计费规则**需手动创建 hashmap 或 pyscript 规则，否则不产生计费结果
9. **利用 Dynamic Pollster** 动态定义自定义采集指标，无需重启代理服务
10. **监控 Ceilometer 自身性能**，关注消息队列积压和 Pipeline 处理延迟
