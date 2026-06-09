---
title: 网络管理-tcpdump
description: tcpdump
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-tcpdump` 🌐 — 网络包抓取分析

## 作用

tcpdump 是一个命令行网络包分析工具，捕获并显示网络接口上传输的数据包内容。用于网络故障排查、协议分析、流量监控和安全审计，是网络管理员的核心工具。

## 语法

```
tcpdump [选项] [表达式]
```

## 用法

tcpdump 需要 root 权限运行。`-i 接口` 指定抓包接口（如 `eth0`）；`-n` 不解析主机名；`-x` 以十六进制显示包内容；`-X` 以十六进制和 ASCII 显示；`-v` 详细输出；`-C 文件大小` 按大小切割文件（单位 MB）；`-W 文件数` 指定切割文件数量；`-S` 绝对序列号；`-E 算法:密钥` 解密 IPSEC 流量。过滤表达式：`HOST 1.2.3.4` 按主机过滤；`PORT 80` 按端口过滤；`TCP`、`UDP`、`ICMP` 按协议过滤。Ctrl+C 停止抓取。

## 常用参数

| 参数      | 说明                  |
| --------- | --------------------- |
| `-i 接口` | 指定抓包接口          |
| `-n`      | 不解析主机名          |
| `-x`      | 十六进制显示          |
| `-X`      | 十六进制 + ASCII 显示 |
| `-v`      | 详细输出              |
| `-C 大小` | 切割文件（MB）        |
| `-W 数量` | 文件数量限制          |
| `-S`      | 绝对序列号            |

## 示例

```bash
tcpdump -i eth0                  # 抓取 eth0 所有流量
tcpdump -i eth0 port 80          # 抓取 eth0 上 80 端口流量
tcpdump -i eth0 host 192.168.1.1 # 抓取与特定主机的通信
tcpdump -i eth0 -X               # 抓包并以十六进制+ASCII 显示
tcpdump -i eth0 -C 100 -W 5      # 切割为 100MB 文件，保留 5 个
tcpdump -i eth0 icmp             # 仅抓取 ICMP（ping）包
tcpdump -i eth0 'TCP[13] & 2 != 0'  # 抓取 TCP SYN 包
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-tcpdump.html)
