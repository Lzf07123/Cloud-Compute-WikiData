---
title: 权限与用户管理-passwd
description: passwd
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-passwd` 🔐 — 管理用户密码

## 作用

passwd（password）设置或修改用户密码，支持锁定/解锁账户、删除密码等操作。

## 语法

```
passwd [选项] [用户名]
```

## 用法

passwd 不带参数修改当前用户密码。root 可管理任意用户密码。`-l` 锁定账户，`-u` 解锁，`-d` 删除密码（变为空密码，谨慎），`--stdin` 从管道读取密码（脚本中使用）。普通用户只能改自己密码。

## 常用参数

| 参数      | 说明                       |
| --------- | -------------------------- |
| `-l`      | 锁定账户                   |
| `-u`      | 解锁账户                   |
| `-d`      | 删除密码                   |
| `--stdin` | 从 stdin 读密码            |
| `-S`      | 显示密码状态               |
| `-e`      | 过期密码（下次登录须修改） |

## 示例

```bash
passwd                        # 修改当前用户密码
passwd ALICE                  # root 修改 ALICE 密码
passwd -l ALICE               # 锁定账户
passwd -u ALICE               # 解锁账户
echo "P@SSWORD" | passwd --stdin ALICE  # 管道设置密码（脚本用）
passwd -S ALICE               # 查看密码状态
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-passwd.html)

## 🔗 相关文档

[权限与用户管理-useradd](权限与用户管理-useradd.md) | [权限与用户管理-usermod](权限与用户管理-usermod.md) | [权限与用户管理-su](权限与用户管理-su.md) | [权限与用户管理-sudo](权限与用户管理-sudo.md)
