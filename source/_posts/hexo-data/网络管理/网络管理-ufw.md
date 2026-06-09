---
title: 网络管理-ufw
description: ufw
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-ufw` 🌐 — 防火墙管理

## 作用

ufw（uncomplicated firewall）是 iptables 的简化前端，提供用户友好的命令行接口来管理防火墙规则。是 Ubuntu 等发行版的默认防火墙管理工具，适合快速配置基本的入站/出站流量规则。

## 语法

```
ufw [选项] [操作] [规则]
```

## 用法

ufw 默认拒绝入站流量、允许出站流量。`enable` / `disable` 启用/禁用防火墙；`status` 查看当前规则；`status verbose` 查看详细信息；`allow 端口/协议` 允许入站流量；`deny 端口/协议` 拒绝入站流量；`delete 规则编号` 删除指定规则；`default allow/deny incoming` 设置默认策略。规则示例：`ufw allow 22/tcp`、`ufw allow 80/tcp`、`ufw allow from 192.168.1.0/24`。

## 常用参数

| 参数           | 说明       |
| -------------- | ---------- |
| `enable`       | 启用防火墙 |
| `disable`      | 禁用防火墙 |
| `status`       | 查看状态   |
| `allow 端口`   | 允许端口   |
| `deny 端口`    | 拒绝端口   |
| `delete 编号`  | 删除规则   |
| `default 策略` | 默认策略   |
| `--dry-run`    | 模拟运行   |

## 示例

```bash
ufw enable                       # 启用防火墙
ufw allow 22/tcp                 # 允许 SSH 端口
ufw allow 80/tcp                 # 允许 HTTP 端口
ufw allow from 192.168.1.0/24    # 允许来自该网段的流量
ufw deny 23                      # 拒绝 Telnet 端口
ufw status verbose               # 查看详细防火墙状态
ufw delete 1                     # 删除第一条规则
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-ufw.html)
