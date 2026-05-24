---
title: 文件与目录管理-mkdir
description: mkdir
tags:
  - linux
  - command
  - file-management
created: 2026-05-24
updated: 2026-05-24
category: 文件与目录管理
---

# `文件与目录管理-mkdir` 📁 — 创建目录

## 作用

mkdir（make directory）用于创建新目录，支持单级和多级目录创建，并可同时设置目录权限。

## 语法

```
mkdir [选项] 目录...
```

## 用法

mkdir 最常见用法是 `mkdir -p DIR/SUB/SUB2` 创建多级嵌套目录（自动创建父目录）。不加 `-p` 时若父目录不存在则报错。`-m 755` 在创建时直接设置权限。建议创建嵌套目录时始终加 `-p`。

## 常用参数

| 参数      | 说明           |
| --------- | -------------- |
| `-p`      | 自动创建父目录 |
| `-m 权限` | 设置目录权限   |
| `-v`      | 显示创建过程   |

## 示例

```bash
mkdir NEWDIR                 # 创建单级目录
mkdir -p A/B/C/D             # 创建多级嵌套目录
mkdir -m 755 SECUREDIR       # 创建目录并设置权限 755
mkdir DIR1 DIR2 DIR3         # 批量创建多个目录
mkdir -pv PARENT/CHILD       # 递归创建并显示过程
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-mkdir.html)
