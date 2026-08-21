---
title: 系统管理-free
description: free
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-free` 🖥️ — 查看内存使用

## 作用

free 显示物理内存和交换分区的总量、已用、可用和缓冲区使用情况。

## 语法

```
free [选项]
```

## 用法

free 查看内存使用统计。`-h` 人类可读格式（K/M/G），`-w` 加宽显示，`-t` 显示总量行。`-s N` 每秒刷新。排查内存问题时关注 `available` 列（实际可用）而非 `free` 列。

## 常用参数

| 参数   | 说明         |
| ------ | ------------ |
| `-h`   | 人类可读格式 |
| `-w`   | 加宽显示     |
| `-t`   | 显示总量     |
| `-s N` | 每秒刷新     |

## 示例

```bash
free -h                      # 人类可读显示内存使用
free -ht                     # 显示总量行
free -hw                     # 加宽显示
free -s 2                    # 每 2 秒刷新
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-free.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 3：系统资源监控](../学习路线与课程/week4-day3-系统资源监控.md) | [Day 5：本周串联实操](../学习路线与课程/week4-day5-本周串联实操.md)

## 🔗 相关文档

[系统管理-uname](系统管理-uname.md) | [系统管理-lscpu](系统管理-lscpu.md) | [swapon-swapoff](../磁盘与存储管理/磁盘与存储管理-swapon-swapoff.md)
