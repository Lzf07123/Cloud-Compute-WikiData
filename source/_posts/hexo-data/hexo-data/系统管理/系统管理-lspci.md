---
title: 系统管理-lspci
description: lspci
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-lspci` 🖥️ — 列出 PCI 设备

## 作用

lspci 列出系统中的所有 PCI（Peripheral Component Interconnect）总线设备信息，包括显卡、网卡、声卡、存储控制器等。是查看硬件设备型号、驱动信息和总线拓扑的重要命令。

## 语法

```
lspci [选项]
```

## 用法

lspci 默认以简洁格式列出所有 PCI 设备。`-v` 显示详细信息（含驱动、IRQ、内存地址等）；`-t` 以树形结构显示总线拓扑；`-k` 显示内核驱动信息；`-n` 以数字形式显示厂商和设备 ID；`-s [[域:]总线:设备:功能]` 过滤指定设备。配合 `grep` 可快速定位特定硬件，如 `lspci | grep -i vga` 查显卡。

## 常用参数

| 参数             | 说明                    |
| ---------------- | ----------------------- |
| `-v`             | 详细输出                |
| `-t`             | 树形拓扑结构            |
| `-k`             | 显示内核驱动            |
| `-n`             | 数字 ID 显示            |
| `-s 地址`        | 过滤指定设备            |
| `-d [厂商:设备]` | 按厂商/设备 ID 过滤显示 |

## 示例

```bash
lspci                           # 列出所有 PCI 设备
lspci -v                        # 显示详细信息
lspci -t                        # 树形拓扑显示
lspci -k                        # 显示内核驱动信息
lspci | grep -i vga             # 搜索显卡设备
lspci -s 00:02.0 -v             # 查看指定设备的详情
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-lspci.html)

## 🔗 相关文档

{% post_link 系统管理/系统管理-lscpu %} | {% post_link 系统管理/系统管理-lsusb %} | {% post_link 系统管理/系统管理-dmidecode %}
