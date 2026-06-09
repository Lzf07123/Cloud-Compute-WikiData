---
title: CentOS 系统详情
author: Claude Code
date: 2026-05-30
version: v1.0
tags:
  - Linux
  - CentOS
  - CentOS Stream
  - RHEL
  - Red Hat
  - 服务器操作系统
  - RPM
  - 滚动更新
category: 技术文档
subcategory: Linux 操作系统
description: CentOS 系统的原子化详情，涵盖基本信息、CentOS Linux 停服影响、CentOS Stream 技术架构、版本生命周期、包管理体系、安全框架与迁移建议。
difficulty: 入门
audience:
  - 运维工程师
  - 系统架构师
  - CentOS 迁移决策者
  - 开发者
status: 已完成
license: CC BY-SA 4.0
source:
  - Red Hat 官方文档
  - CentOS 官方公告
  - CentOS Stream 发布说明
  - 华为云技术博客
render_with:
  - Markdown
  - YAML Frontmatter
related:
  - 四大服务器系统对比总览
  - openEuler系统详情
  - Ubuntu系统详情
  - Debian系统详情
---

# 🟠 CentOS 系统详情

> 参见: {% post_link 系统与发行版/四大服务器系统对比总览 %} | {% post_link 系统与发行版/Linux发行版系别与差异详解 %} | {% post_link 系统与发行版/openEuler系统详情 openEuler 详情 %} | {% post_link 系统与发行版/Ubuntu系统详情 Ubuntu 详情 %} | {% post_link 系统与发行版/Debian系统详情 Debian 详情 %}

---

## 📖 一、基本信息

| 项目             | 内容                                  |
| ---------------- | ------------------------------------- |
| **全称**         | Community Enterprise Operating System |
| **发起方**       | Red Hat 公司（现属 IBM）              |
| **首次发布**     | 2004 年 5 月                          |
| **所属系别**     | Red Hat 系（RPM 体系）                |
| **包格式**       | `.rpm`                                |
| **包管理器**     | `dnf`（CentOS 8+）/ `yum`（CentOS 7） |
| **治理模式**     | Red Hat 主导                          |
| **当前活跃版本** | CentOS Stream 9 / 10                  |
| **官网**         | https://www.centos.org                |

### ⚠️ 重要历史转折

| 时间           | 事件                                              |
| -------------- | ------------------------------------------------- |
| **2020.12**    | Red Hat 宣布终止 CentOS Linux，转向 CentOS Stream |
| **2021.12.31** | CentOS Linux 8 停止维护（原定 2029 年）           |
| **2024.06.30** | CentOS Linux 7 停止维护（最后传统版本终结）       |
| **2026.05**    | CentOS Stream 10 正式发布                         |

> ⛔ **CentOS Linux 已彻底退出历史舞台，不再推荐任何新生产环境使用。**

---

## 🔄 二、CentOS Linux vs CentOS Stream

### 定位变化

| 维度             | CentOS Linux（已停维）        | CentOS Stream（当前）                   |
| ---------------- | ----------------------------- | --------------------------------------- |
| **与 RHEL 关系** | 下游重建版（RHEL 发布后跟进） | 上游开发平台（RHEL 的前一站）           |
| **更新策略**     | 定点发布 (Point Release)      | **滚动更新** (Rolling Release)          |
| **稳定性**       | 高（跟随已验证的 RHEL）       | 中等（含即将进入 RHEL 的新特性）        |
| **更新节奏**     | RHEL 发布后数周跟进           | 提前 **1-6 周**获得即将进入 RHEL 的更新 |
| **适用场景**     | 企业生产环境                  | 开发测试、前沿技术验证                  |
| **生命周期**     | ~10 年                        | ~5 年                                   |

### 红帽生态中的位置

```
Fedora (激进创新) → CentOS Stream (技术预览) → RHEL (企业级稳定)
    ↓                       ↓                        ↓
  每6个月发布             滚动更新                  定点发布
  最新技术试验             RHEL 的上游              10年支持
  社区驱动                Red Hat 主导              付费订阅
```

---

## 🔧 三、技术架构

### 当前版本技术栈

