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
version: 2.0
created: 2025-05-20
updated: 2026-06-21
category: 文档说明
tags:
  - 使用说明
  - 目录索引
  - 学习路线
  - 常见问题
top: 100
---

# 📚 职业技能大赛云计算赛项 · 资料共享知识库

> 本仓库汇集云计算赛项相关的**理论知识**、**Linux 操作指南**、**容器与云平台技术文档**以及**结构化学习路线**，共 **287 篇 Markdown 文档**，按主题分类归档，涵盖 Docker、Kubernetes、OpenStack、Linux 系统管理等方向，内容持续更新中。

---

## 📝 更新日志

> 记录本知识库的新增内容与重要变更，最新更新在最前。

| 日期       |  类型   | 说明                                                                                                     |
| ---------- | :-----: | -------------------------------------------------------------------------------------------------------- |
| 2026-06-21 | 📑 索引 | 重构 README，新增完整计划索引与 Hexo `post_link` 全文章目录                                              |
| 2026-06-10 | 🆕 计划 | 上线 [Kubernetes 30 天学习计划](kubernetes学习计划/k8s-00-学习路线总览.md)（8 模块 / 30 天） |
| 2026-06-09 | 🆕 内容 | 新增 Kubernetes 章节与学习路线                                                                           |
| 2025-05-20 | 🚀 创建 | 知识库正式上线，首批导入 Linux 系统管理、Docker、OpenStack 等基础内容                                    |

---

## 📂 目录概览

| 分类              | 说明                                                | 文档数  |
| ----------------- | --------------------------------------------------- | :-----: |
| 🗓️ 学习路线与计划 | K8s 30天计划、Linux 1月速成、容器云路径、私有云路径 |   59    |
| 🐳 容器与云原生   | Docker + Kubernetes + 容器基础核心概念              |   33    |
| ☁️ 私有云         | OpenStack 服务架构与详细原理                        |   20    |
| 🐧 Linux 系统管理 | 系统管理、网络、权限、进程、磁盘等 10 个子分类      |   161   |
| 📋 基础与通用技能 | 云计算基础、数据格式标准、发行版对比                |   11    |
| 🏆 竞赛与认证     | 赛题、样题解析、知识点汇总                          |    3    |
| **合计**          |                                                     | **287** |

---

## 🗓️ 计划索引

> 以下为本仓库提供的结构化学习计划，按顺序学习可获得系统性知识体系。

### Kubernetes 30 天学习计划

**总览：**[Kubernetes 容器云学习路线总览](kubernetes学习计划/k8s-00-学习路线总览.md)

从集群搭建到赛题模拟，共 **30 天 / 8 个模块**，每日约 4 小时，适合有 Linux 基础的容器云入门者。

| 模块 | 主题               |   天数    | 核心内容                                                      |
| :--: | ------------------ | :-------: | ------------------------------------------------------------- |
|  M1  | 集群架构与搭建     |  Day 1-3  | 架构原理、环境准备、多节点集群、kubectl 精通                  |
|  M2  | Pod 与核心工作负载 |  Day 4-7  | Pod 生命周期、Deployment、DaemonSet、StatefulSet、Job/CronJob |
|  M3  | 网络与服务发现     | Day 8-11  | Service、Ingress、网络策略、CNI 原理                          |
|  M4  | 存储与配置管理     | Day 12-15 | ConfigMap/Secret、Volume、PV/PVC、StorageClass                |
|  M5  | 调度与资源管理     | Day 16-18 | 调度策略、亲和性/反亲和性、资源限制与 QoS                     |
|  M6  | 安全与认证         | Day 19-21 | RBAC、ServiceAccount、镜像安全、网络策略                      |
|  M7  | 监控日志与排错     | Day 22-24 | Prometheus 监控、EFK 日志、故障排查实战                       |
|  M8  | 综合实战与赛题模拟 | Day 25-30 | 真题模拟、弱点回顾、全真模拟考试                              |

#### M1 · 集群架构与搭建

[Day 1 - 集群架构原理与环境准备](kubernetes学习计划/M1-集群架构与搭建/k8s-day01-集群架构原理与环境准备.md)
[Day 2 - 多节点集群与节点管理](kubernetes学习计划/M1-集群架构与搭建/k8s-day02-多节点集群与节点管理.md)
[Day 3 - 集群运维与 kubectl 精通](kubernetes学习计划/M1-集群架构与搭建/k8s-day03-集群运维与kubectl精通.md)

#### M2 · Pod 与核心工作负载

