---
title: 磁盘与存储管理-parted
description: parted
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-parted` 💾 — 磁盘分区（GPT 支持）

## 作用

parted 是一个功能强大的磁盘分区工具，支持 MBR 和 GPT 两种分区表格式，尤其擅长处理大于 2TB 的 GPT 分区。提供交互式和命令行两种模式，比 `fdisk` 更灵活。

## 语法

```
parted [选项] [设备] [命令]
```

## 用法

parted 可交互运行或直接执行命令。`-l` 列出所有磁盘设备；`-s` 脚本模式（不等待确认）；`-a ALIGNMENT` 设置对齐方式。交互模式命令：`print` 打印分区表；`mkpart 类型 文件系统 起始 结束` 创建分区；`rm 编号` 删除分区；`resizepart 编号 结束位置` 调整分区大小（需确保数据安全）。单位支持 `GB`、`MB`、`%` 等。

## 常用参数

| 参数         | 说明         |
| ------------ | ------------ |
| `-l`         | 列出所有设备 |
| `-s`         | 脚本模式     |
| `-a 对齐`    | 设置对齐方式 |
| `print`      | 打印分区表   |
| `mkpart`     | 创建分区     |
| `rm`         | 删除分区     |
| `resizepart` | 调整分区大小 |

## 示例

```bash
parted -l                           # 列出所有磁盘设备
parted /dev/sda print               # 打印 sda 分区表
parted /dev/sda mkpart primary ext4 0% 10GB  # 创建 10GB 主分区
parted /dev/sda rm 1                # 删除分区 1
parted /dev/sda resizepart 1 20GB   # 将分区 1 调整为 20GB
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-parted.html)
