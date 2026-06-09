---
title: 系统管理-dmesg
description: dmesg
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-dmesg` 🖥️ — 显示内核日志

## 作用

dmesg（display message）用于显示和控制内核环形缓冲区中的消息，通常包含系统启动以来的硬件检测、驱动加载、设备连接/断开等内核相关信息。是诊断硬件问题、查看系统启动信息的首选工具。

## 语法

```
dmesg [选项]
```

## 用法

dmesg 默认输出所有内核日志，信息量很大。常用过滤方式：`-l` 按日志级别过滤（如 `-l err` 只看错误）；`-T` 将时间戳转为可读格式；`-w` 实时跟踪新日志；`--level` 按级别过滤（emerg/alert/crit/err/warning/notice/info/debug）；`-n 级别` 设置控制台日志级别 (0-7)。结合 `grep` 可定位硬件信息，如 `dmesg | grep USB`、`dmesg | grep -i error`。

## 常用参数

| 参数      | 说明                     |
| --------- | ------------------------ |
| `-T`      | 可读时间戳               |
| `-l 级别` | 按级别过滤               |
| `-w`      | 实时跟踪新日志           |
| `-n 级别` | 设置控制台日志级别 (0-7) |
| `--level` | 按级别名过滤             |
| `-H`      | 人类可读输出             |
| `-C`      | 清空环形缓冲区           |

## 示例

```bash
dmesg | grep USB                # 搜索 USB 设备相关日志
dmesg -T                        # 可读时间戳显示
dmesg -w                        # 实时跟踪新内核消息
dmesg -l err                    # 只显示错误级别日志
dmesg | grep -i error           # 搜索错误信息（忽略大小写）
dmesg -n 1                      # 设置控制台日志级别为 1（仅紧急和警报）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-dmesg.html)

## 🔗 相关文档

{% post_link 系统管理/系统管理-journalctl %} | {% post_link 系统管理/系统管理-systemctl %}
