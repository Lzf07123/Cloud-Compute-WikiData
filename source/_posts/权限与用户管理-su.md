---
title: 权限与用户管理-su
description: su
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-su` 🔐 — 切换用户身份

## 作用

su（switch user）切换当前登录用户身份，不加参数默认切换到 root。

## 语法

```
su [选项] [用户]
```

## 用法

su 不接参数切换到 root（需 root 密码）。`-` 模拟登录环境（加载目标用户的 Shell 配置和环境变量）。`-c 命令` 以目标用户身份执行单条命令后返回。相比 sudo，su 需知道目标用户密码。

## 常用参数

| 参数        | 说明                         |
| ----------- | ---------------------------- |
| `-`         | 模拟登录（加载目标用户配置） |
| `-c 命令`   | 执行命令后返回               |
| `-s`        | 指定 Shell                   |
| `-m` / `-p` | 保留当前环境变量             |

## 示例

```bash
su                         # 切换到 root（需 root 密码）
su -                       # 切换 root 并加载环境配置
su -c "whoami"             # 以目标用户执行命令后返回
su - JOHN                  # 切换到用户 JOHN
su -s /bin/bash            # 指定 Shell 切换用户
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-su.html)
