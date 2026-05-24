---
title: 网络管理-netstat
description: netstat
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-netstat` 🌐 — 查看网络连接状态

## 作用

netstat（network statistics）显示网络连接、路由表、接口统计、伪连接等信息。是排查网络连接问题、查看端口监听状态的传统工具。现代系统推荐使用 `SS` 和 `IP` 替代。

## 语法

```
netstat [选项]
```

## 用法

netstat 常用组合：`-tulnp` 查看所有 TCP/UDP 监听端口及对应进程；`-i` 显示接口统计；`-r` 显示路由表；`-s` 按协议显示统计信息；`-c` 持续刷新。`-p` 显示进程信息；`-a` 显示所有套接字（含非监听）；`-A 地址族` 指定地址族（如 inet/unix）。

## 常用参数

| 参数        | 说明              |
| ----------- | ----------------- |
| `-t`        | TCP 连接          |
| `-u`        | UDP 连接          |
| `-l`        | 仅监听状态        |
| `-n`        | 数字显示地址/端口 |
| `-p`        | 显示进程信息      |
| `-a`        | 显示所有套接字    |
| `-A 地址族` | 指定地址族        |
| `-i`        | 接口统计          |
| `-r`        | 路由表            |
| `-s`        | 协议统计          |
| `-c`        | 持续刷新          |

## 示例

```bash
netstat -tulnp                   # 查看所有 TCP/UDP 监听端口及进程
netstat -i                       # 查看网络接口统计
netstat -r                       # 查看路由表
netstat -s                       # 按协议查看统计信息
netstat -tulnp | grep 80         # 过滤端口 80 的监听
netstat -c                       # 持续刷新网络状态
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-netstat.html)