[Day 4 - Pod 生命周期与多容器模式](kubernetes学习计划/M2-Pod与核心工作负载/k8s-day04-Pod生命周期与多容器模式.md)
[Day 5 - Deployment 与 ReplicaSet](kubernetes学习计划/M2-Pod与核心工作负载/k8s-day05-Deployment与ReplicaSet.md)
[Day 6 - DaemonSet、StatefulSet、Job、CronJob](kubernetes学习计划/M2-Pod与核心工作负载/k8s-day06-DaemonSet-StatefulSet-Job-CronJob.md)
[Day 7 - Pod 资源综合实战](kubernetes学习计划/M2-Pod与核心工作负载/k8s-day07-Pod资源综合实战.md)

#### M3 · 网络与服务发现

[Day 8 - Service 与集群内服务发现](kubernetes学习计划/M3-网络与服务发现/k8s-day08-Service与集群内服务发现.md)
[Day 9 - Ingress 与外部流量接入](kubernetes学习计划/M3-网络与服务发现/k8s-day09-Ingress与外部流量接入.md)
[Day 10 - 网络策略与 CNI 原理](kubernetes学习计划/M3-网络与服务发现/k8s-day10-网络策略与CNI原理.md)
[Day 11 - 网络综合实战](kubernetes学习计划/M3-网络与服务发现/k8s-day11-网络综合实战.md)

#### M4 · 存储与配置管理

[Day 12 - ConfigMap 与 Secret](kubernetes学习计划/M4-存储与配置管理/k8s-day12-ConfigMap与Secret.md)
[Day 13 - Volume 与 PV/PVC](kubernetes学习计划/M4-存储与配置管理/k8s-day13-Volume与PV-PVC.md)
[Day 14 - StorageClass 与动态供给](kubernetes学习计划/M4-存储与配置管理/k8s-day14-StorageClass与动态供给.md)
[Day 15 - 存储综合实战](kubernetes学习计划/M4-存储与配置管理/k8s-day15-存储综合实战.md)

#### M5 · 调度与资源管理

[Day 16 - 调度策略与亲和性](kubernetes学习计划/M5-调度与资源管理/k8s-day16-调度策略与亲和性.md)
[Day 17 - 资源限制与 QoS](kubernetes学习计划/M5-调度与资源管理/k8s-day17-资源限制与QoS.md)
[Day 18 - 调度综合实战](kubernetes学习计划/M5-调度与资源管理/k8s-day18-调度综合实战.md)

#### M6 · 安全与认证

[Day 19 - RBAC 权限控制](kubernetes学习计划/M6-安全与认证/k8s-day19-RBAC权限控制.md)
[Day 20 - ServiceAccount 与镜像安全](kubernetes学习计划/M6-安全与认证/k8s-day20-ServiceAccount与镜像安全.md)
[Day 21 - 安全综合实战](kubernetes学习计划/M6-安全与认证/k8s-day21-安全综合实战.md)

#### M7 · 监控日志与排错

[Day 22 - Prometheus 监控体系](kubernetes学习计划/M7-监控日志与排错/k8s-day22-Prometheus监控体系.md)
[Day 23 - 日志收集与 EFK](kubernetes学习计划/M7-监控日志与排错/k8s-day23-日志收集与EFK.md)
[Day 24 - 故障排查实战](kubernetes学习计划/M7-监控日志与排错/k8s-day24-故障排查实战.md)

#### M8 · 综合实战与赛题模拟

[Day 25 - 赛题模拟 1-3](kubernetes学习计划/M8-综合实战与赛题模拟/k8s-day25-赛题模拟1-3.md)
[Day 26 - 赛题模拟 4-5](kubernetes学习计划/M8-综合实战与赛题模拟/k8s-day26-赛题模拟4-5.md)
[Day 27 - 赛题模拟 6-7](kubernetes学习计划/M8-综合实战与赛题模拟/k8s-day27-赛题模拟6-7.md)
[Day 28 - 赛题模拟 8 + 限时挑战](kubernetes学习计划/M8-综合实战与赛题模拟/k8s-day28-赛题模拟8+限时挑战.md)
[Day 29 - 弱点回顾与强化](kubernetes学习计划/M8-综合实战与赛题模拟/k8s-day29-弱点回顾与强化.md)
[Day 30 - 全真模拟考试](kubernetes学习计划/M8-综合实战与赛题模拟/k8s-day30-全真模拟考试.md)

### Linux 1 月速成计划

