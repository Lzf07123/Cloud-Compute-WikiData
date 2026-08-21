---
title: Ubuntu 系统详情
author: Claude Code
date: 2026-05-30
version: v1.0
tags:
  - Linux
  - Ubuntu
  - 服务器操作系统
  - LTS
  - Canonical
  - 云原生
  - AI
  - Snap
  - APT
category: 技术文档
subcategory: Linux 操作系统
description: Ubuntu 系统的原子化详情，涵盖基本信息、技术架构、版本生命周期、包管理体系（APT + Snap）、安全框架、生态定位与 AI/云原生优势。
difficulty: 入门
audience:
  - 运维工程师
  - 开发者
  - 系统架构师
  - Linux 初学者
  - AI/ML 工程师
status: 已完成
license: CC BY-SA 4.0
source:
  - Ubuntu 官方文档 (ubuntu.com)
  - Ubuntu Release Notes
  - Canonical 官方博客
  - Ubuntu 26.04 LTS 技术预览
render_with:
  - Markdown
  - YAML Frontmatter
related:
  - 四大服务器系统对比总览
  - openEuler系统详情
  - CentOS系统详情
  - Debian系统详情
---

# 🔴 Ubuntu 系统详情

> 参见: [四大服务器系统对比总览 — openEuler / CentOS / Ubuntu / Debian](四大服务器系统对比总览.md) | [Linux 发行版系别与差异详解](Linux发行版系别与差异详解.md) | [openEuler 详情](openEuler系统详情.md) | [CentOS 详情](CentOS系统详情.md) | [Debian 详情](Debian系统详情.md)

---

## 📖 一、基本信息

| 项目         | 内容                                               |
| ------------ | -------------------------------------------------- |
| **全称**     | Ubuntu（源自祖鲁语，意为"人性"/"对他人的善意"）    |
| **发起方**   | Canonical Ltd.（Mark Shuttleworth 创立）           |
| **首次发布** | 2004 年 10 月 20 日（Ubuntu 4.10 "Warty Warthog"） |
| **所属系别** | Debian 系                                          |
| **技术血缘** | 基于 Debian，继承 `.deb` 体系                      |
| **开源协议** | GPL / MIT / Apache 2.0 等多协议                    |
| **定位**     | 桌面、服务器、云、IoT 全场景操作系统               |
| **支持架构** | x86_64、ARM64、RISC-V、IBM Z (s390x)、ppc64el      |
| **官网**     | https://ubuntu.com                                 |

### 市场地位

- **全球最流行的 Linux 发行版**之一（桌面+服务器）
- 云平台 (AWS/Azure/GCP/阿里云/腾讯云) 上**使用率最高的 Linux 镜像**
- AI/ML 领域 NVIDIA CUDA **官方首选** Linux 发行版
- WSL (Windows Subsystem for Linux) 默认发行版

---

## 🔧 二、技术架构

### Ubuntu 26.04 LTS ("Resolute Raccoon") 技术栈

| 组件             | 24.04 LTS (Noble)  | **26.04 LTS (Resolute)**         |
| ---------------- | ------------------ | -------------------------------- |
| **内核**         | Linux 6.8          | **Linux 7.0**                    |
| **桌面环境**     | GNOME 46           | **GNOME 50** (Wayland-Only)      |
| **显示服务器**   | Wayland (X11 可选) | **Wayland 独占** (X11 会话移除)  |
| **Init 系统**    | systemd 255        | **systemd 259** (强制 cgroup v2) |
| **图形驱动**     | Mesa 24.x          | **Mesa 26.0** (Vulkan 1.4)       |
| **Initramfs**    | initramfs-tools    | **Dracut** (默认)                |
| **包管理器**     | APT 2.x            | **APT 3.1** (新依赖求解器)       |
| **时间同步**     | systemd-timesyncd  | **Chrony 4.8** (默认)            |
| **默认文件系统** | ext4               | ext4                             |

### 核心技术变革 (26.04 LTS)

#### 🔒 Rust 重写核心组件

- **sudo-rs:** Rust 重写的 sudo，消除 C 语言缓冲区溢出等内存安全漏洞
- **rust-coreutils (uutils):** `ls`、`cp`、`mv`、`cat` 等基础命令的 Rust 实现
- sudo 密码输入显示 `******` 反馈

#### 🖥️ Wayland-Only 桌面

