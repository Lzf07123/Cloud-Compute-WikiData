---
title: "OpenStack Heat 编排服务概念"
description: "全面介绍OpenStack Heat编排服务的核心概念，包括HOT模板语法、Stack/Resource/Parameter三要素、架构组件及与AWS CloudFormation兼容性"
tags: [OpenStack, Heat, 编排服务, HOT, CloudFormation, 基础设施即代码]
categories: [云计算]
date: 2026-05-16
aliases: ["Heat编排服务", "OpenStack编排组件", "HOT模板详解"]
---

# OpenStack Heat 编排服务概念

Heat 是 OpenStack 的**核心编排服务**（Orchestration Service）🔥，通过声明式模板定义一组云资源及其依赖关系，实现**基础设施即代码（IaC）**，自动化完成资源的创建、更新与删除。

核心设计理念：将基础设施部署转化为**代码化模板**，实现可重复、可版本控制、可自动化编排的云资源管理。

---

## 一、HOT 模板语法

Heat Orchestration Template（HOT）是基于 **YAML** 的声明式模板格式，是 Heat 的核心编排语言📝。

### 模板顶层结构

```yaml
HEAT_TEMPLATE_VERSION: 2016-10-14
DESCRIPTION: 描述模板用途

PARAMETER_GROUPS:
  - LABEL: 网络配置
    PARAMETERS:
      - NETWORK_NAME
      - SUBNET_CIDR

PARAMETERS:
  NETWORK_NAME:
    TYPE: STRING
    LABEL: 网络名称
    DEFAULT: MY-NETWORK

RESOURCES:
  MY_NETWORK:
    TYPE: OS::NEUTRON::NET
    PROPERTIES:
      NAME: { GET_PARAM: NETWORK_NAME }

  MY_SERVER:
    TYPE: OS::NOVA::SERVER
    DEPENDS_ON: MY_NETWORK
    PROPERTIES:
      NAME: MY-INSTANCE
      FLAVOR: M1.SMALL
      IMAGE: CIRROS-0.6.2
      NETWORKS:
        - NETWORK: { GET_RESOURCE: MY_NETWORK }

OUTPUTS:
  INSTANCE_IP:
    DESCRIPTION: 实例IP地址
    VALUE: { GET_ATTR: [MY_SERVER, FIRST_ADDRESS] }
```

### 模板版本

| 版本              | 特性                          |
| ----------------- | ----------------------------- |
| `2013-05-23`      | HOT v1.0 初始版本             |
| `2014-10-16`      | 新增 `STR_REPLACE`、`GET_FILE`、`REPEAT` |
| `2015-04-30`      | 新增 `CONDITIONS` 条件判断    |
| `2016-10-14`      | 新增 `PARAMETER_GROUPS`       |
| `2018-03-02`      | 增强 `GET_ATTR` 嵌套引用      |

---

## 二、三大核心概念

### 1. Stack（栈）📦

**Stack** 是 Heat 的基本部署单元，一次模板部署即产生一个 Stack。

| 属性         | 说明                                  |
| ------------ | ------------------------------------- |
| **名称**     | 唯一标识一个 Stack                    |
| **模板**     | Stack 引用的 HOT 模板                 |
| **参数**     | 部署时传入的具体参数值                |
| **状态**     | `IN_PROGRESS` → `COMPLETE` / `FAILED` |
| **输出**     | 部署完成后返回的信息                  |

**Stack 生命周期：**

```
CREATE_IN_PROGRESS → CREATE_COMPLETE
                      ↘ (回滚) → ROLLBACK_COMPLETE
UPDATE_IN_PROGRESS → UPDATE_COMPLETE
DELETE_IN_PROGRESS → DELETE_COMPLETE
```

**嵌套 Stack**（Nested Stack）：通过 `OS::HEAT::STACK` 类型引用子模板，实现模块化编排。

### 2. Resource（资源）📋

**Resource** 是 Stack 内被管理的具体 OpenStack 云资源。资源类型命名格式：`OS::<服务>::<类型>`。

| 资源类型                        | 对应服务    | 说明                     |
| ------------------------------- | ----------- | ------------------------ |
| `OS::NOVA::SERVER`              | Nova        | 虚拟机实例               |
| `OS::NEUTRON::NET`              | Neutron     | 网络                     |
| `OS::NEUTRON::SUBNET`          | Neutron     | 子网                     |
| `OS::NEUTRON::PORT`             | Neutron     | 端口                     |
| `OS::NEUTRON::ROUTER`           | Neutron     | 路由器                   |
| `OS::CINDER::VOLUME`            | Cinder      | 云硬盘                   |
| `OS::GLANCE::IMAGE`             | Glance      | 镜像                     |
| `OS::HEAT::AUTOSCALING_GROUP`   | Heat 内置   | 自动伸缩组               |
| `OS::HEAT::RESOURCE_GROUP`      | Heat 内置   | 批量创建同类资源          |
| `OS::HEAT::STACK`               | Heat 内置   | 嵌套 Stack               |
| `OS::HEAT::SOFTWARE_CONFIG`     | Heat 内置   | 用户数据脚本配置          |

**资源间关系建立方式：**

- **`DEPENDS_ON`** — 显式声明依赖
- **`GET_RESOURCE`** — 引用同 Stack 内其他资源的 ID
- **`GET_ATTR`** — 获取资源的运行时属性
- **自动推理** — Heat 引擎根据 `PROPERTIES` 引用自动推断隐式依赖

### 3. Parameter（参数）🔧

**Parameter** 是模板的输入变量，让模板在不同环境间复用：

