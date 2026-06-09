---
title: 网络管理-ethtool
description: ethtool
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-ethtool` 🌐 — 查询和设置网卡参数

## 作用

ethtool 用于查询和修改网络接口卡（NIC）的驱动参数和硬件设置，包括速率、双工模式、自动协商、网卡队列、Ring Buffer、硬件卸载功能等。是网卡性能调优和故障排查的核心工具。

## 语法

```
ethtool [选项] 接口名
```

## 用法

ethtool `接口名` 查看网卡基本信息（速率、双工、Link 状态）；`-S` 查看网卡统计信息（丢包、错包等）；`-i` 查看驱动信息；`-k` 查看硬件卸载功能（如 GRO/GSO/TSO），`-K` 修改硬件卸载功能；`-g` 查看 Ring Buffer 大小，`-G` 修改 Ring Buffer 大小；`-x` 查看 RSS 哈希配置，`-L` 设置 RSS 队列数；`-a` 查看流控（pause frame），`-A` 修改流控；`-P` 查看网卡永久 MAC 地址。`ethtool eth0` 是最常用的快速检查。注意：查看类选项用小写，修改类选项用大写。

## 常用参数

| 参数 | 说明                  |
| ---- | --------------------- |
| `-S` | 统计信息              |
| `-i` | 驱动信息              |
| `-k` | 查看硬件卸载功能      |
| `-K` | 修改硬件卸载功能      |
| `-g` | 查看 Ring Buffer 大小 |
| `-G` | 修改 Ring Buffer 大小 |
| `-x` | 查看 RSS 哈希配置     |
| `-L` | 设置 RSS 队列数       |
| `-a` | 查看流控设置          |
| `-A` | 修改流控设置          |
| `-P` | 永久 MAC 地址         |

## 示例

```bash
ethtool eth0                     # 查看 eth0 基本参数（速率/双工）
ethtool -S eth0                  # 查看网卡统计信息（丢包等）
ethtool -i eth0                  # 查看网卡驱动信息
ethtool -K eth0 gro on          # 开启 GRO 硬件卸载
ethtool -G eth0 rx 1024          # 设置 RX Ring Buffer 为 1024
ethtool -L eth0 combined 4       # 设置组合通道（RSS 队列）数为 4
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-ethtool.html)