- GNOME 50 不再提供 X11 会话（XWayland 保留用于兼容旧应用）
- NVIDIA Wayland 性能大幅改善，动态 Boost 默认启用
- KDE/Xfce/MATE 等衍生版仍支持 X11

#### 🔐 安全增强

- **TPM 2.0 全盘加密:** 生产级稳定版，支持安装后添加/移除 PIN
- **后量子密码学:** OpenSSH 和 OpenSSL 默认启用混合后量子算法 (`mlkem768x25519-sha256`)
- **Intel TDX:** 硬件级机密计算
- **Snap 权限提示:** 访问敏感资源时弹出用户授权提示

### 默认应用

| 旧应用         | 新默认应用    | 说明                  |
| -------------- | ------------- | --------------------- |
| GNOME Terminal | **Ptyxis**    | GTK4，支持容器标签    |
| Evince (PDF)   | **Papers**    | GTK4，部分 Rust 重写  |
| Eye of GNOME   | **Loupe**     | 完全 Rust 重写        |
| Totem (视频)   | **Showtime**  | libadwaita 极简播放器 |
| System Monitor | **Resources** | GTK4 + Rust           |

### 开发者工具链

| 工具    | 版本 (26.04 LTS)           |
| ------- | -------------------------- |
| GCC     | 15.2 (GCC 16 可选)         |
| Python  | 3.13.9 / 3.14              |
| OpenJDK | 25 (LTS 8/11/17/21 仍可用) |
| Rust    | 1.93                       |
| Go      | 1.26                       |
| LLVM    | 21                         |
| .NET    | 10                         |
| glibc   | 2.43                       |

---

## 📅 三、版本与生命周期

### 版本策略

| 版本类型             | 发布周期  | 支持时长                  | 发布月份              |
| -------------------- | --------- | ------------------------- | --------------------- |
| **LTS (长期支持版)** | 每 2 年   | 5 年免费 + 5 年 ESM (Pro) | 4 月 (偶数年)         |
| **临时版 (Interim)** | 每 6 个月 | 9 个月                    | 4 月 (奇数年) + 10 月 |

### 当前与未来版本

| 版本      | 代号                 | 发布日期       | 类型    | 标准支持截止 | Pro ESM 截止 |
| --------- | -------------------- | -------------- | ------- | ------------ | ------------ |
| 22.04     | Jammy Jellyfish      | 2022-04        | LTS     | 2027-04      | 2032-04      |
| **24.04** | **Noble Numbat**     | **2024-04**    | **LTS** | **2029-04**  | **2034-04**  |
| 25.10     | Questing Quokka      | 2025-10        | 临时版  | 2026-07      | —            |
| **26.04** | **Resolute Raccoon** | **2026-04-23** | **LTS** | **2031-04**  | **2036-04**  |

### 26.04 LTS 开发里程碑

| 日期           | 里程碑                            |
| -------------- | --------------------------------- |
| 2025-11-27     | Snapshot 1                        |
| 2026-02-19     | 功能冻结                          |
| 2026-03-12     | UI 冻结                           |
| 2026-03-26     | **Beta 发布**                     |
| 2026-04-09     | 内核冻结                          |
| 2026-04-16     | RC (候选发布)                     |
| **2026-04-23** | **正式发布**                      |
| 2026-08-06     | 26.04.1 点版本 (LTS→LTS 升级开启) |

### 生命周期阶段

```
发布 ──── 5年免费标准支持 ────┬──── 5年 ESM (Pro) ────┬──── 5年 Legacy (Pro)
  │                           │                       │
  └─ 安全+Bug修复              └─ CVE补丁 (扩展)       └─ Legacy 附加支持
```

### 升级路径

```bash
# 临时版 → 下一版 (如 25.10→26.04)
do-release-upgrade

# LTS → LTS (需等待 .1 点版本，如 24.04→26.04.1，2026年8月)
do-release-upgrade -d
```

### Ubuntu Pro

- **个人用户:** 免费，最多 5 台机器
- **企业用户:** 付费订阅
- 扩展 10 年安全维护 (标准 5 年 + ESM 5 年)
- 额外功能: FIPS 140-2 认证、Livepatch (免重启内核补丁)、合规工具

### 硬件支持

