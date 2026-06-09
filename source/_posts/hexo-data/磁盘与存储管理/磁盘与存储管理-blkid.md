---
title: 磁盘与存储管理-blkid
description: blkid
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-blkid` 💾 — 查看块设备属性

## 作用

blkid（block ID）用于查看块设备的 UUID、文件系统类型、卷标等属性信息。在配置 `/etc/fstab` 自动挂载时，常用 UUID 替代设备名以避免设备名变动导致的问题。

## 语法

```
blkid [选项] [设备]
```

## 用法

blkid 不加参数列出所有块设备的属性。`-o 格式` 指定输出格式（如 value/full/export）；`-L 标签` 按卷标查找（如 `-L DATA`）；`-U UUID` 按 UUID 查找；`-p` 从磁盘重新读取（不依赖缓存）；`-i` 显示 I/O 限制信息；`-V` 显示版本。输出包含 `DEVICE`、`UUID`、`TYPE`、`LABEL` 等信息。

## 常用参数

| 参数      | 说明                   |
| --------- | ---------------------- |
| `-o 格式` | 输出格式（value/full） |
| `-L 标签` | 按卷标查找             |
| `-U UUID` | 按 UUID 查找           |
| `-p`      | 重新读取属性           |
| `-i`      | 显示 I/O 限制信息      |

## 示例

```bash
blkid                               # 列出所有块设备属性
blkid /dev/sda1                     # 查看 sda1 的 UUID 和类型
blkid -L DATA                       # 按卷标 DATA 查找设备
blkid -U ABC123-DEF456              # 按 UUID 查找设备
blkid -i /dev/sda1                  # 显示 I/O 限制信息
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-blkid.html)

## 🔗 相关文档

{% post_link 磁盘与存储管理/磁盘与存储管理-mount %} | {% post_link 磁盘与存储管理/磁盘与存储管理-fdisk %} | {% post_link 磁盘与存储管理/磁盘与存储管理-mkfs %}
