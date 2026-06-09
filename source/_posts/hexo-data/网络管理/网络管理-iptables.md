---
title: 网络管理-iptables
description: iptables
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-iptables` 🌐 — 防火墙规则管理

## 作用

管理 Linux 内核防火墙规则（IPv4）

## 语法

```
iptables [操作] [链] [匹配] [动作]
```

## 用法

iptables 基于表（filter/nat/mangle）和链（INPUT/OUTPUT/FORWARD）管理包过滤规则。`-A` 追加规则，`-D` 删除规则，`-L` 列表现有规则。`--line-numbers` 显示规则编号便于操作。

## 常用操作

| 操作            | 说明         |
| --------------- | ------------ |
| `-L -n -v`      | 查看规则     |
| `-A INPUT`      | 追加规则     |
| `-D INPUT`      | 删除规则     |
| `-P INPUT DROP` | 设置默认策略 |
| `-F`            | 清空规则     |

## 示例

```bash
sudo iptables -L -n -v
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-iptables.html)
