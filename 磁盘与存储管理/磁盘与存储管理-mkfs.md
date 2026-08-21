---
title: 磁盘与存储管理-mkfs
description: mkfs
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-mkfs` 💾 — 创建文件系统

## 作用

mkfs（make filesystem）在磁盘分区上创建文件系统（格式化）。是磁盘分区后必须执行的步骤，支持多种文件系统类型如 EXT4、XFS、BTRFS、VFAT 等。

## 语法

```
mkfs [选项] [类型] 设备
```

## 用法

mkfs 通过 `-t 类型` 指定文件系统类型（或使用专用命令如 `mkfs.ext4`、`mkfs.xfs`）。`-L 标签` 设置卷标；`-b 块大小` 指定块大小（如 1024/2048/4096）；`-i 字节比` 每 inode 字节比（值越小 inode 越多）；`-I 大小` inode 大小；`-N 节点数` 指定 inode 数量；`-F` 强制格式化（即使设备已挂载或有文件系统）；`-V` 详细输出。

## 常用参数

| 参数        | 说明            |
| ----------- | --------------- |
| `-t 类型`   | 文件系统类型    |
| `-L 标签`   | 设置卷标        |
| `-b 大小`   | 块大小          |
| `-i 字节比` | 每 inode 字节比 |
| `-I 大小`   | inode 大小      |
| `-N 数量`   | inode 数量      |
| `-F`        | 强制格式化      |
| `-V`        | 详细输出        |

## 示例

```bash
mkfs -t ext4 /dev/sda1               # 格式化为 ext4
mkfs -t xfs /dev/sda2                # 格式化为 XFS
mkfs -t vfat /dev/sdb1               # 格式化为 FAT32
mkfs.ext4 -L DATA /dev/sda1          # 格式化并设置卷标
mkfs -t ext4 -F /dev/sda1            # 强制格式化（覆盖已有文件系统）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-mkfs.html)

## 🔗 相关文档

[磁盘与存储管理-fdisk](磁盘与存储管理-fdisk.md) | [磁盘与存储管理-mount](磁盘与存储管理-mount.md) | [磁盘与存储管理-blkid](磁盘与存储管理-blkid.md)
