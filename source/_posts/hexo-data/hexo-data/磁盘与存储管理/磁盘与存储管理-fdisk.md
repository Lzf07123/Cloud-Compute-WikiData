---
title: 磁盘与存储管理-fdisk
description: fdisk
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-fdisk` 💾 — 磁盘分区管理

## 作用

fdisk 创建、删除、查看和修改磁盘分区表。支持 MBR（传统）和 GPT（现代）两种分区表格式，是 Linux 磁盘分区管理最经典的工具。需要 root 权限运行。

## 语法

```
fdisk [选项] 设备
```

## 用法

fdisk 使用交互式菜单操作分区表。`-l` 列出所有磁盘设备；`-b 大小` 指定扇区大小；`-v` 显示版本；`-V` 验证分区表（--verify）。交互模式命令：`p` 打印分区表；`n` 新建分区；`d` 删除分区；`w` 写入并退出；`q` 不保存退出。注意：`w` 会实际写入磁盘，操作前需确认。

## 常用参数

| 参数      | 说明         |
| --------- | ------------ |
| `-l`      | 列出所有磁盘 |
| `-b 大小` | 扇区大小     |
| `-v`      | 显示版本     |
| `-V`      | 验证分区表   |

## 示例

```bash
fdisk -l                            # 列出所有磁盘设备
fdisk /dev/sda                      # 交互式管理 sda 分区
```

> 交互操作：`p` 查看分区，`n` 新建，`d` 删除，`w` 写入。

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-fdisk.html)

## 🔗 相关文档

{% post_link 磁盘与存储管理/磁盘与存储管理-mount %} | {% post_link 磁盘与存储管理/磁盘与存储管理-mkfs %} | {% post_link 磁盘与存储管理/磁盘与存储管理-parted %} | {% post_link 磁盘与存储管理/磁盘与存储管理-blkid %}
