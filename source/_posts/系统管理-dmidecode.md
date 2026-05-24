---
title: 系统管理-dmidecode
description: dmidecode
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-dmidecode` 🖥️ — 查看硬件信息

## 作用

dmidecode（DMI table decoder）用于读取系统的 DMI（Desktop Management Interface）表，输出硬件组件的详细信息，包括 BIOS 版本、主板型号、内存插槽、CPU 信息、序列号等。是获取底层硬件信息的利器，常用于服务器硬件巡检和资产登记。

## 语法

```
dmidecode [选项]
```

## 用法

dmidecode 默认输出全部 DMI 信息，内容详尽。`-t 类型` 按类型过滤，常用类型：`1` 系统信息（制造商/产品名/序列号）、`2` 主板信息、`4` CPU 信息、`17` 内存设备。`-s 关键字` 按字符串搜索。`-q` 只显示关键信息（不显示未知或空值）。需要 root 权限查看完整信息。

## 常用参数

| 参数        | 说明                   |
| ----------- | ---------------------- |
| `-t 类型`   | 按 DMI 类型过滤        |
| `-s 关键字` | 按字符串搜索           |
| `-q`        | 精简输出（隐藏空值）   |
| `-h`        | 显示帮助（含类型列表） |
| `--version` | 显示版本               |

## 示例

```bash
dmidecode -t 1                  # 查看系统信息（制造商/序列号）
dmidecode -t 4                  # 查看 CPU 信息
dmidecode -t 17                 # 查看内存插槽信息
dmidecode -s system-serial-number  # 搜索序列号
dmidecode -q                    # 精简输出，隐藏空值
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-dmidecode.html)
