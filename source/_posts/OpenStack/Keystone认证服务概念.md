---
title: "OpenStack Keystone认证服务概念"
description: "全面介绍OpenStack Keystone身份认证服务的核心概念，包括User、Project、Role、Domain、Endpoint、Token等关键组件的含义与作用"
tags: [OpenStack, Keystone, 认证服务, 身份管理, 云计算]
categories: [云计算组件]
date: 2026-05-16
aliases: ["OpenStack认证服务", "Keystone概念解析"]
---

# Keystone认证服务概念

Keystone 是 OpenStack 中的**身份认证服务**（Identity Service）👤，负责用户认证、授权和服务目录管理。所有 OpenStack 服务之间的交互都必须经过 Keystone，它是整个云平台的**安全中枢**。

### 什么是 Keystone

Keystone 提供三大核心功能：

- **认证（Authentication）** 🔐：验证用户身份的真实性
- **授权（Authorization）** 🛡️：确定用户是否有权限执行特定操作
- **服务目录（Service Catalog）** 📖：提供服务注册与发现能力，让各组件相互找到对方

---

### User（用户）

操作主体，可以是**人、服务或系统**。

- 用户是执行操作的最小实体，**本身不拥有资源**
- 用户名在所属 Domain 内唯一，不同 Domain 可以有同名用户
- 用户必须加入特定 Project 并分配 Role 后才能访问资源
- 典型服务用户：`nova`、`glance`、`cinder`、`neutron` 等（用于服务间通信）

### Project（项目）

**资源归属的基本单元**和隔离边界（V2 中称为 Tenant/租户）。

- 所有 OpenStack 资源（虚拟机、卷、镜像、网络等）都必须归属于某个 Project
- 一个 Project 可包含多个 User，每个 User 可分配不同 Role
- Project 名称在所属 Domain 内唯一
- 支持按 Project 设置配额（Quota）限制资源使用

### Role（角色）

定义用户在 Project 或 Domain 中的**操作权限级别**。

| 角色     | 权限说明                                 |
| -------- | ---------------------------------------- |
| `admin`  | 完全管理权限，是 member 和 reader 的超集 |
| `member` | 读写权限，可操作资源（是 reader 的超集） |
| `reader` | 只读权限，仅可查看资源                   |

- 角色存在**隐式继承关系**：`admin ⊃ member ⊃ reader`
- 授权机制 = Role + Policy（通过 `policy.json` 定义具体可执行的 API）

### Domain（域）

V3 API 引入的**高层容器**和命名空间，是 Project、User、Group 的顶层隔离单元。

- **Domain 名称全局唯一**（其他资源只需域内唯一）
- 系统预设 `Default` 域
- 不同 Domain 支持不同的认证后端（如一个用 SQL，一个用 LDAP）
- 实现**真正多租户（Multi-tenancy）**架构，客户可在自己域内独立管理资源

### Endpoint（端点）

服务的**网络访问 URL**，存储在服务目录（Service Catalog）中。

| 类型       | 用途           | 典型端口 |
| ---------- | -------------- | -------- |
| `public`   | 外部用户访问   | 5000     |
| `internal` | 内部服务间通信 | 5000     |
| `admin`    | 管理 API       | 35357    |

### Token（令牌）

认证通过后 Keystone 颁发的**字符串通行凭证**。

| 作用域         | 说明                         |
| -------------- | ---------------------------- |
| Unscoped       | 仅证明身份，无授权信息       |
| Project-scoped | 绑定到特定 Project（最常用） |
| Domain-scoped  | 绑定到特定 Domain            |
| System-scoped  | 管理全局资源                 |

**Token 格式演进**：

- `UUID Token` — 简单但需持久化到数据库，性能较差
- `PKI Token` — 包含大量信息，体积大，传输效率低
- `Fernet Token` ✅ — **当前默认**，AES256 加密 + SHA256 签名，无需持久化，轻量安全

### Group（组）

用户的集合容器（V3 新增）。

- 给 Group 分配 Role，组内所有用户自动继承该角色
- 避免逐一对每个用户分配角色的繁琐操作
- 类比 Linux 操作系统中的用户组

---

### 数据模型关系

```
Domain（域）[全局唯一]
├── Project（项目）[域内唯一] ← 资源归属
│   └── User + Role → 用户在项目中的权限
├── User（用户）[域内唯一] ← 认证主体
├── Group（组）[域内唯一] ← 用户集合
└── Role（角色）[域内唯一] ← 权限定义
```

### 典型认证流程

```text
User → 提交凭证（用户名/密码）
     → Keystone 验证身份（Authentication）
     → 返回 Token + 服务目录（Endpoint 列表）
     → 携带 Token 调用 Nova/Glance 等服务
     → 目标服务用 authtoken 中间件向 Keystone 验证 Token
     → 验证通过，执行操作
```

---

### 使用场景

- **多租户云平台** ☁️：通过 Domain 隔离不同租户，Project 隔离租户内资源
- **服务间通信** 🔄：Nova 调用 Glance 创建云主机时，使用服务用户 Token 进行认证
- **权限分级管理** 🛡️：admin 管理平台、member 使用资源、reader 只读审计

### 一句话概括

**Domain** 是顶层容器，**Project** 是资源隔离单元，**User** 是操作主体，**Role** 是权限级别，**Token** 是通行凭证，**Endpoint** 是服务入口。六者协同构成 OpenStack 完整的身份认证与授权体系。

## 🔗 相关文档

{% post_link OpenStack/OpenStack-Keystone详解 %} | {% post_link OpenStack/Nova计算服务概念 %} | {% post_link OpenStack/Swift对象存储服务概念 %} | {% post_link OpenStack/Glance镜像服务概念 %}
