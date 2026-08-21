---
title: 权限与用户管理-id
description: id
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-id` 🔐 — 显示用户身份

## 作用

id 显示当前或指定用户的 UID、GID 和所属组信息，确认身份的常用命令。

## 语法

```
id [选项] [用户名]
```

## 用法

id 输出用户数字/名称 ID 及所有组成员。`-u` 只显示 UID，`-g` 只显示 GID，`-n`（须与 `-u`/`-g`/`-G` 配合）显示名称而非数字，`-G` 显示所有附加组 GID。无参数显示当前用户信息。

## 常用参数

| 参数 | 说明                |
| ---- | ------------------- |
| `-u` | 显示 UID            |
| `-g` | 显示 GID            |
| `-n` | 显示名称而非数字    |
| `-G` | 显示所有组 ID       |
| `-Z` | 显示 SELinux 上下文 |

## 示例

```bash
id                        # 显示当前用户的完整身份信息
id -u                     # 只显示 UID
id -g                     # 只显示 GID
id -un                    # 显示当前用户名（-n 配合 -u）
id ALICE                  # 查看指定用户的身份
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-id.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 1：用户身份与基本权限](../学习路线与课程/week2-day1-用户身份与基本权限.md) | [Day 5：本周串联实操](../学习路线与课程/week2-day5-本周串联实操.md)

## 🔗 相关文档

[权限与用户管理-whoami](权限与用户管理-whoami.md) | [权限与用户管理-users](权限与用户管理-users.md) | [权限与用户管理-who](权限与用户管理-who.md) | [权限与用户管理-w](权限与用户管理-w.md)