**总览：**[Linux 1 月速成计划学习路线图](学习路线与课程/linux-1月速成计划学习路线图.md)

4 周掌握 Linux 日常操作与运维技能，**纯终端实操、排错驱动**，适合零基础入门。

|  周次  | 主题               | 每日内容                                                             |
| :----: | ------------------ | -------------------------------------------------------------------- |
| Week 1 | 文件与目录基础     | 路径切换 → 文件操作 → 内容查看 → 文本统计 → 串联实操 → 综合实战      |
| Week 2 | 用户权限与进程管理 | 用户身份 → 权限深入 → 进程管理 → 管道重定向 → 串联实操 → 综合实战    |
| Week 3 | 文本处理三剑客     | grep 基础 → 正则进阶 → sed 流编辑 → awk 列处理 → 打包压缩 → 综合实战 |
| Week 4 | 网络与系统运维     | 网络诊断 → SSH 远程 → 资源监控 → 定时任务 → 串联实操 → 综合实战      |

#### Week 1 · 文件与目录基础

[Day 1 - 路径与目录切换](学习路线与课程/week1-day1-路径与目录切换.md)
[Day 2 - 文件与目录操作](学习路线与课程/week1-day2-文件与目录操作.md)
[Day 3 - 文件内容查看](学习路线与课程/week1-day3-文件内容查看.md)
[Day 4 - 文本统计与处理](学习路线与课程/week1-day4-文本统计与处理.md)
[Day 5 - 本周串联实操](学习路线与课程/week1-day5-本周串联实操.md)
[Weekend - 综合实战](学习路线与课程/week1-weekend-综合实战.md)

#### Week 2 · 用户权限与进程管理

[Day 1 - 用户身份与基本权限](学习路线与课程/week2-day1-用户身份与基本权限.md)
[Day 2 - 权限深入与提权](学习路线与课程/week2-day2-权限深入与提权.md)
[Day 3 - 进程查看与管理](学习路线与课程/week2-day3-进程查看与管理.md)
[Day 4 - 前后台与重定向管道](学习路线与课程/week2-day4-前后台与重定向管道.md)
[Day 5 - 本周串联实操](学习路线与课程/week2-day5-本周串联实操.md)
[Weekend - 综合实战](学习路线与课程/week2-weekend-综合实战.md)

#### Week 3 · 文本处理三剑客

[Day 1 - grep 基础](学习路线与课程/week3-day1-grep基础.md)
[Day 2 - grep 正则](学习路线与课程/week3-day2-grep正则.md)
[Day 3 - sed 流编辑](学习路线与课程/week3-day3-sed流编辑.md)
[Day 4 - awk 列处理](学习路线与课程/week3-day4-awk列处理.md)
[Day 5 - 打包与压缩](学习路线与课程/week3-day5-打包与压缩.md)
[Weekend - 综合实战](学习路线与课程/week3-weekend-综合实战.md)

#### Week 4 · 网络与系统运维

[Day 1 - 网络诊断](学习路线与课程/week4-day1-网络诊断.md)
[Day 2 - SSH 远程连接](学习路线与课程/week4-day2-SSH远程连接.md)
[Day 3 - 系统资源监控](学习路线与课程/week4-day3-系统资源监控.md)
[Day 4 - 定时任务](学习路线与课程/week4-day4-定时任务.md)
[Day 5 - 本周串联实操](学习路线与课程/week4-day5-本周串联实操.md)
[Weekend - 综合实战](学习路线与课程/week4-weekend-综合实战.md)

### 容器云学习路径

**总览：**[容器云学习路径](学习路线与课程/容器云学习路径.md)

从容器基础（Namespace/Cgroups/OCI）→ Docker（镜像、网络、存储、Compose）→ Kubernetes 的完整知识体系。

### 私有云学习路径

**总览：**[私有云 OpenStack 学习路径](学习路线与课程/私有云学习路径.md)

从 OpenStack 概念与架构 → 核心组件（Keystone/Nova/Neutron/Glance）→ 存储（Cinder/Swift）→ 管理（Horizon/Heat/Ceilometer）→ 部署运维的完整路径。

### 学习技巧

[学习技巧](学习路线与课程/学习技巧.md)

---

## 📚 知识体系索引

> 按知识域分组的完整文章索引，点击链接即可访问对应文章。

### 🐳 容器与云原生

#### 容器基础

[OCI 标准概述](容器基础/OCI标准概述.md)
[容器核心概念](容器基础/容器核心概念.md)
[容器镜像仓库](容器基础/容器镜像仓库.md)

