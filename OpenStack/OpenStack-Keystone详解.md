---
title: "OpenStack Keystone详解"
description: "深入解析OpenStack Keystone身份认证服务的核心概念（User/Project/Role/Domain/Endpoint/Token）、认证流程与最佳实践"
tags: [OpenStack, Keystone, 认证服务, 云平台, 身份管理]
categories: [云计算, 技术教程]
date: 2026-05-16
aliases: ["Keystone认证详解", "OpenStack身份认证"]
---

# OpenStack Keystone详解

### Keystone角色定位

Keystone是OpenStack云平台的**身份认证核心组件**，作为整个平台的"统一门卫"🔐。用户访问任何服务（Nova计算、Glance镜像、Cinder块存储等）都必须先通过Keystone认证。Keystone一旦宕机，所有服务都将瘫痪。

---

### 六大核心概念

|     概念     | 说明                                           | 关键约束                            |
| :----------: | ---------------------------------------------- | ----------------------------------- |
|   **User**   | 访问OpenStack的个人或服务账户                  | 用户名在所属Domain内唯一            |
| **Project**  | 资源（计算/存储/网络）的容器（V2时代称Tenant） | 项目名在所属Domain内唯一            |
|   **Role**   | 权限集合，决定User对Project资源的操作级别      | 全局唯一名称                        |
|  **Domain**  | 顶层虚拟容器，包含Users/Groups/Projects        | Domain Name全局唯一                 |
| **Endpoint** | 服务可访问的网络地址（URL）                    | 三类接口：Public / Internal / Admin |
|  **Token**   | 认证通过后签发的身份凭证字符串                 | 有时效性（默认1h），可撤销          |

**关键解读** 📌：

- **User权限**通过"User + Project + Role"三元组定义，缺一不可
- **Domain**提供多租户隔离边界，不同部门可划分独立Domain
- **三类Endpoint**：Public面向外部用户 / Internal服务间通信 / Admin管理员专用

---

### 认证流程详解

```
                  ┌─────────────────────────────────────────────┐
                  │               OpenStack 云平台               │
                  │                                             │
  ┌───────┐     ① POST /v3/auth/tokens     ┌──────────┐     ⑥ Token     ┌─────────┐
  │       │ ──────────────────────────────→ │          │ ←──验证请求──→ │         │
  │       │     ② ← 返回 Unscoped Token     │ Keystone │                │  Nova   │
  │ 用户  │                                 │  (认证)  │     ⑦ ← 返回   │ (计算)  │
  │       │     ③ POST /v3/auth/tokens      │          │    用户信息    │         │
  │       │ ─────────── (带 Scope) ────────→ │          │ ─────────────→ │         │
  │       │                                 └──────────┘                └─────────┘
  │       │     ④ ← 返回 Scoped Token +                              ┌─────────┐
  │       │        Service Catalog + Roles                            │ Glance  │
  │       │                                                           │ (镜像)  │
  │       │     ⑤ Nova API 调用                                       │         │
  │       │ ──────── X-Auth-Token: <SCOPED_TOKEN> ─────────────────→ │         │
  └───────┘                                                           └─────────┘
```

#### 标准V3认证流程（七步）

| 步骤  | 动作               | 说明                                                              |
| :---: | ------------------ | ----------------------------------------------------------------- |
| **①** | 用户提交凭证       | `POST /v3/auth/tokens` — 携带 `username + password` 或 `API Key`  |
| **②** | 返回Unscoped Token | 仅证明身份，不含Service Catalog和Role，不可直接访问服务           |
| **③** | 请求Scoped Token   | 指定Scope（`project`或`domain`），绑定到具体Project/Domain        |
| **④** | 返回Scoped Token   | 附带完整Service Catalog（所有服务地址）+ Roles（当前Project权限） |
| **⑤** | 访问目标服务       | Scoped Token放入HTTP头 `X-Auth-Token` 请求Nova/Glance等服务       |
| **⑥** | 服务验证Token      | 通过`keystonemiddleware`中间件向Keystone校验Token有效性           |
| **⑦** | 返回用户上下文     | Keystone返回User ID、Project ID、Roles，服务据此执行策略鉴权      |

