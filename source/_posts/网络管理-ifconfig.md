---
title: 网络管理-ifconfig
description: ifconfig
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-ifconfig` 🌐 — 配置网络接口

## 作用

ifconfig（interface configuration）查看和配置网络接口参数，包括 IP 地址、子网掩码、MAC 地址、MTU、接口状态等。是传统网络配置工具，现代系统推荐使用 `ip` 命令替代。

## 语法

```
ifconfig [接口] [选项] [地址]
```

## 用法

ifconfig 不加参数显示所有活跃接口的详细信息。`-a` 显示所有接口（含禁用）；`-s` 以简短格式显示概要；`-v` 显示版本。设置操作：`ifconfig eth0 192.168.1.100 netmask 255.255.255.0` 配置 IP；`ifconfig eth0 up` 启用接口；`ifconfig eth0 down` 禁用接口；`ifconfig eth0 hw ether XX:XX:XX:XX:XX:XX` 修改 MAC 地址。

## 常用参数

| 参数           | 说明          |
| -------------- | ------------- |
| `-a`           | 显示所有接口  |
| `-s`           | 简短格式      |
| `up / down`    | 启用/禁用接口 |
| `netmask 掩码` | 设置子网掩码  |
| `hw ether MAC` | 设置 MAC 地址 |
| `mtu 值`       | 设置 MTU      |

## 示例

```bash
ifconfig                          # 查看所有活跃网卡信息
ifconfig -a                       # 查看所有网卡（含禁用状态）
ifconfig eth0 192.168.1.100 netmask 255.255.255.0  # 配置 IP 和掩码
ifconfig eth0 up                  # 启用 eth0 接口
ifconfig eth0 down                # 禁用 eth0 接口
ifconfig eth0 mtu 1500            # 设置 MTU 为 1500
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-ifconfig.html)