#### Docker

[Docker 架构解析](Docker/Docker架构解析.md)
[Docker 镜像管理](Docker/Docker镜像管理.md)
[Docker 镜像管理详解](Docker/Docker镜像管理详解.md)
[Docker 容器生命周期](Docker/Docker容器生命周期.md)
[Docker 容器生命周期详解](Docker/Docker容器生命周期详解.md)
[Docker 网络模型](Docker/Docker网络模型.md)
[Docker 网络模型详解](Docker/Docker网络模型详解.md)
[Docker 数据持久化](Docker/Docker数据持久化.md)
[Docker 数据持久化详解](Docker/Docker数据持久化详解.md)
[Docker 镜像仓库详解](Docker/Docker镜像仓库详解.md)
[Docker 多容器编排详解](Docker/Docker多容器编排详解.md)
[Docker Compose 多容器编排入门](Docker/Docker-Compose多容器编排入门.md)
[Docker 实际应用 - 安装与部署 Nginx](Docker/Docker实际应用-安装与部署Nginx.md)
[Docker 实际应用 - 构建自定义镜像](Docker/Docker实际应用-构建自定义镜像.md)

#### Kubernetes

[Kubernetes 核心概念全景](Kubernetes/Kubernetes核心概念全景.md)
[Kubernetes 核心概念详解](Kubernetes/Kubernetes核心概念详解.md)
[Kubernetes 网络模型与实现](Kubernetes/Kubernetes网络模型与实现.md)
[Kubernetes 网络模型与实现详解](Kubernetes/Kubernetes网络模型与实现详解.md)
[Kubernetes 存储抽象](Kubernetes/Kubernetes存储抽象.md)
[Kubernetes 存储抽象详解](Kubernetes/Kubernetes存储抽象详解.md)
[Kubernetes 调度与部署策略](Kubernetes/Kubernetes调度与部署策略.md)
[Kubernetes 调度与部署详解](Kubernetes/Kubernetes调度与部署详解.md)
[Kubernetes 资源管理](Kubernetes/Kubernetes资源管理.md)
[Kubernetes 资源管理详解](Kubernetes/Kubernetes资源管理详解.md)
[Kubernetes 安全基础](Kubernetes/Kubernetes安全基础.md)
[Kubernetes 安全基础详解](Kubernetes/Kubernetes安全基础详解.md)
[Kubernetes 可观测性](Kubernetes/Kubernetes可观测性.md)
[Kubernetes 可观测性详解](Kubernetes/Kubernetes可观测性详解.md)
[Kubernetes 配置与密文](Kubernetes/Kubernetes配置与密文.md)
[Kubernetes 配置与密文详解](Kubernetes/Kubernetes配置与密文详解.md)

### ☁️ 私有云

#### OpenStack

[OpenStack 概述](OpenStack/OpenStack概述.md)
[OpenStack 架构与分工](OpenStack/OpenStack架构与分工.md)

**核心服务**

[Keystone 认证服务 - 概念](OpenStack/Keystone认证服务概念.md)
[Keystone 认证服务 - 详解](OpenStack/OpenStack-Keystone详解.md)
[Nova 计算服务 - 概念](OpenStack/Nova计算服务概念.md)
[Nova 计算服务 - 详解](OpenStack/OpenStack-Nova计算服务详解.md)
[Neutron 网络服务 - 概念](OpenStack/Neutron网络服务概念.md)
[Neutron 网络服务 - 详解](OpenStack/OpenStack-Neutron网络服务详解.md)
[Glance 镜像服务 - 概念](OpenStack/Glance镜像服务概念.md)
[Glance 镜像服务 - 详解](OpenStack/OpenStack-Glance镜像服务详解.md)

**存储服务**

[Cinder 块存储 - 概念](OpenStack/Cinder块存储服务概念.md)
[Cinder 块存储 - 详解](OpenStack/OpenStack-Cinder块存储详解.md)
[Swift 对象存储 - 概念](OpenStack/Swift对象存储服务概念.md)
[Swift 对象存储 - 详解](OpenStack/OpenStack-Swift对象存储详解.md)

**管理与监控**

[Horizon Web 界面 - 概念](OpenStack/HorizonWeb界面概念.md)
[Horizon Web 界面 - 详解](OpenStack/OpenStack-Horizon详解.md)
[Heat 编排服务 - 概念](OpenStack/Heat编排服务概念.md)
[Heat 编排服务 - 详解](<OpenStack/OpenStack-Heat(编排)详解.md>)
[Ceilometer 监控计量 - 概念](OpenStack/Ceilometer监控计量服务概念.md)
[Ceilometer 监控计量 - 详解](OpenStack/OpenStack-Ceilometer监控计量详解.md)

