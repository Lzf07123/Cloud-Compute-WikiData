---
title: route-arp
description: route / arp
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-route-arp` / `arp` 🌐 — 路由与 ARP 缓存

## 作用

route 显示或操作内核 IP 路由表，添加/删除路由。arp 查看和修改 ARP 缓存表（IP 到 MAC 地址的映射）。两者均为传统网络工具，现代系统推荐使用 `ip route` 和 `ip neigh` 替代。

## route

```
route [选项]
```

查看路由表：`-n` 数字格式显示；`-e` 显示路由度量值；`-v` 版本。添加路由：`route add -net 192.168.2.0/24 gw 192.168.1.1`；添加默认网关：`route add default gw 192.168.1.1`；删除路由：`route del -net 192.168.2.0/24`。

## arp

```
arp [选项]
```

查看 ARP 缓存：`-a` 显示所有条目；`-n` 数字格式。`arp -d IP` 删除指定 ARP 条目，`arp -s IP MAC` 添加静态 ARP 条目。

## 常用参数

| 参数               | 说明              |
| ------------------ | ----------------- |
| `-n`（route）      | 数字格式          |
| `-e`（route）      | 显示度量值        |
| `-a`（arp）        | 显示所有 ARP 条目 |
| `-d IP`（arp）     | 删除 ARP 条目     |
| `-s IP MAC`（arp） | 添加静态条目      |

## 示例

```bash
route -n                         # 数字格式查看路由表
route add default gw 192.168.1.1 # 添加默认网关
route del -net 192.168.2.0/24    # 删除网段路由
arp -a                           # 查看所有 ARP 缓存条目
arp -d 192.168.1.100             # 删除指定 ARP 条目
```

> 已被 `ip route` 和 `ip neigh` 替代。

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-route.html)

## 🔗 相关文档

[网络管理-ip](网络管理-ip.md) | [网络管理-ifconfig](网络管理-ifconfig.md) | [网络管理-netstat](网络管理-netstat.md) | [网络管理-traceroute-mtr](网络管理-traceroute-mtr.md)
