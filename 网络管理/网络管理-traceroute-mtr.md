---
title: traceroute-mtr
description: traceroute / mtr
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-traceroute-mtr` / `mtr` 🌐 — 路由追踪

## 作用

traceroute 追踪数据包从本机到达目标主机经过的每一跳路由，用于诊断网络延迟、丢包位置和路由路径。mtr（My TraceRoute）是增强版，结合 traceroute 和 ping，实时显示每一跳的延迟和丢包率。

## traceroute

```
traceroute [选项] 主机
```

基本路径追踪。`-n` 不解析主机名（加快速度）；`-I` 使用 ICMP 包替代默认的 UDP；`-m N` 最大跳数（默认 30）；`-w 秒` 超时时间。

## mtr（增强版，实时诊断）

```
mtr [选项] 主机
```

实时动态显示路径中每一跳的丢包率和延迟。`-r` 报告模式（生成统计报告后退出）；`-4` 仅 IPv4；`-6` 仅 IPv6；`-C` CSV 格式输出。mtr 比 traceroute 更适合持续监控网络质量。

## 常用参数

| 参数                 | 说明         |
| -------------------- | ------------ |
| `-n`（traceroute）   | 不解析主机名 |
| `-I`（traceroute）   | 使用 ICMP    |
| `-m N`（traceroute） | 最大跳数     |
| `-r`（mtr）          | 报告模式     |
| `-4` / `-6`          | IPv4 / IPv6  |

## 示例

```bash
traceroute -n GOOGLE.COM        # 追踪路由（不解析主机名）
traceroute -I GOOGLE.COM        # 使用 ICMP 包追踪
mtr GOOGLE.COM                  # 实时动态监测路由质量
mtr -r GOOGLE.COM               # 报告模式（生成统计后退出）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-traceroute.html)

## 🔗 相关文档

[网络管理-ping](网络管理-ping.md) | [网络管理-dig-nslookup-host](网络管理-dig-nslookup-host.md) | [网络管理-nmap](网络管理-nmap.md) | [网络管理-nc](网络管理-nc.md)