| 平台          | 说明                                                 |
| ------------- | ---------------------------------------------------- |
| **x86-64-v3** | 可选软件包变体，AVX2/FMA/BMI2 指令集，性能提升 5-15% |
| **最低 CPU**  | Intel Haswell (2013) 或 AMD Zen (2017) 以上          |
| **ARM64**     | 首个官方 ARM64 桌面 ISO，支持高通 Snapdragon         |
| **RISC-V**    | RVA23S64 ISA (不再支持旧 RVA20)                      |
| **IBM Z**     | 最低 z15 架构                                        |
| **树莓派**    | 桌面镜像缩小 777 MB，A/B 启动支持                    |

---

## 📦 四、包管理体系

### 多层包管理架构

```
┌─────────────────────────────────────┐
│         App Center (统一 UI)         │
├────────────────┬────────────────────┤
│    DEB 包      │     Snap 包        │
│   (APT 3.1)    │    (snapd)         │
├────────────────┴────────────────────┤
│       Ubuntu 软件仓库 (Archive)       │
│   Main / Universe / Multiverse      │
│   + Ubuntu Pro (ESM 扩展)           │
└─────────────────────────────────────┘
```

### 仓库分类

| 仓库           | 说明                         | 支持范围            |
| -------------- | ---------------------------- | ------------------- |
| **Main**       | 核心软件，Canonical 官方支持 | 全量安全更新        |
| **Universe**   | 社区维护的自由软件           | 社区尽力支持        |
| **Restricted** | 闭源驱动 (NVIDIA 等)         | Canonical 有限支持  |
| **Multiverse** | 非自由/受限软件              | 无支持保障          |
| **PPA**        | Personal Package Archive     | 第三方个人/团队维护 |
| **ESM (Pro)**  | 扩展安全维护                 | 10 年安全补丁       |

### APT 3.1 新特性 (26.04 LTS)

- 新依赖求解器，自动回退经典求解器
- OpenSSL 替代 GnuTLS 进行安全传输
- 移除 `apt-key` 命令（用 `/etc/apt/trusted.gpg.d/` 替代）
- 依赖解析加速约 30%

### Snap 包管理

| 维度            | 说明                                       |
| --------------- | ------------------------------------------ |
| **定位**        | Canonical 主推的跨发行版打包格式           |
| **特点**        | 自包含依赖、沙箱化运行、自动更新           |
| **与 DEB 差异** | 更大体积 (自含依赖)、更强隔离、跨发行版    |
| **26.04 变化**  | Snap 权限提示默认启用、App Center 统一管理 |

### 常用命令速查

```bash
# === APT (DEB 包管理) ===

# 刷新索引
sudo apt update

# 安装软件
sudo apt install nginx

# 搜索软件
apt search nginx

# 升级所有包
sudo apt upgrade

# 卸载 (保留配置)
sudo apt remove nginx

# 彻底卸载 (含配置)
sudo apt purge nginx

# 清理孤立依赖
sudo apt autoremove

# 查看已安装
apt list --installed

# 查看包信息
apt show nginx

# 编辑仓库源
sudo vi /etc/apt/sources.list

# === Snap 包管理 ===

# 安装 Snap 包
sudo snap install nextcloud

# 搜索 Snap
snap find nextcloud

# 列出已安装
snap list

# 更新所有 Snap
sudo snap refresh

# 卸载
sudo snap remove nextcloud
```

### 开发者快速配置

```bash
# 安装 build-essential (GCC/make 等)
sudo apt install build-essential

# 安装 Python 开发环境
sudo apt install python3 python3-pip python3-venv

# 安装 NVIDIA CUDA (26.04 已内置 APT 仓库)
sudo apt install nvidia-cuda-toolkit

# 安装 Docker (官方 APT 源)
sudo apt install docker.io
```

---

## 🛡️ 五、安全与合规

### 安全框架

| 维度           | 说明                                                  |
| -------------- | ----------------------------------------------------- |
| **MAC 框架**   | **AppArmor**（默认 enforcing 模式）                   |
| **防火墙**     | ufw（Uncomplicated Firewall，iptables/nftables 前端） |
| **安全启动**   | Secure Boot + TPM 2.0 全盘加密                        |
| **后量子密码** | OpenSSH/OpenSSL 默认启用混合后量子算法 (26.04+)       |
| **机密计算**   | Intel TDX（硬件级 VM 隔离）                           |
| **实时内核**   | Livepatch（免重启内核补丁，Pro 订阅）                 |