| 组件             | CentOS Stream 9              | CentOS Stream 10                          |
| ---------------- | ---------------------------- | ----------------------------------------- |
| **内核**         | Linux 5.14                   | Linux **6.12**                            |
| **编译器**       | GCC 11                       | GCC 14 / LLVM 19                          |
| **Rust**         | —                            | Rust 1.82                                 |
| **Go**           | —                            | Go 1.23                                   |
| **Python**       | 3.9                          | 3.12                                      |
| **Java**         | OpenJDK 11 / 17              | OpenJDK 21                                |
| **Ruby**         | —                            | 3.3                                       |
| **PHP**          | —                            | 8.3                                       |
| **Node.js**      | —                            | 22                                        |
| **Web 服务器**   | Apache 2.4 / nginx           | Apache 2.4.62 / nginx 1.26                |
| **数据库**       | PostgreSQL 13 / MariaDB 10.5 | PostgreSQL 16 / MariaDB 10.11 / MySQL 8.4 |
| **缓存**         | Redis                        | Valkey 7.2                                |
| **Init 系统**    | systemd                      | systemd                                   |
| **安全框架**     | SELinux (强制)               | SELinux (强制)                            |
| **默认文件系统** | XFS                          | XFS                                       |
| **容器引擎**     | Podman                       | Podman                                    |

### Stream 10 关键特性

- **cgroups v2** 原生支持，适配 Kubernetes 1.24+
- **Podman** 默认替代 Docker，兼容 OCI 标准
- **Btrfs** 文件系统支持更完善
- 支持 **x86_64** 和 **ARM64** 架构
- **eBPF** 全面支持，可观测性和网络编程能力增强

### 默认文件系统与内核

| 项目             | 说明                         |
| ---------------- | ---------------------------- |
| **默认文件系统** | XFS                          |
| **支持文件系统** | XFS, ext4, Btrfs, NFS, GFS2  |
| **内核策略**     | 保守稳定，跟随 RHEL 内核基线 |
| **架构支持**     | x86_64 (主要), ARM64 (有限)  |

---

## 📅 四、版本与生命周期

### 历史版本时间线

| 版本                 | 发布日期 | 停止维护       | 生命周期         | 状态      |
| -------------------- | -------- | -------------- | ---------------- | --------- |
| CentOS 6             | 2011-07  | 2020-11-30     | ~9 年            | 已停维    |
| CentOS 7             | 2014-07  | **2024-06-30** | ~10 年           | ⛔ 已停维 |
| CentOS 8             | 2019-09  | **2021-12-31** | ~2 年 (提前终止) | ⛔ 已停维 |
| CentOS Stream 8      | 2019-09  | 2024-05-31     | ~5 年            | 已停维    |
| **CentOS Stream 9**  | 2021-12  | **~2027**      | ~5 年            | ✅ 活跃   |
| **CentOS Stream 10** | 2026-05  | **~2030**      | ~5 年            | ✅ 最新   |

### CentOS Stream 生命周期政策

- 每个大版本约 **5 年**维护周期
- **滚动更新**模式，无小版本号概念
- `dnf system-upgrade` 实现跨大版本升级
- 生命周期结束时停止安全更新

### 维护终止影响

```
CentOS 7 (2024.6 EOL)
├── 不再接收安全补丁
├── 不再接收 Bug 修复
├── 软件仓库可能下线
├── 合规审计不可通过
└── 建议立即迁移: Rocky Linux / AlmaLinux / Ubuntu LTS
```

---

## 📦 五、包管理体系

### 包管理概览

| 项目           | CentOS 7            | CentOS Stream 8/9/10 |
| -------------- | ------------------- | -------------------- |
| **包格式**     | `.rpm`              | `.rpm`               |
| **底层工具**   | `rpm`               | `rpm`                |
| **上层管理器** | `yum`               | `dnf`                |
| **仓库配置**   | `/etc/yum.repos.d/` | `/etc/yum.repos.d/`  |
| **软件包数量** | ~10,000             | ~15,000              |

### 仓库结构

| 仓库                 | 说明                                            |
| -------------------- | ----------------------------------------------- |
| **BaseOS**           | 核心操作系统包                                  |
| **AppStream**        | 应用程序流（多版本支持）                        |
| **EPEL**             | Extra Packages for Enterprise Linux（社区扩展） |
| **CRB (PowerTools)** | 开发者工具和库                                  |
| **HighAvailability** | 高可用集群组件                                  |
| **RT**               | 实时内核                                        |

### 模块化版本流 (Module Streams)

```bash
# 查看可用模块流
dnf module list

# 安装特定版本流
sudo dnf module install nodejs:18

# 切换模块流
sudo dnf module reset nodejs
sudo dnf module install nodejs:20
```

### 常用命令速查