#### Token类型演进 🚀

| 类型           | 存储方式       | 验证方式                       |      状态       |
| -------------- | -------------- | ------------------------------ | :-------------: |
| **UUID**       | 持久化到数据库 | 每次查询数据库                 |    ❌ 已弃用    |
| **Fernet**     | 不持久化       | 本地加密验证（需Keystone在线） | ✅ **生产默认** |
| **PKI / PKIZ** | 不持久化       | 本地证书签名验证               |    ❌ 已弃用    |
| **JWS**        | 不持久化       | 非对称签名验证                 |     ⚠️ 可选     |

#### Token作用域对比

| 作用域             | 含Service Catalog? | 含Roles? | 适用场景                               |
| ------------------ | :----------------: | :------: | -------------------------------------- |
| **Unscoped**       |         ❌         |    ❌    | 仅证明身份，获取Scoped Token的前置步骤 |
| **Project-scoped** |         ✅         |    ✅    | **日常操作（最常用）**                 |
| **Domain-scoped**  |        部分        |    ✅    | Domain级别管理操作                     |

---

### Keystone内部架构

#### 六大内部服务

```
┌─────────────────────────────────────────────────────────────┐
│                      Keystone 服务架构                       │
│                                                             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ Identity │  │ Resource │  │Assigment │  │  Token   │   │
│  │ Service  │  │ Service  │  │ Service  │  │ Service  │   │
│  │ (用户/组) │  │ (项目/域) │  │ (角色分配)│  │ (Token)  │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
│                                                             │
│  ┌──────────┐  ┌──────────┐                                 │
│  │ Catalog  │  │  Policy  │                                 │
│  │ Service  │  │ Service  │                                 │
│  │(端点注册)│  │(授权引擎)│                                 │
│  └──────────┘  └──────────┘                                 │
└─────────────────────────────────────────────────────────────┘
```

| 内部服务       | 管理实体                | 职责                                           |
| -------------- | ----------------------- | ---------------------------------------------- |
| **Identity**   | Users、Groups           | 身份凭据验证（支持SQL / LDAP / AD / 联合认证） |
| **Resource**   | Projects、Domains       | 资源容器管理                                   |
| **Assignment** | Roles、Role Assignments | 角色CRUD及User-Project-Role映射                |
| **Token**      | Tokens                  | Token签发、验证、撤销                          |
| **Catalog**    | Services、Endpoints     | 服务注册与端点发现                             |
| **Policy**     | Policies、Rules         | 基于规则的授权引擎                             |

#### 认证后端选项

| 后端                          |     模式     | 适用场景                              |
| ----------------------------- | :----------: | ------------------------------------- |
| **SQL**                       |     读写     | 默认，Keystone管理全部身份            |
| **LDAP / AD**                 | 只读（推荐） | 对接企业已有目录服务                  |
| **SAML 2.0 / OpenID Connect** |     联合     | 与外部IdP（Okta、Azure AD、ADFS）集成 |
| **Keystone-to-Keystone**      |     联合     | 多OpenStack云联邦信任                 |

---

### CLI操作速查 🛠️

