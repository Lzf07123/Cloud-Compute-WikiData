---
title: 系统管理-lsusb
description: lsusb
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-lsusb` 🖥️ — 列出 USB 设备

## 作用

lsusb 列出系统中的所有 USB（Universal Serial Bus）总线设备信息，包括 USB 控制器、集线器、外设（键盘、鼠标、U盘等）。用于查看 USB 设备的厂商、型号、速度和驱动等信息。

## 语法

```
lsusb [选项]
```

## 用法

lsusb 默认以简洁格式列出所有 USB 设备。`-v` 显示详细信息；`-t` 以树形结构显示 USB 总线拓扑（集线器层级）；`-d [厂商:产品]` 按厂商/产品 ID 过滤；`-D 设备文件` 显示指定 USB 设备的详细信息；`-s [[域:]总线:设备]` 过滤指定总线上的设备。`lsusb -t` 能直观展示设备间的层级关系，便于排查 USB 连接问题。

## 常用参数

| 参数             | 说明                |
| ---------------- | ------------------- |
| `-v`             | 详细输出            |
| `-t`             | 树形拓扑结构        |
| `-d [厂商:产品]` | 按厂商/产品 ID 过滤 |
| `-D 设备文件`    | 显示指定设备详情    |
| `-s 总线`        | 过滤指定总线        |
| `-n`             | 数字 ID 显示        |

## 示例

```bash
lsusb                           # 列出所有 USB 设备
lsusb -v                        # 显示详细信息
lsusb -t                        # 树形拓扑显示
lsusb -D /dev/bus/usb/001/002  # 查看指定 USB 设备的详情
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-lsusb.html)