### 🐧 Linux 系统管理

#### 文件与目录管理

[ls - 列出目录内容](文件与目录管理/文件与目录管理-ls.md)
[cd - 切换目录](文件与目录管理/文件与目录管理-cd.md)
[pwd - 显示当前路径](文件与目录管理/文件与目录管理-pwd.md)
[mkdir - 创建目录](文件与目录管理/文件与目录管理-mkdir.md)
[cp - 复制文件与目录](文件与目录管理/文件与目录管理-cp.md)
[mv - 移动/重命名文件](文件与目录管理/文件与目录管理-mv.md)
[rm - 删除文件](文件与目录管理/文件与目录管理-rm.md)
[rmdir - 删除空目录](文件与目录管理/文件与目录管理-rmdir.md)
[touch - 创建文件/更新时间戳](文件与目录管理/文件与目录管理-touch.md)
[ln - 创建链接](文件与目录管理/文件与目录管理-ln.md)
[find - 查找文件](文件与目录管理/文件与目录管理-find.md)
[locate - 快速定位文件](文件与目录管理/文件与目录管理-locate.md)
[tree - 树形显示目录](文件与目录管理/文件与目录管理-tree.md)
[stat - 查看文件状态](文件与目录管理/文件与目录管理-stat.md)
[file - 识别文件类型](文件与目录管理/文件与目录管理-file.md)
[df - 磁盘空间使用](文件与目录管理/文件与目录管理-df.md)
[du - 目录空间占用](文件与目录管理/文件与目录管理-du.md)

#### 文件查看与文本处理

[cat - 连接并显示文件](文件查看与文本处理/文件查看与文本处理-cat.md)
[less - 分页浏览文件](文件查看与文本处理/文件查看与文本处理-less.md)
[more - 分页显示文件](文件查看与文本处理/文件查看与文本处理-more.md)
[head - 查看文件头部](文件查看与文本处理/文件查看与文本处理-head.md)
[tail - 查看文件尾部](文件查看与文本处理/文件查看与文本处理-tail.md)
[tac - 反向输出文件](文件查看与文本处理/文件查看与文本处理-tac.md)
[nl - 带行号显示文件](文件查看与文本处理/文件查看与文本处理-nl.md)
[cut - 按列剪切文本](文件查看与文本处理/文件查看与文本处理-cut.md)
[sort - 排序文本](文件查看与文本处理/文件查看与文本处理-sort.md)
[uniq - 去重与统计](文件查看与文本处理/文件查看与文本处理-uniq.md)
[wc - 统计行数/词数/字符数](文件查看与文本处理/文件查看与文本处理-wc.md)
[grep - 文本搜索过滤](文件查看与文本处理/文件查看与文本处理-grep.md)
[sed - 流编辑器](文件查看与文本处理/文件查看与文本处理-sed.md)
[awk - 文本处理语言](文件查看与文本处理/文件查看与文本处理-awk.md)
[diff - 文件差异对比](文件查看与文本处理/文件查看与文本处理-diff.md)
[comm - 比较已排序文件](文件查看与文本处理/文件查看与文本处理-comm.md)
[join - 关联合并文件](文件查看与文本处理/文件查看与文本处理-join.md)
[paste - 按列合并文件](文件查看与文本处理/文件查看与文本处理-paste.md)
[tr - 字符替换与删除](文件查看与文本处理/文件查看与文本处理-tr.md)
[tee - 双向输出](文件查看与文本处理/文件查看与文本处理-tee.md)
[xargs - 参数传递](文件查看与文本处理/文件查看与文本处理-xargs.md)
[split - 分割文件](文件查看与文本处理/文件查看与文本处理-split.md)
[od - 八进制/十六进制查看](文件查看与文本处理/文件查看与文本处理-od.md)
[rev - 反转文本行](文件查看与文本处理/文件查看与文本处理-rev.md)
[fold - 文本换行折叠](文件查看与文本处理/文件查看与文本处理-fold.md)
[column - 列格式化](文件查看与文本处理/文件查看与文本处理-column.md)
[echo - 输出文本](文件查看与文本处理/文件查看与文本处理-echo.md)
[printf - 格式化输出](文件查看与文本处理/文件查看与文本处理-printf.md)

