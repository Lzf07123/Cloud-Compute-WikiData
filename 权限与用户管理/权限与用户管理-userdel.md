---
title: 权限与用户管理-userdel
description: userdel
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-userdel` 🔐 — 删除用户

## 作用

userdel（user delete）删除用户账户，默认只删除账户不删除其家目录和邮件池。

## 语法

```
userdel [选项] 用户名
```

## 用法

userdel 默认只从系统文件中移除用户条目。`-f` 强制删除（即使用户登录中），`-r` 同时删除家目录和邮件池。**注意**：无法删除正在登录的用户（除非 `-f`）。删除前应确认用户不再需要其数据。

## 常用参数

| 参数 | 说明                     |
| ---- | ------------------------ |
| `-f` | 强制删除（登录中也可删） |
| `-r` | 同时删除家目录和邮件池   |
| `-Z` | 删除 SELinux 用户映射    |

## 示例

```bash
userdel ALICE                  # 删除用户账户
userdel -r BOB                 # 删除用户及其家目录
userdel -f CHARLIE             # 强制删除（即使登录中）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-userdel.html)

## 🔗 相关文档

[权限与用户管理-useradd](权限与用户管理-useradd.md) | [权限与用户管理-usermod](权限与用户管理-usermod.md) | [权限与用户管理-groupdel](权限与用户管理-groupdel.md) | [权限与用户管理-passwd](权限与用户管理-passwd.md)