### AppArmor 关键操作

```bash
# 查看 AppArmor 状态
sudo aa-status

# 查看特定进程的 AppArmor 配置文件
sudo aa-status | grep nginx

# 将配置文件设为 Complain 模式 (仅记录不阻止)
sudo aa-complain /etc/apparmor.d/usr.bin.nginx

# 将配置文件设为 Enforce 模式 (阻止)
sudo aa-enforce /etc/apparmor.d/usr.bin.nginx

# 重新加载所有配置文件
sudo systemctl reload apparmor
```

### 合规认证

| 认证                | 状态                               |
| ------------------- | ---------------------------------- |
| **FIPS 140-2/3**    | ✅ (Ubuntu Pro 付费)               |
| **Common Criteria** | ✅ (Ubuntu 22.04 LTS 获 EAL2 认证) |
| **DISA STIG**       | ✅ (安全技术实施指南可用)          |
| **CIS Benchmarks**  | ✅ (社区维护)                      |
| **GDPR/HIPAA**      | 通过配置支持                       |
| **信创/国产化**     | ❌ 不适用                          |

### 漏洞响应

- Canonical 安全团队 (Ubuntu Security Team)
- 严重漏洞 **72 小时内**发布修复
- [Ubuntu Security Notices (USN)](https://ubuntu.com/security/notices) 公开披露
- `unattended-upgrades` 可配置自动安装安全更新

---

## 🌐 六、生态定位与横向对比

### 与同类系统的关键差异

| 对比维度         | Ubuntu LTS             | openEuler          | CentOS Stream | Debian Stable |
| ---------------- | ---------------------- | ------------------ | ------------- | ------------- |
| **云平台兼容**   | ⭐⭐⭐⭐⭐ 全球最广    | ⭐⭐⭐⭐           | ⭐⭐⭐        | ⭐⭐⭐⭐      |
| **AI/CUDA 支持** | ⭐⭐⭐⭐⭐ NVIDIA 首选 | ⭐⭐⭐ (MindSpore) | ⭐⭐          | ⭐⭐          |
| **桌面体验**     | ⭐⭐⭐⭐⭐             | ⭐⭐               | ⭐            | ⭐⭐⭐        |
| **新手友好度**   | ⭐⭐⭐⭐⭐             | ⭐⭐⭐             | ⭐⭐          | ⭐⭐⭐        |
| **信创合规**     | ❌                     | ✅                 | ❌            | ❌            |
| **长期支持**     | 15 年 (Pro)            | 8 年               | 5 年          | 10 年 (ELTS)  |
| **商业支持**     | ✅ Canonical Pro       | ✅ 华为/麒麟等     | ❌ (需 RHEL)  | ❌            |
| **中文社区**     | ⭐⭐⭐                 | ⭐⭐⭐⭐⭐         | ⭐            | ⭐⭐          |
| **Snap 依赖**    | 强制趋势               | 无                 | 无            | 无            |

### 优势场景

**AI/机器学习:**

- NVIDIA CUDA/cuDNN 官方 APT 仓库 (26.04 直接集成)
- PyTorch、TensorFlow、JAX 官方优先支持
- ROCm (AMD GPU) 同样通过 APT 仓库提供

**云原生:**

- 全球云平台 (AWS/Azure/GCP/阿里云/腾讯云) **默认首选** Linux 镜像
- K8s (MicroK8s/Charmed K8s) 官方支持
- Docker/Podman/containerd 生态最成熟

**桌面与新手:**

- WSL 默认发行版
- 社区教程、问答 (AskUbuntu/StackOverflow) 最丰富
- 中文社区翻译覆盖最广

### Snap 争议

- **优点:** 沙箱隔离、自动更新、跨发行版、开发者友好
- **缺点:** 体积大、启动慢、强制依赖趋势引发社区争议
- **替代选择:** Linux Mint 基于 Ubuntu 但移除 Snap，改用 Flatpak

---

## 📝 总结

Ubuntu LTS 是当前最通用的 Linux 发行版，在云平台兼容性、AI/ML 生态、新手友好度方面无可匹敌。Canonical 正在推动 Rust 核心组件、Wayland-Only 桌面、后量子密码学等技术变革，26.04 LTS 是近十年最重要的架构演进版本。对 Snap 机制敏感的用户可考虑 Linux Mint 或 Debian。