#### 系统管理

[systemctl - 系统服务管理](系统管理/系统管理-systemctl.md)
[journalctl - 日志查看](系统管理/系统管理-journalctl.md)
[crontab - 定时任务](系统管理/系统管理-crontab.md)
[at - 一次性定时任务](系统管理/系统管理-at.md)
[date - 日期时间](系统管理/系统管理-date.md)
[timedatectl - 时区与时间管理](系统管理/系统管理-timedatectl.md)
[hostname - 主机名管理](系统管理/系统管理-hostname.md)
[uname - 系统信息](系统管理/系统管理-uname.md)
[lsblk - 列出块设备](系统管理/系统管理-lsblk.md)
[lscpu - CPU 信息](系统管理/系统管理-lscpu.md)
[lspci - PCI 设备信息](系统管理/系统管理-lspci.md)
[lsusb - USB 设备信息](系统管理/系统管理-lsusb.md)
[dmidecode - 硬件信息](系统管理/系统管理-dmidecode.md)
[free - 内存使用情况](系统管理/系统管理-free.md)
[dmesg - 内核日志](系统管理/系统管理-dmesg.md)
[history - 命令历史](系统管理/系统管理-history.md)
[alias / unalias - 命令别名](系统管理/系统管理-alias-unalias.md)
[export - 环境变量导出](系统管理/系统管理-export.md)
[env - 环境变量管理](系统管理/系统管理-env.md)
[locale - 语言环境](系统管理/系统管理-locale.md)
[man - 手册查阅](系统管理/系统管理-man.md)
[whatis - 命令简介](系统管理/系统管理-whatis.md)
[cal - 日历](系统管理/系统管理-cal.md)
[shutdown - 关机/重启](系统管理/系统管理-shutdown.md)

#### 网络管理

[curl - 网络请求工具](网络管理/网络管理-curl.md)
[wget - 文件下载工具](网络管理/网络管理-wget.md)
[ip - IP 网络配置](网络管理/网络管理-ip.md)
[ifconfig - 网络接口配置](网络管理/网络管理-ifconfig.md)
[netstat - 网络连接统计](网络管理/网络管理-netstat.md)
[ss - Socket 统计](网络管理/网络管理-ss.md)
[ping - 连通性测试](网络管理/网络管理-ping.md)
[traceroute / mtr - 路由追踪](网络管理/网络管理-traceroute-mtr.md)
[dig / nslookup / host - DNS 查询](网络管理/网络管理-dig-nslookup-host.md)
[nmap - 端口扫描](网络管理/网络管理-nmap.md)
[nc - 网络调试工具](网络管理/网络管理-nc.md)
[tcpdump - 抓包分析](网络管理/网络管理-tcpdump.md)
[ssh - 远程登录](网络管理/网络管理-ssh.md)
[scp - 远程文件复制](网络管理/网络管理-scp.md)
[rsync - 远程同步](网络管理/网络管理-rsync.md)
[iptables - 防火墙](网络管理/网络管理-iptables.md)
[ufw - 简易防火墙](网络管理/网络管理-ufw.md)
[lsof - 列出打开的文件](网络管理/网络管理-lsof.md)
[ethtool - 网卡配置](网络管理/网络管理-ethtool.md)
[route / arp - 路由与 ARP](网络管理/网络管理-route-arp.md)

#### 权限与用户管理

[chmod - 修改文件权限](权限与用户管理/权限与用户管理-chmod.md)
[chown - 修改文件所有者](权限与用户管理/权限与用户管理-chown.md)
[chgrp - 修改文件所属组](权限与用户管理/权限与用户管理-chgrp.md)
[umask - 默认权限掩码](权限与用户管理/权限与用户管理-umask.md)
[useradd - 创建用户](权限与用户管理/权限与用户管理-useradd.md)
[userdel - 删除用户](权限与用户管理/权限与用户管理-userdel.md)
[usermod - 修改用户](权限与用户管理/权限与用户管理-usermod.md)
[groupadd - 创建用户组](权限与用户管理/权限与用户管理-groupadd.md)
[groupdel - 删除用户组](权限与用户管理/权限与用户管理-groupdel.md)
[passwd - 密码管理](权限与用户管理/权限与用户管理-passwd.md)
[su - 切换用户](权限与用户管理/权限与用户管理-su.md)
[sudo - 提权执行](权限与用户管理/权限与用户管理-sudo.md)
[id - 查看用户身份](权限与用户管理/权限与用户管理-id.md)
[who - 当前登录用户](权限与用户管理/权限与用户管理-who.md)
[whoami - 当前用户名](权限与用户管理/权限与用户管理-whoami.md)
[w - 用户活动信息](权限与用户管理/权限与用户管理-w.md)
[last - 登录历史](权限与用户管理/权限与用户管理-last.md)
[users - 当前登录用户列表](权限与用户管理/权限与用户管理-users.md)