```bash
# 🔹 认证测试
openstack token issue
#                      └─ 请求并签发一个新的 Scoped Token，验证当前认证凭据是否有效

# 🔹 服务发现
openstack catalog list
#                      └─ 列出所有已注册服务的 Endpoint（Public / Internal / Admin）

# 🔹 用户管理
openstack user create --domain DEFAULT --password <PASSWORD> <USERNAME>
#                      ├─ --domain 指定用户所属的域（若不指定则使用 default 域）
#                      ├─ --password 设置用户登录密码
#                      └─ <USERNAME> 用户名，在所属域内唯一
openstack user list
#                      └─ 列出当前域下所有用户（可加 --domain 指定目标域）
openstack user show <USER_ID>
#                      └─ 查看指定用户的完整信息（含 ID、Domain、启用状态等）

# 🔹 项目管理
openstack project create --domain DEFAULT <PROJECT>
#                      ├─ --domain 指定项目所属的域
#                      └─ <PROJECT> 项目名称，在所属域内唯一
openstack project list
#                      └─ 列出所有项目及其所属域

# 🔹 角色分配（核心：User + Project + Role）
openstack role add --user <USER> --project <PROJECT> <ROLE>
#                      ├─ --user 指定目标用户
#                      ├─ --project 指定目标项目
#                      └─ <ROLE> 要分配的角色名（如 admin、member、reader）
openstack role assignment list --names --user <USER>
#                      ├─ --names 以名称而非 ID 形式显示（更易读）
#                      └─ --user 筛选指定用户的角色分配列表

# 🔹 域管理
openstack domain list
#                      └─ 列出所有域及其启用/禁用状态
openstack domain create <DOMAIN>
#                      └─ 创建一个新域（Domain Name 全局唯一）
openstack domain set --disable <DOMAIN>
#                      ├─ --disable 将域设为禁用状态（删除域的前置条件）
#                      └─ 域被禁用后，其下的 User 和 Project 也将无法使用
openstack domain delete <DOMAIN>
#                      └─ 删除指定域（必须先禁用）

# 🔹 端点管理
openstack endpoint list
#                      └─ 列出所有服务的 Endpoint（含服务类型、区域、URL）
openstack endpoint create --region <REGION> <SERVICE> <TYPE> <URL>
#                      ├─ --region 指定区域（如 RegionOne）
#                      ├─ <SERVICE>  服务名称（如 nova、glance）
#                      ├─ <TYPE>     端点类型（public / internal / admin）
#                      └─ <URL>      端点访问地址（如 https://controller:8774/v2.1）
```

---

### 生产安全配置 🔒

| 配置项                                   | 说明                     |
| ---------------------------------------- | ------------------------ |
| `KeystonePasswordRegex`                  | 密码强度正则表达式       |
| `KeystonePasswordExpiresDays`            | 密码有效期天数           |
| `KeystoneLockoutFailureAttempts`         | 锁定前允许的失败认证次数 |
| `KeystoneDisableUserAccountDaysInactive` | 不活跃自动禁用天数       |
| `KeystoneUniqueLastPasswordCount`        | 防止重用旧密码           |
| `KeystoneChangePasswordUponFirstUse`     | 首次登录强制改密         |

#### 生产部署建议 ✅

- Fernet密钥**至少每月轮换一次**（`keystone-manage fernet_rotate`— 重新生成 Fernet 密钥库，旧 Token 在缓存失效前仍可用）
- 部署**多控制器节点 + 负载均衡**实现Keystone高可用
- 禁用`admin_token`（避免无需认证的管理入口）
- 监控日志：`/var/log/keystone/keystone.log`
- 所有服务间通信建议使用**Internal Endpoint**，避免暴露Public API

---

### 一句话总结

Keystone是OpenStack的**统一身份验证 + 服务发现中心**，核心工作流为：**凭据 → Token → Service Catalog → 服务访问**。用户的Token就是云平台的"通行证"，服务间信任通过Keystone的Token校验来保障。🛡️

## 🔗 相关文档

[OpenStack Keystone认证服务概念](Keystone认证服务概念.md) | [OpenStack Nova计算服务详解](OpenStack-Nova计算服务详解.md) | [OpenStack Swift对象存储详解](OpenStack-Swift对象存储详解.md) | [OpenStack Neutron网络服务详解](OpenStack-Neutron网络服务详解.md) | [INI 文件格式概述](../数据格式与标准/ini文件格式概述.md)
