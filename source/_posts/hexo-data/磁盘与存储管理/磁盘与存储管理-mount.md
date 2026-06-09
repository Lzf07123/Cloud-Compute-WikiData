---
title: 磁盘与存储管理-mount
description: mount
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-mount` 💾 — 挂载文件系统

## 作用

mount 将存储设备（磁盘分区、ISO 镜像、网络文件系统等）挂载到目录树的指定挂载点，使文件系统可被访问。是 Linux 存储管理中最核心的命令之一，配对的 `umount` 用于卸载。

## 语法

```
mount [选项] [设备] [挂载点]
```

## 用法

mount 不加参数显示当前已挂载的所有文件系统。`-t 类型` 指定文件系统类型（如 `ext4`、`ntfs`、`vfat`）；`-o 选项` 指定挂载选项（如 `ro` 只读、`rw` 读写、`noexec`、`nosuid`）；`-a` 挂载 `/etc/fstab` 中所有条目；`--bind` 绑定挂载（将目录挂载到另一位置）；`--remount` 或 `-o remount` 重新挂载。`umount 挂载点` 卸载文件系统。常用设备路径如 `/dev/sda1`、`/dev/nvme0n1p1`。

## 常用参数

| 参数        | 说明                      |
| ----------- | ------------------------- |
| `-t 类型`   | 文件系统类型              |
| `-o 选项`   | 挂载选项                  |
| `-a`        | 挂载 fstab 所有条目       |
| `--bind`    | 绑定挂载                  |
| `--remount` | 重新挂载（或 -o remount） |
| `-L 标签`   | 按卷标挂载                |

## 示例

```bash
mount /dev/sda1 /mnt                  # 挂载分区到 /mnt
mount -t ext4 /dev/sda1 /mnt          # 指定 ext4 类型挂载
mount -o ro /dev/sda1 /mnt            # 以只读模式挂载
mount -a                              # 挂载 fstab 中所有设备
mount --bind /SRC /DEST               # 绑定挂载（目录镜像）
mount -L DATA /mnt/DATA               # 按卷标挂载
umount /mnt                           # 卸载 /mnt 挂载点
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-mount.html)

## 🔗 相关文档

{% post_link 磁盘与存储管理/磁盘与存储管理-fdisk %} | {% post_link 磁盘与存储管理/磁盘与存储管理-mkfs %} | {% post_link 磁盘与存储管理/磁盘与存储管理-findmnt %} | {% post_link 磁盘与存储管理/磁盘与存储管理-blkid %}