```yaml
PARAMETERS:
  INSTANCE_TYPE:
    TYPE: STRING
    LABEL: 实例规格
    DEFAULT: M1.SMALL
    CONSTRAINTS:
      - ALLOWED_VALUES: [M1.SMALL, M1.MEDIUM, M1.LARGE]
  ADMIN_PASSWORD:
    TYPE: STRING
    LABEL: 管理员密码
    HIDDEN: TRUE
    NO_ECHO: TRUE
```

| 参数类型                | 说明               |
| ----------------------- | ------------------ |
| `STRING`                | 字符串             |
| `NUMBER`                | 数字               |
| `JSON`                  | JSON 格式数据      |
| `COMMA_DELIMITED_LIST`  | 逗号分隔列表       |
| `BOOLEAN`               | 布尔值             |
| `MAP`                   | 键值对映射         |

**环境文件（Environment File）**：将参数值与模板分离，独立存储：

```yaml
# ENVIRONMENT.YAML
PARAMETERS:
  INSTANCE_TYPE: M1.LARGE
RESOURCE_REGISTRY:
  "OS::NOVA::SERVER": "CUSTOM_SERVER.YAML"
```

---

## 三、架构组件

```
用户 (CLI / Horizon / API)
        │
        ▼
  ┌──────────┐   ┌──────────────────┐
  │ HEAT-API │──▶│   HEAT-API-CFN   │── AWS CloudFormation 兼容 API
  └────┬─────┘   └────────┬─────────┘
       │                  │
       ▼                  ▼
  ┌──────────────────────────────┐
  │        HEAT-ENGINE           │ ← 核心编排引擎
  │  (依赖解析 + 任务编排 + 回滚) │
  └──────┬───────┬───────┬───────┘
         │       │       │
         ▼       ▼       ▼
    OpenStack 服务 (Nova / Neutron / Cinder / ...)
```

### heat-api — REST API 入口 🚪

- 暴露原生 Heat REST API（端口 8004）
- 接收模板提交与 Stack 操作请求
- 与 Keystone 集成进行认证鉴权

### heat-api-cfn — CloudFormation 兼容 API 🔄

- 暴露 AWS CloudFormation 兼容的 Query API（端口 8000）
- 允许 AWS CloudFormation 工具/脚本直接对接 OpenStack
- 提供从 AWS 迁移到 OpenStack 的便捷路径

### heat-engine — 核心编排引擎 ⚙️

| 职责           | 说明                                               |
| -------------- | -------------------------------------------------- |
| **模板解析**   | 解析并验证 HOT 模板语法                            |
| **依赖分析**   | 构建资源依赖 DAG 图，确定执行拓扑顺序              |
| **任务编排**   | 按依赖顺序逐步执行资源操作（并行创建无依赖资源）   |
| **回滚处理**   | 资源创建失败时自动回滚已成功的资源                  |
| **内置函数**   | 运行时求值 `GET_RESOURCE`、`GET_ATTR`、`GET_PARAM` |
| **收敛修正**   | `STACK UPDATE` 检测差异并执行增量变更              |

---

## 四、与 AWS CloudFormation 兼容性

Heat 在设计上兼容 AWS CloudFormation 的模板语法和 API 风格，便于混合云或从 AWS 迁移的场景🔄。

| 对比维度         | Heat                         | AWS CloudFormation          |
| ---------------- | ---------------------------- | --------------------------- |
| **模板格式**     | YAML（HOT）                  | YAML / JSON                 |
| **原生 API**     | RESTful（端口 8004）         | AWS API                     |
| **兼容 API**     | heat-api-cfn（端口 8000）    | -                           |
| **资源类型**     | `OS::*` 命名空间             | `AWS::*` 命名空间           |
| **状态管理**     | OpenStack Database           | AWS 托管                    |
| **回滚策略**     | 内建支持                     | 内建支持                    |

---

## 五、常用操作命令

```bash
# 创建 Stack
openstack stack create -T TEMPLATE.YAML -E ENVIRONMENT.YAML MY-STACK

# 列出 Stack
openstack stack list

# 查看 Stack 详情
openstack stack show MY-STACK

# 查看 Stack 资源列表
openstack stack resource list MY-STACK

# 更新 Stack
openstack stack update -T TEMPLATE.YAML -E ENVIRONMENT.YAML MY-STACK

# 删除 Stack
openstack stack delete MY-STACK

# 查看 Stack 事件（排错用）
openstack stack event list MY-STACK

# 查看模板输出
openstack stack output show MY-STACK OUTPUT_NAME
```

---

## 六、Heat vs Terraform 对比 ⚖️

| 维度           | Heat                    | Terraform                    |
| -------------- | ----------------------- | ---------------------------- |
| **云平台**     | 仅 OpenStack            | 多云（AWS/Azure/GCP 等）    |
| **模板语言**   | YAML（HOT）             | HCL                          |
| **状态管理**   | 数据库                  | 本地/远程状态文件（tfstate） |
| **变更预览**   | 无显式 Plan             | `TERRAFORM PLAN`             |
| **资源粒度**   | Stack 级整体部署        | 资源级独立管理               |

### 一句话概括

**Heat 是 OpenStack 的 IaC 编排引擎**——通过 HOT 模板定义 Stack（部署单元），Stack 包含 Resource（云资源）和 Parameter（输入参数），heat-engine 解析依赖 DAG 自动按序创建/更新/删除，实现基础设施部署的自动化与可重复。

## 🔗 相关文档

[OpenStack Heat（编排）详解](<OpenStack-Heat(编排)详解.md>) | [OpenStack Nova计算服务概念](Nova计算服务概念.md) | [OpenStack Neutron网络概念](Neutron网络服务概念.md) | [OpenStack Keystone认证服务概念](Keystone认证服务概念.md)
