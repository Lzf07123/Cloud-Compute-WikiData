---
title: swapon-swapoff
description: swapon / swapoff
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-swapon-swapoff` / `swapoff` 💾 — 管理交换空间

## 作用

swapon 启用指定的交换分区或交换文件，swapoff 禁用之。交换空间是磁盘上用于扩展物理内存的区域，当物理内存不足时系统将部分数据交换到磁盘。

## 语法

```
swapon [选项] [设备或文件]
swapoff [选项] [设备或文件]
```

## 用法

swapon 不加参数显示当前已启用的交换空间。`-a` 启用 `/etc/fstab` 中所有交换条目；`-s` 显示交换摘要信息（设备、大小、已用）；`-p 优先级` 设置交换优先级（数值越高优先使用）；`-v` 详细信息。创建交换空间流程：先 `fallocate -l 2G /swapfile` 创建交换文件，`mkswap /swapfile` 格式化，`swapon /swapfile` 启用。`swapoff -a` 禁用所有交换。

## 常用参数

| 参数        | 说明          |
| ----------- | ------------- |
| `-a`        | 全部启用/禁用 |
| `-s`        | 显示摘要      |
| `-p 优先级` | 设置优先级    |
| `-v`        | 详细信息      |

## 示例

```bash
swapon                              # 查看已启用的交换空间
swapon -s                           # 显示交换空间摘要
swapon -a                           # 启用 fstab 中所有交换条目
swapon /swapfile                    # 启用指定的交换文件
swapon -p 10 /dev/sda3              # 设置交换优先级为 10
swapoff /swapfile                   # 禁用交换文件
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-swapon.html)

## 🔗 相关文档

{% post_link 磁盘与存储管理/磁盘与存储管理-mount %} | {% post_link 系统管理/系统管理-free %}
