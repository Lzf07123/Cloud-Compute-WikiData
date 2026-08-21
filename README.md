---
title: 职业技能大赛云计算赛项资料共享知识库
description: 汇集云计算赛项理论知识、Linux 操作指南、容器与云平台技术文档及结构化学习路线，涵盖 Docker、Kubernetes、OpenStack、Linux 系统管理等方向
keywords:
  - 知识库
  - 职业技能大赛
  - 云计算赛项
  - Docker
  - Kubernetes
  - OpenStack
  - Linux
  - 学习计划
author: 知识库管理员
version: 3.0
created: 2025-05-20
updated: 2026-08-22
category: 文档说明
tags:
  - 使用说明
  - 目录索引
  - 学习路线
  - 常见问题
top: 100
---

# 职业技能大赛云计算赛项 · 资料共享知识库

> 汇集云计算赛项理论知识、Linux 操作指南、容器与云平台技术文档及结构化学习路线，共 287 篇 Markdown 文档，按主题分类归档，持续更新中。

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Role](https://img.shields.io/badge/role-%E7%9F%A5%E8%AF%86%E5%BA%93-blue)
![Focus](https://img.shields.io/badge/focus-%E4%BA%91%E8%AE%A1%E7%AE%97%E8%B5%9B%E9%A1%B9-orange)

[![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)](https://www.linux.org/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![OpenStack](https://img.shields.io/badge/OpenStack-ED1944?logo=openstack&logoColor=white)](https://www.openstack.org/)
[![Shell](https://img.shields.io/badge/Shell-4EAA25?logo=gnubash&logoColor=white)](https://www.gnu.org/software/bash/)
[![Markdown](https://img.shields.io/badge/Markdown-000000?logo=markdown&logoColor=white)](https://daringfireball.net/projects/markdown/)

## 目录

- [关于](#关于)
- [内容领域](#内容领域)
- [更新日志](#更新日志)
- [项目](#项目)
- [当前目标](#当前目标)
- [路线图](#路线图)
- [仓库结构](#仓库结构)
- [许可](#许可)

## 关于

本仓库是职业技能大赛云计算赛项的资料共享知识库（文档存储仓库），汇集 Docker、Kubernetes、OpenStack、Linux 系统管理等方向的 Markdown 文档，并提供 Kubernetes 30 天、Linux 1 月速成等结构化学习计划。文档按主题分类归档，共 **287 篇**、21 个分类目录。

完整文章目录、计划索引、使用说明与常见问题见 [INDEX.md](INDEX.md)，作为目录数据的单一事实来源。

| 项目 | 内容 |
| --- | --- |
| 身份 | 文档存储仓库 |
| 方向 | 云计算赛项知识资料 |
| 方式 | Markdown 分类归档 + 结构化学习计划 |
| 目标 | 持续更新，覆盖「理论 → 操作 → 赛题」 |

> 说明：文章无特定顺序，排列位置不代表重要程度或阅读先后；部分文章可能受索引限制无法被搜索框准确检索，建议按分类目录查阅。

## 内容领域

| 分类 | 说明 | 文档数 |
| --- | --- | :---: |
| 🗓️ 学习路线与计划 | K8s 30 天计划、Linux 1 月速成、容器云路径、私有云路径 | 59 |
| 🐳 容器与云原生 | Docker + Kubernetes + 容器基础核心概念 | 33 |
| ☁️ 私有云 | OpenStack 服务架构与详细原理 | 20 |
| 🐧 Linux 系统管理 | 系统管理、网络、权限、进程、磁盘等 10 个子分类 | 161 |
| 📋 基础与通用技能 | 云计算基础、数据格式标准、发行版对比 | 11 |
| 🏆 竞赛与认证 | 赛题、样题解析、知识点汇总 | 3 |
| **合计** |  | **287** |

## 更新日志

| 日期 | 类型 | 说明 |
| --- | :---: | --- |
| 2026-08-22 | 🎨 规范 | 按 Li&About README 规范重构 README，完整文章索引移至 INDEX.md |
| 2026-06-21 | 📑 索引 | 重构 README，新增完整计划索引与全文章目录 |
| 2026-06-10 | 🆕 计划 | 上线 [Kubernetes 30 天学习计划](kubernetes学习计划/k8s-00-学习路线总览.md)（8 模块 / 30 天） |
| 2026-06-09 | 🆕 内容 | 新增 Kubernetes 章节与学习路线 |
| 2025-05-20 | 🚀 创建 | 知识库正式上线，首批导入 Linux 系统管理、Docker、OpenStack 等基础内容 |

## 项目

| 项目 | 简介 | 技术栈 | 状态 |
| --- | --- | --- | --- |
| cloud-compute-wiki-data | 职业技能大赛云计算赛项资料共享知识库（文档存储） | Markdown | 维护中 |

## 当前目标

| 目标 | 说明 | 期限 |
| --- | --- | --- |
| 持续更新资料 | 完善 Kubernetes 学习计划，补充赛题解析与 Linux 常用命令文档 | 持续更新（无固定期限） |

## 路线图

- 近期：完善 Kubernetes 30 天学习计划与赛题模拟内容
- 中期：扩充 Docker / OpenStack 进阶与故障排查文档
- 远期：形成覆盖「理论 → 操作 → 赛题」的完整备赛资料体系

## 仓库结构

```text
cloud-compute-wiki-data/
├── README.md              # 知识库简介与导航
├── INDEX.md               # 完整文章索引与使用说明
├── 学习路线与课程/          # 学习路线与学习计划（28）
├── kubernetes学习计划/      # K8s 30 天计划（31）
├── Docker/                # Docker 文档（14）
├── Kubernetes/            # Kubernetes 文档（16）
├── 容器基础/               # 容器核心概念（3）
├── OpenStack/             # 私有云文档（20）
├── 文件与目录管理/          # 文件与目录命令（17）
├── 权限与用户管理/          # 权限与用户命令（18）
├── 进程管理/               # 进程管理命令（16）
├── 系统管理/               # 系统管理文档（24）
├── 网络管理/               # 网络管理文档（20）
├── 磁盘与存储管理/          # 磁盘与存储命令（12）
├── 文件查看与文本处理/      # 文本处理命令（28）
├── 云计算基础/              # 云计算基础概念（1）
├── 数据格式与标准/          # 数据格式文档（3）
├── 系统与发行版/            # 发行版对比（6）
├── 压缩与归档/              # 压缩归档命令（7）
├── 包管理/                 # 包管理命令（7）
├── Shell内置与杂项/         # Shell 内置与杂项（12）
├── 大赛与认证/              # 竞赛与认证资料（3）
└── 杂项/                   # 其他资料（1）
```

## 许可

© 2026 Lzf07123。保留所有权利。
