---
title: 权限与用户管理-groupdel
description: groupdel
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-groupdel` 🔐 — 删除用户组

## 作用

groupdel（group delete）删除指定用户组，不能删除有用户作为主组的组。

## 语法

```
groupdel [选项] 组名
```

## 用法

groupdel 删除系统组记录。若该组是某用户的主组则无法删除（需先将用户删除或改其主组）。`-f` 强制删除。

## 常用参数

| 参数 | 说明                             |
| ---- | -------------------------------- |
| `-f` | 强制删除（即使用户的主组是该组） |

## 示例

```bash
groupdel DEVELOPERS            # 删除指定组
groupdel OLDSYSGROUP           # 删除旧系统组
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-groupdel.html)

## 🔗 相关文档

[权限与用户管理-groupadd](权限与用户管理-groupadd.md) | [权限与用户管理-userdel](权限与用户管理-userdel.md) | [权限与用户管理-usermod](权限与用户管理-usermod.md) | [权限与用户管理-chgrp](权限与用户管理-chgrp.md)
