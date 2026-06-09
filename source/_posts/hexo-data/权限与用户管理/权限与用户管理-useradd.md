---
title: 权限与用户管理-useradd
description: useradd
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-useradd` 🔐 — 创建用户

## 作用

useradd 创建新的用户账户，支持设置家目录、Shell、组、UID 等属性。

## 语法

```
useradd [选项] 用户名
```

## 用法

useradd 默认不创建家目录（部分发行版除外）。`-m` 创建家目录，`-G 组` 指定附加组，`-s /bin/bash` 指定登录 Shell，`-u UID` 指定 UID，`-p 密码` 设置密码（不安全，建议后用 passwd）。创建用户后需用 `passwd 用户名` 设置密码。

## 常用参数

| 参数       | 说明           |
| ---------- | -------------- |
| `-m`       | 创建家目录     |
| `-G 组`    | 指定附加组     |
| `-s SHELL` | 指定登录 Shell |
| `-u UID`   | 指定用户 ID    |
| `-g 主组`  | 指定主组       |
| `-p 密码`  | 设置密码       |
| `-d 目录`  | 指定家目录路径 |
| `-r`       | 创建系统用户   |

## 示例

```bash
useradd -m ALICE                # 创建用户并创建家目录
useradd -m -G sudo -s /bin/bash BOB  # 创建用户加入 sudo 组并指定 Shell
useradd -u 1001 -m CHARLIE      # 指定 UID 为 1001
useradd -r -s /sbin/nologin SYSUSER  # 创建系统用户，不可登录
passwd ALICE                    # 创建用户后设置密码
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-useradd.html)
