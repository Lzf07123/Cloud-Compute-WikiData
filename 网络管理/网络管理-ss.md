---
title: 网络管理-ss
description: ss
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-ss` 🌐 — 查看套接字统计

## 作用

ss（socket statistics）显示套接字统计信息，比 `netstat` 更快更详细，是 `iproute2` 套件的一部分。用于查看网络连接、监听端口、套接字状态等，是排查网络问题的现代推荐工具。

## 语法

```
ss [选项] [过滤器]
```

## 用法

ss 常见用法：`-tulnp` 查看所有 TCP/UDP 监听端口及进程信息；`-4` 仅 IPv4；`-6` 仅 IPv6；`-s` 显示套接字统计概要；`-e` 显示详细的套接字信息；`-o` 显示计时器信息（如 keepalive）；`-i` 显示 TCP 内部信息（如缓冲区大小）；`-m` 显示内存使用。支持状态过滤，如 `ss state established` 显示已建立的连接。`ss -tulnp` 最常用，相当于 `netstat -tulnp`。

## 常用参数

| 参数 | 说明              |
| ---- | ----------------- |
| `-t` | TCP 套接字        |
| `-u` | UDP 套接字        |
| `-l` | 仅监听状态        |
| `-n` | 数字显示          |
| `-p` | 显示进程          |
| `-4` | 仅 IPv4           |
| `-6` | 仅 IPv6           |
| `-s` | 统计概要          |
| `-e` | 详细套接字信息    |
| `-o` | 显示计时器信息    |
| `-i` | 显示 TCP 内部信息 |
| `-m` | 内存使用          |

## 示例

```bash
ss -tulnp                        # 查看所有 TCP/UDP 监听端口及进程
ss -tulnp | grep 80              # 过滤端口 80 的监听
ss -s                            # 查看套接字统计概要
ss state established             # 查看已建立的连接
ss -4 -l                         # 查看 IPv4 监听端口
ss -t -e                         # 查看 TCP 套接字详细信息
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-ss.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 1：网络诊断](../学习路线与课程/week4-day1-网络诊断.md) | [Day 5：本周串联实操](../学习路线与课程/week4-day5-本周串联实操.md)

## 🔗 相关文档

[网络管理-netstat](网络管理-netstat.md) | [网络管理-lsof](网络管理-lsof.md) | [网络管理-tcpdump](网络管理-tcpdump.md) | [网络管理-ip](网络管理-ip.md)
