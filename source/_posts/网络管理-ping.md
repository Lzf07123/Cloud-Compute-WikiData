---
title: 网络管理-ping
description: ping
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-ping` 🌐 — 测试网络连通性

## 作用

ping 通过发送 ICMP（Internet Control Message Protocol）回显请求包并等待目标主机回复，检测网络连通性、测量往返时间（RTT）和丢包率，是网络故障排查最基础的工具。

## 语法

```
ping [选项] 目标
```

## 用法

ping 默认持续发送 ICMP 包直到手动停止（Ctrl+C）。`-c 次数` 发送指定数量后自动停止；`-i 秒` 指定发送间隔（默认 1 秒）；`-W 秒` 设置超时时间；`-s 字节` 指定包大小；`-q` 静默模式（只输出统计信息）；`-4` 使用 IPv4；`-6` 使用 IPv6。按 Ctrl+C 后显示丢包率和 RTT 统计。

## 常用参数

| 参数      | 说明     |
| --------- | -------- |
| `-c 次数` | 发送次数 |
| `-i 秒`   | 发送间隔 |
| `-W 秒`   | 超时时间 |
| `-s 字节` | 包大小   |
| `-q`      | 静默模式 |
| `-4`      | IPv4     |
| `-6`      | IPv6     |

## 示例

```bash
ping -c 4 GOOGLE.COM             # 发送 4 个包测试连通性
ping -c 10 -s 1400 192.168.1.1    # 发送 10 个 1400 字节的大包
ping -i 0.5 -c 20 BAIDU.COM      # 每 0.5 秒发 1 个包共 20 次
ping -4 -c 4 LOCALHOST            # 使用 IPv4 测试本机回环
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-ping.html)
