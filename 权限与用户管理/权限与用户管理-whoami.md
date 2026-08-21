---
title: 权限与用户管理-whoami
description: whoami
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-whoami` 🔐 — 显示当前用户名

## 作用

whoami 打印当前有效 UID 对应的用户名，等价于 `id -un`。

## 语法

```
whoami
```

## 用法

whoami 无参数，直接输出当前用户名。在 sudo 状态下可能与实际登录用户不同：`sudo whoami` 输出 root。适合脚本中确认当前运行用户身份。

## 示例

```bash
whoami                     # 显示当前用户名
sudo whoami                # sudo 下输出 root
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-whoami.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 1：用户身份与基本权限](../学习路线与课程/week2-day1-用户身份与基本权限.md) | [Day 5：本周串联实操](../学习路线与课程/week2-day5-本周串联实操.md)

## 🔗 相关文档

[权限与用户管理-id](权限与用户管理-id.md) | [权限与用户管理-who](权限与用户管理-who.md) | [权限与用户管理-w](权限与用户管理-w.md) | [权限与用户管理-users](权限与用户管理-users.md)
