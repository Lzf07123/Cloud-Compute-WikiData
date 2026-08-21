---
title: 磁盘与存储管理-smartctl
description: smartctl
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-smartctl` 💾 — 磁盘健康检测

## 作用

smartctl（SMART control）用于查看和监控支持 S.M.A.R.T.（Self-Monitoring, Analysis and Reporting Technology）技术的磁盘健康状态，包括温度、坏道数、通电时间、读写错误率等。是预测磁盘故障、保障数据安全的重要工具。

## 语法

```
smartctl [选项] 设备
```

## 用法

smartctl 需要 root 权限。`-A` 显示所有 SMART 信息；`-H` 显示磁盘健康状态概要；`-i` 显示设备信息；`-l 类型` 查看指定日志（如 error/selftest）；`-t` 离线测试（`-t SHORT` 短测试，`-t LONG` 长测试）；`-c` 查看当前测试进度；`-s ON` 启用 SMART。组合 `-A -i` 可查看完整报告。重点关注 `REALLOCATED_SECTOR_COUNT`（重映射扇区数）、`PENDING_SECTOR`（待处理扇区）等关键属性。

## 常用参数

| 参数       | 说明                           |
| ---------- | ------------------------------ |
| `-A`       | 所有 SMART 信息                |
| `-H`       | 健康状态                       |
| `-i`       | 设备信息                       |
| `-l 类型`  | 查看指定日志（error/selftest） |
| `-t SHORT` | 短测试                         |
| `-t LONG`  | 长测试                         |
| `-c`       | 测试进度                       |
| `-s ON`    | 启用 SMART                     |

## 示例

```bash
smartctl -A /dev/sda                # 查看完整 SMART 信息
smartctl -H /dev/sda                # 查看磁盘健康状态
smartctl -i /dev/sda                # 查看设备基本信息
smartctl -t LONG /dev/sda           # 执行长测试（检测坏道）
smartctl -c /dev/sda                # 查看测试进度
smartctl -l error /dev/sda          # 查看错误日志
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-smartctl.html)

## 🔗 相关文档

[磁盘与存储管理-fdisk](磁盘与存储管理-fdisk.md) | [磁盘与存储管理-mount](磁盘与存储管理-mount.md)
