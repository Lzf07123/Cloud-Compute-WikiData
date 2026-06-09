---
title: 权限与用户管理-umask
description: umask
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-umask` 🔐 — 设置默认权限掩码

## 作用

umask 指定新建文件和目录应去除的权限，默认掩码通常是 0022（文件 644、目录 755）。

## 语法

```
umask [选项] [掩码]
```

## 用法

umask 是 Shell 内置命令，设置当前 Shell 的文件创建默认权限。掩码为 0022 表示新建文件默认 `666 - 022 = 644`，目录 `777 - 022 = 755`。`-S` 以符号形式显示。更小的掩码意味着更高的默认权限。可在 `~/.bashrc` 中永久设置。

## 常用参数

| 参数 | 说明         |
| ---- | ------------ |
| `-S` | 符号模式显示 |
| `-p` | 数字格式显示 |

## 示例

```bash
umask                         # 查看当前掩码
umask -S                      # 符号模式显示
umask 0027                    # 设置掩码为 0027（文件 640，目录 750）
umask 0002                    # 设置掩码为 0002（文件 664，目录 775）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-umask.html)
