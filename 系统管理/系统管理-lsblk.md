---
title: 系统管理-lsblk
description: lsblk
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-lsblk` 🖥️ — 列出块设备

## 作用

lsblk（list block devices）列出系统中的所有块设备信息，包括磁盘、分区、LVM 逻辑卷、RAID 阵列、环回设备等。以树形结构展示设备间的从属关系，是了解磁盘拓扑的核心命令。

## 语法

```
lsblk [选项] [设备]
```

## 用法

lsblk 默认以树形结构列出所有块设备，显示设备名、大小、类型、挂载点等。`-f` 显示文件系统信息；`-m` 显示权限和所有者；`-t` 显示拓扑信息；`-P` 以键值对格式输出（方便脚本处理）；`-o` 列表格式输出；`-e` 排除指定设备类型（如回环设备）。可指定具体设备查看，如 `lsblk /dev/sda`。

## 常用参数

| 参数      | 说明             |
| --------- | ---------------- |
| `-f`      | 显示文件系统信息 |
| `-m`      | 显示权限和所有者 |
| `-t`      | 显示拓扑信息     |
| `-P`      | 键值对输出       |
| `-o`      | 列表格式         |
| `-e 类型` | 排除指定类型     |
| `-a`      | 显示空设备       |

## 示例

```bash
lsblk                           # 树形显示所有块设备
lsblk -f                        # 显示文件系统信息
lsblk -m                        # 显示权限和所有者
lsblk /dev/sda                  # 查看指定磁盘详情
lsblk -e 7                      # 排除环回设备
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-lsblk.html)

## 🔗 相关文档

[系统管理-lscpu](系统管理-lscpu.md) | [系统管理-lspci](系统管理-lspci.md)