#### 进程管理

[ps - 查看进程](进程管理/进程管理-ps.md)
[top - 实时进程监控](进程管理/进程管理-top.md)
[htop - 交互式进程监控](进程管理/进程管理-htop.md)
[kill - 终止进程](进程管理/进程管理-kill.md)
[killall - 按名称终止进程](进程管理/进程管理-killall.md)
[pkill - 按名称模式杀进程](进程管理/进程管理-pkill.md)
[pgrep - 按名称查找进程](进程管理/进程管理-pgrep.md)
[pidof - 获取进程 PID](进程管理/进程管理-pidof.md)
[nice - 调整进程优先级](进程管理/进程管理-nice.md)
[renice - 修改运行中进程优先级](进程管理/进程管理-renice.md)
[nohup - 后台免挂起运行](进程管理/进程管理-nohup.md)
[bg / fg / jobs - 前后台任务管理](进程管理/进程管理-bg-fg-jobs.md)
[pstree - 进程树](进程管理/进程管理-pstree.md)
[uptime - 系统运行时间](进程管理/进程管理-uptime.md)
[time - 命令执行计时](进程管理/进程管理-time.md)
[watch - 周期性执行命令](进程管理/进程管理-watch.md)

#### 磁盘与存储管理

[fdisk - 磁盘分区](磁盘与存储管理/磁盘与存储管理-fdisk.md)
[parted - 高级分区工具](磁盘与存储管理/磁盘与存储管理-parted.md)
[mount - 挂载文件系统](磁盘与存储管理/磁盘与存储管理-mount.md)
[findmnt - 查找挂载点](磁盘与存储管理/磁盘与存储管理-findmnt.md)
[mkfs - 创建文件系统](磁盘与存储管理/磁盘与存储管理-mkfs.md)
[fsck - 文件系统检查](磁盘与存储管理/磁盘与存储管理-fsck.md)
[blkid - 块设备属性](磁盘与存储管理/磁盘与存储管理-blkid.md)
[dd - 磁盘数据读写](磁盘与存储管理/磁盘与存储管理-dd.md)
[eject - 弹出介质](磁盘与存储管理/磁盘与存储管理-eject.md)
[smartctl - 磁盘健康检测](磁盘与存储管理/磁盘与存储管理-smartctl.md)
[swapon / swapoff - 交换空间管理](磁盘与存储管理/磁盘与存储管理-swapon-swapoff.md)
[sync - 强制写入磁盘](磁盘与存储管理/磁盘与存储管理-sync.md)

#### Shell 内置与杂项

[echo - 输出文本](Shell内置与杂项/Shell内置与杂项-echo.md)
[printf - 格式化输出](Shell内置与杂项/Shell内置与杂项-printf.md)
[read - 读取输入](Shell内置与杂项/Shell内置与杂项-read.md)
[test - 条件测试](Shell内置与杂项/Shell内置与杂项-test.md)
[eval / trap / shift - Shell 特殊内置命令](Shell内置与杂项/Shell内置与杂项-eval-trap-shift.md)
[set / unset - Shell 选项与变量](Shell内置与杂项/Shell内置与杂项-set-unset.md)
[source - 执行脚本于当前 Shell](Shell内置与杂项/Shell内置与杂项-source.md)
[command / exec - 命令查找与替换](Shell内置与杂项/Shell内置与杂项-command-exec.md)
[type - 显示命令类型](Shell内置与杂项/Shell内置与杂项-type.md)
[which / whereis - 定位命令路径](Shell内置与杂项/Shell内置与杂项-which-whereis.md)
[exit / clear - 退出与清屏](Shell内置与杂项/Shell内置与杂项-exit-clear.md)
[true / false / sleep - 返回码与休眠](Shell内置与杂项/Shell内置与杂项-true-false-sleep.md)

#### 包管理

