---
title: 网络管理-ip
description: ip
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-ip` 🌐 — 网络配置管理工具

## 作用

ip 是 Linux 现代网络配置命令，集成了 `ifconfig`、`route`、`arp` 等传统工具的功能，用于管理网络接口、路由表、ARP 缓存、隧道等网络资源。是 `iproute2` 软件包的核心命令。

## 语法

```
ip [选项] 对象 操作
```

## 用法

ip 通过不同对象管理网络：`ip link` 管理网络接口（查看/启用/禁用/MTU）；`ip addr` 管理 IP 地址；`ip route` 管理路由表；`ip neigh` 管理 ARP/ND 缓存。`ip link show` 查看接口；`ip link set eth0 up/down` 启用/禁用接口；`ip addr add 192.168.1.100/24 dev eth0` 添加 IP；`ip route add default via 192.168.1.1` 添加默认网关。

## 常用参数

### 对象

| 对象    | 说明         |
| ------- | ------------ |
| `link`  | 网络接口管理 |
| `addr`  | IP 地址管理  |
| `route` | 路由表管理   |
| `neigh` | ARP 缓存管理 |

### 全局选项

| 选项 | 说明     |
| ---- | -------- |
| `-4` | 仅 IPv4  |
| `-6` | 仅 IPv6  |
| `-s` | 统计信息 |

## 示例

```bash
ip link show                     # 查看所有网络接口状态
ip addr show                     # 查看所有 IP 地址
ip route show                    # 查看路由表
ip addr add 192.168.1.100/24 dev eth0  # 为 eth0 添加 IP 地址
ip link set eth0 up              # 启用 eth0 接口
ip route add default via 192.168.1.1  # 添加默认网关
ip -s link show eth0             # 查看 eth0 的统计信息
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-ip.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week4-day1-网络诊断 "Day 1：网络诊断" %} | {% post_link 学习路线与课程/week4-day5-本周串联实操 "Day 5：本周串联实操" %}