```bash
# 刷新仓库索引
sudo dnf makecache

# 安装软件
sudo dnf install nginx

# 搜索软件
dnf search nginx

# 升级所有软件包
sudo dnf upgrade

# 卸载软件
sudo dnf remove nginx

# 清理缓存
sudo dnf clean all

# 查看已安装包
dnf list --installed

# 查看包信息
dnf info nginx

# 查看可用更新
dnf check-update

# 跨大版本升级 (如 9→10)
sudo dnf system-upgrade download --releasever=10
sudo dnf system-upgrade reboot

# 管理服务
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 与 RHEL 的包兼容性

- CentOS Stream 包与 RHEL 包**高度兼容**但非 100% 二进制一致
- 第三方软件商通常提供 "RHEL/CentOS 兼容" 的 RPM 包
- EPEL 提供额外 7,000+ 软件包

---

## 🛡️ 五、安全与合规

### 安全框架

| 维度         | 说明                                                    |
| ------------ | ------------------------------------------------------- |
| **MAC 框架** | **SELinux**（默认 enforcing 模式）                      |
| **防火墙**   | firewalld（nftables 后端）                              |
| **内核加固** | Kernel Lockdown、Secure Boot                            |
| **审计**     | auditd（内核级审计子系统）                              |
| **加密策略** | system-wide crypto policies（`update-crypto-policies`） |

### SELinux 关键操作

```bash
# 查看 SELinux 状态
getenforce

# 临时切换模式
sudo setenforce 0          # Permissive
sudo setenforce 1          # Enforcing

# 永久配置
sudo vi /etc/selinux/config

# 查看 SELinux 拒绝日志
sudo sealert -a /var/log/audit/audit.log

# 恢复文件安全上下文
sudo restorecon -Rv /path/
```

### 合规认证（通过 RHEL 生态）

| 认证            | 状态                        |
| --------------- | --------------------------- |
| FIPS 140-2/3    | ✅ (RHEL 认证，Stream 兼容) |
| Common Criteria | ✅ (RHEL 认证)              |
| DISA STIG       | ✅ (RHEL 认证)              |
| 等保 2.0        | 通过配置支持，无原生认证    |
| 信创/国产化     | ❌ 不适用                   |

### 漏洞响应

- Red Hat 安全团队负责漏洞评估和修复
- 严重漏洞通过 Red Hat 安全公告 (RHSA) 发布
- Stream 用户比 RHEL 用户**更早**获得修复（提前进入上游）
- `dnf upgrade --security` 仅安装安全更新

---

## 🌐 六、生态定位与横向对比

### 与同类系统的关键差异

| 对比维度        | CentOS Stream   | openEuler         | Ubuntu LTS    | Debian Stable  |
| --------------- | --------------- | ----------------- | ------------- | -------------- |
| **更新模式**    | 滚动更新        | LTS + 创新版      | LTS 定点发布  | 稳定版定点发布 |
| **生产适用性**  | ⭐⭐ (开发测试) | ⭐⭐⭐⭐⭐        | ⭐⭐⭐⭐⭐    | ⭐⭐⭐⭐⭐     |
| **RHEL 兼容性** | ⭐⭐⭐⭐ (上游) | ⭐⭐⭐ (RPM 兼容) | ⭐ (不同系别) | ⭐ (不同系别)  |
| **信创合规**    | ❌              | ✅                | ❌            | ❌             |
| **云平台支持**  | ⭐⭐⭐          | ⭐⭐⭐⭐          | ⭐⭐⭐⭐⭐    | ⭐⭐⭐⭐       |
| **中文生态**    | ⭐              | ⭐⭐⭐⭐⭐        | ⭐⭐⭐        | ⭐⭐           |

### 迁移策略

| 当前系统              | 推荐迁移目标                | 理由                        |
| --------------------- | --------------------------- | --------------------------- |
| **CentOS 7**          | Rocky Linux 9 / AlmaLinux 9 | RHEL 1:1 兼容，迁移成本最低 |
| **CentOS 8**          | Rocky Linux 9 / AlmaLinux 9 | 同系别，包管理一致          |
| **CentOS Stream 8**   | CentOS Stream 10            | 同系列升级                  |
| **不想继续 RPM 生态** | Ubuntu 24.04 LTS            | 生态最广，AI/云原生首选     |
| **信创需求**          | openEuler 24.03 LTS         | 合规唯一选择                |

### 适用场景总结

**✅ 适合 CentOS Stream:**

- RHEL 生态的开发和测试环境
- 提前验证应用与下一版 RHEL 的兼容性
- CI/CD 流水线基础镜像
- 云原生/容器化场景（Podman、eBPF、cgroups v2）
- 需要紧跟 RHEL 技术演进的团队

**❌ 不适合 CentOS Stream:**

- 对稳定性要求极高的传统企业生产环境
- 需要 10 年超长生命周期的关键业务
- 信创合规/国产化项目
- 希望"装好就忘"的无人值守场景

---

## 📝 总结

CentOS Linux 已终结，但 CentOS Stream 作为 RHEL 的上游预览通道有其独特价值——适合开发测试和前沿技术验证。对于生产环境，应选择 Rocky Linux、AlmaLinux（RHEL 兼容替代）或 Ubuntu LTS（跨系别替代）。**不要再在任何新项目中使用 CentOS Linux。**
