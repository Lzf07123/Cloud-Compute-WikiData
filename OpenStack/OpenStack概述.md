---
title: "OpenStack 概述"
description: "全面介绍 OpenStack 的起源、版本演进及核心价值，涵盖 NASA 与 Rackspace 的合作背景、版本历史与开源生态"
tags: [OpenStack, 云计算, IaaS, 开源基础设施]
categories: [技术文档]
date: 2026-05-16
---

# OpenStack 概述

OpenStack 是一个开源的云计算管理平台项目，提供基础设施即服务（IaaS）解决方案，支持公有云、私有云和混合云的建设与管理。

## 一、起源

OpenStack 于 2010 年 7 月由 NASA 和 Rackspace 合作发起。

### 1. 背景

- **NASA** 贡献了其 Nebula 平台的代码，发展成为 **Nova**（计算组件），负责虚拟服务器部署和业务计算模块
- **Rackspace** 贡献了其 Cloud Files 平台的代码，发展成为 **Swift**（对象存储组件），负责分布式云存储模块

### 2. 发展里程碑

```text
2010.07  NASA 与 Rackspace 联合宣布 OpenStack 项目
2010.10  发布首个版本 Austin（Nova + Swift）
2012.09  成立 OpenStack 基金会（非营利组织）
2020     宣布更名为 Open Infrastructure Foundation
```

- 项目以 Apache 许可证 2.0 授权发布
- 核心代码超过 1000 万行，由 8000+ 开发者贡献了 50 万+ 变更

## 二、版本演进

OpenStack 采用字母序单词作为版本代号，每半年发布一个新版本。2023 年起改为年份.序号命名格式。

### 早期版本（2010–2015）

| 版本 | 时间 | 核心新增 |
|------|------|---------|
| **Austin** | 2010.10 | 首个版本，核心：Nova + Swift |
| **Bexar** | 2011.02 | Glance（镜像服务）、IPv6、Hyper-V |
| **Essex** | 2012.04 | Horizon（仪表板）、Keystone（身份服务） |
| **Folsom** | 2012.09 | Neutron（网络）、Cinder（块存储） |
| **Grizzly** | 2013.04 | 230+ 新功能，Cells 分布式集群 |
| **Havana** | 2013.10 | Ceilometer（计量）、Heat（编排） |
| **Icehouse** | 2014.04 | Trove（数据库服务） |
| **Kilo** | 2015.04 | Ironic（裸金属部署） |

### 近期版本（2023–至今）

| 版本 | 时间 | 备注 |
|------|------|------|
| **2023.1 Antelope** | 2023.03 | 启用新命名规则 |
| **2024.1 Caracal** | 2024.04 | 33 个核心服务 |
| **2024.2 Dalmatian** | 2024.10 | 33 个核心服务 |
| **2025.1 Epoxy** | 2025.04 | 35 个核心服务 |

## 三、核心价值

### 1. 开源

- 采用 **Apache 2.0** 许可证，代码完全免费开放
- 社区拥有来自 100+ 国家的数万名开发者和 500+ 企业（Intel、IBM、华为、Red Hat、Cisco 等）

### 2. 可扩展

- 采用**水平扩展**架构，无需专有硬件
- 支持从小型单节点到大规模数据中心的各种规模
- 支持 KVM、Xen、Hyper-V、Docker/LXC 等多种虚拟化技术

### 3. API 驱动

- 各服务之间通过统一 RESTful API 调用，实现系统松耦合
- 组件内部服务之间通过 AMQP（消息队列）交互
- 用户可通过 Web 界面（Horizon）、命令行工具或 RESTful API 管理云资源
- 插件架构：Neutron 支持多种网络后端，Cinder 支持多种存储后端

### 4. 核心组件一览

| 类别 | 组件 | 功能 |
|------|------|------|
| 计算 | Nova | 虚拟机实例生命周期管理 |
| 对象存储 | Swift | 大规模分布式对象存储 |
| 镜像服务 | Glance | 虚拟机镜像查找与检索 |
| 身份服务 | Keystone | 身份验证与服务令牌管理 |
| 网络服务 | Neutron | 网络虚拟化，支持 SDN |
| 块存储 | Cinder | 持久化块存储设备管理 |
| 仪表板 | Horizon | Web 管理门户 |
| 编排服务 | Heat | 云基础设施自动化部署 |
| 裸金属 | Ironic | 物理裸机服务器管理 |

## 架构交互流程

```text
用户 / Horizon / CLI / API 客户端
          ↕  RESTful API
     ┌──────────────────┐
     │   Keystone       │
     │  (身份认证)       │
     └──────────────────┘
          ↕  RESTful API
     ┌────┬────┬────┬────┐
     │Nova│Swift│Glance│Cinder│
     └─┬──┴─┬──┴─┬──┴─┬──┘
       │    │    │    │
     ┌─┴────┴────┴────┴─┐
     │  Neutron          │
     │  (网络服务)        │
     └───────────────────┘
          ↕  AMQP 消息队列
        服务内部通信
```

## 总结

OpenStack 始于 2010 年 NASA 与 Rackspace 的合作，历经 15 年发展为全球领先的开源 IaaS 平台，核心价值在于**开源生态、水平可扩展、API 驱动的松耦合架构**。

## 🔗 相关文档

[OpenStack架构详解：四大节点分工与多节点协同](OpenStack架构与分工.md) | [OpenStack Nova计算服务概念](Nova计算服务概念.md) | [OpenStack Swift 对象存储服务概念](Swift对象存储服务概念.md) | [OpenStack Cinder 块存储服务概念](Cinder块存储服务概念.md) | [OpenStack Neutron网络概念](Neutron网络服务概念.md) | [OpenStack Keystone认证服务概念](Keystone认证服务概念.md) | [OpenStack Glance 镜像服务概念](Glance镜像服务概念.md) | [私有云 OpenStack 学习路径](../学习路线与课程/私有云学习路径.md) | [JSON 文档格式概述](../数据格式与标准/JSON文档格式概述.md)
