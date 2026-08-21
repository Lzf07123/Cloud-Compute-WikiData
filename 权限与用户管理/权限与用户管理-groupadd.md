---
title: 权限与用户管理-groupadd
description: groupadd
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-groupadd` 🔐 — 创建用户组

## 作用

groupadd 创建新的用户组，支持指定 GID 和创建系统组。

## 语法

```
groupadd [选项] 组名
```

## 用法

groupadd 创建新组。`-g GID` 指定组 ID，`-r` 创建系统组（GID < 1000）。创建后可用 `usermod -aG 组 用户` 将用户加入该组。

## 常用参数

| 参数     | 说明                 |
| -------- | -------------------- |
| `-g GID` | 指定组 ID            |
| `-r`     | 创建系统组           |
| `-f`     | 若组已存在则成功退出 |
| `-p`     | 创建组密码           |

## 示例

```bash
groupadd DEVELOPERS            # 创建新组
groupadd -g 1001 PROJECTA      # 创建组并指定 GID
groupadd -r SYSGROUP           # 创建系统组
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-groupadd.html)

## 🔗 相关文档

[权限与用户管理-groupdel](权限与用户管理-groupdel.md) | [权限与用户管理-usermod](权限与用户管理-usermod.md) | [权限与用户管理-useradd](权限与用户管理-useradd.md) | [权限与用户管理-chgrp](权限与用户管理-chgrp.md)
