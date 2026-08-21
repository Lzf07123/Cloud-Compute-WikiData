---
title: 磁盘与存储管理-fsck
description: fsck
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-fsck` 💾 — 文件系统检查修复

## 作用

fsck（file system consistency check）用于检查和修复文件系统错误。当系统非正常关机、磁盘出现坏道或文件系统元数据损坏时，fsck 可检测并尝试修复。需要设备未挂载或以只读方式挂载。

## 语法

```
fsck [选项] 设备
```

## 用法

fsck 自动检测文件系统类型并调用对应的检查工具（如 `fsck.ext4`、`fsck.xfs`）。`-A` 检查 `/etc/fstab` 中所有文件系统；`-R` 跳过根文件系统（配合 `-A`）；`-t 类型` 指定文件系统类型；`-N` 不执行仅显示；`-V` 详细输出；`-p` 尝试自动修复（无需确认）；`-C` 显示进度条；`-y` 对所有问题回答 YES（EXT 系列）。建议先 `umount` 设备再运行 fsck。

## 常用参数

| 参数      | 说明                |
| --------- | ------------------- |
| `-A`      | 检查 fstab 所有条目 |
| `-R`      | 跳过根文件系统      |
| `-t 类型` | 指定类型            |
| `-N`      | 仅显示不执行        |
| `-V`      | 详细输出            |
| `-p`      | 自动修复            |
| `-C`      | 显示进度条          |
| `-y`      | 自动确认            |

## 示例

```bash
fsck /dev/sda1                      # 检查 sda1 文件系统
fsck -p /dev/sda1                   # 自动修复文件系统错误
fsck -C /dev/sda1                   # 显示进度条
fsck -A                             # 检查 fstab 中所有文件系统
fsck -N /dev/sda1                   # 仅显示将执行的操作（不实际运行）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-fsck.html)

## 🔗 相关文档

[磁盘与存储管理-mount](磁盘与存储管理-mount.md) | [磁盘与存储管理-mkfs](磁盘与存储管理-mkfs.md) | [磁盘与存储管理-blkid](磁盘与存储管理-blkid.md)
