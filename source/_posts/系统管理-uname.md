---
title: 系统管理-uname
description: uname
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-uname` 🖥️ — 显示系统信息

## 作用

uname（unix name）输出操作系统内核名称、主机名、内核版本、硬件架构等系统信息。

## 语法

```
uname [选项]
```

## 用法

uname 查看系统基本信息。`-a` 输出全部信息（内核名、主机名、版本、架构），`-r` 内核版本号，`-m` 硬件架构名，`-n` 网络主机名。确认系统类型和内核版本时首选。

## 常用参数

| 参数 | 说明         |
| ---- | ------------ |
| `-a` | 全部信息     |
| `-r` | 内核版本     |
| `-m` | 硬件架构     |
| `-n` | 网络主机名   |
| `-s` | 内核名       |
| `-v` | 内核版本详情 |

## 示例

```bash
uname -a                     # 显示全部系统信息
uname -r                     # 查看内核版本号
uname -m                     # 查看硬件架构（如 x86_64）
uname -n                     # 查看网络主机名
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-uname.html)