[apt - Debian 系包管理](包管理/包管理-apt.md)
[apt-get - Debian 系传统包管理](包管理/包管理-apt-get.md)
[dpkg - Debian 系底层包管理](包管理/包管理-dpkg.md)
[dnf / yum - Red Hat 系包管理](包管理/包管理-dnf-yum.md)
[rpm - Red Hat 系底层包管理](包管理/包管理-rpm.md)
[pacman - Arch 系包管理](包管理/包管理-pacman.md)
[snap / flatpak - 通用包格式](包管理/包管理-snap-flatpak.md)

#### 压缩与归档

[tar - 归档工具](压缩与归档/压缩与归档-tar.md)
[gzip - GNU 压缩](压缩与归档/压缩与归档-gzip.md)
[bzip2 - 高压缩率工具](压缩与归档/压缩与归档-bzip2.md)
[xz - 高压缩率工具](压缩与归档/压缩与归档-xz.md)
[zip / unzip - 跨平台压缩解压](压缩与归档/压缩与归档-zip-unzip.md)
[compress - 传统 Unix 压缩](压缩与归档/压缩与归档-compress.md)
[zgrep / zcat / zless - 压缩文件内容操作](压缩与归档/压缩与归档-zgrep-zcat-zless.md)

### 📋 基础与通用技能

#### 云计算基础

[云计算基础概念](云计算基础/云计算基础概念.md)

#### 数据格式与标准

[JSON 文档格式概述](数据格式与标准/JSON文档格式概述.md)
[YAML 文档格式概述](数据格式与标准/YAML文档格式概述.md)
[INI 文件格式概述](数据格式与标准/ini文件格式概述.md)

#### 系统与发行版

[Linux 发行版系别与差异详解](系统与发行版/Linux发行版系别与差异详解.md)
[四大服务器系统对比总览](系统与发行版/四大服务器系统对比总览.md)
[Debian 系统详情](系统与发行版/Debian系统详情.md)
[CentOS 系统详情](系统与发行版/CentOS系统详情.md)
[Ubuntu 系统详情](系统与发行版/Ubuntu系统详情.md)
[openEuler 系统详情](系统与发行版/openEuler系统详情.md)

#### 杂项

[常用 Linux 命令速查](杂项/linux-commands.md)

### 🏆 竞赛与认证

[职业技能大赛云计算赛项知识点](大赛与认证/职业技能大赛云计算赛项知识点.md)
[2025-2026 广东省职业技能大赛云计算赛项赛题](大赛与认证/2025-2026广东省职业技能大赛云计算赛项赛题.md)
[2025 DeepSeek 大赛样题解析](大赛与认证/2025-DeepSeek大赛样题解析.md)

---

## 📖 使用说明

### ⚠️ 重要声明

- **文章无特定顺序**：本知识库中的文章未按逻辑、优先级或时间线排序，排列位置不代表重要程度或阅读先后顺序，请根据需求通过标题或分类目录查阅。
- **部分文章可能无法被搜索索引**：受限于系统机制或标签缺失，部分文章可能无法被搜索框准确检索。若搜索无果，建议浏览相关分类目录或联系管理员。

### ❓ 常见问题（FAQ）

<details>
<summary>点击展开 FAQ</summary>

**Q1：为什么搜索关键词却找不到明明存在的文章？**

A：这正是上述声明中提到的"索引限制"。部分历史导入、特殊格式或未添加标签的文章可能被搜索引擎忽略。建议：

- 更换同义词或缩短关键词再次搜索；
- 直接前往对应分类目录下手动查找；
- 联系管理员补充该文章的搜索标签。

**Q2：文章内容出现错别字、过时或不准确怎么办？**

A：知识库需要大家共同维护。若发现内容有误，欢迎直接编辑修改；若暂无编辑权限，请联系管理员进行更正。

**Q3：如何快速找到需要的内容？**

A：推荐以下方式：

- **精准搜索**：使用搜索框输入核心专业词汇；
- **分类浏览**：参考本页面的"目录概览"和"知识体系索引"按域查找；
- **计划索引**：跟随"计划索引"中的结构化学习路径循序渐进；
- **标签筛选**：通过文章底部的标签快速聚合同类文章。

**Q4：为什么有些页面里的图片无法显示或链接失效？**

A：部分文章从外部导入或迁移而来，附件、图片或外部链接可能已失效。如遇此类情况，请及时联系维护人员修复。

</details>

### 🤝 维护与反馈

本知识库的内容依赖于大家的共同完善。如果您在使用过程中遇到任何问题，包括但不限于**文章无法索引、页面加载异常、权限申请、内容纠错**等，请随时联系知识库管理员：`lzf@lizf.cn`

感谢您的理解与配合，祝使用愉快！🎉
