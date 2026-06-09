---
title: 磁盘与存储管理-eject
description: eject
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-eject` 💾 — 弹出可移动介质

## 作用

eject 弹出光驱、软盘、磁带机等可移动介质设备。对于 CD/DVD 驱动器执行物理弹出托盘，对于 USB 设备执行安全卸载。在现代 Linux 中也可用于弹出或卸载可移动媒体。

## 语法

```
eject [选项] [设备或挂载点]
```

## 用法

eject 默认弹出 `/dev/cdrom` 或 `/dev/sr0`。`-T` 关/开仓门切换（CD 托盘）；`-r` 弹出 CD-ROM；`-f` 弹出软驱；`-d` 显示默认设备；`-n` 显示操作但不执行（--noop）；`-a ON/OFF` 启用/禁用自动弹出。指定设备路径如 `eject /dev/sdb1` 可卸载 USB 设备；`--traytoggle` 切换光驱托盘状态。

## 常用参数

| 参数        | 说明             |
| ----------- | ---------------- |
| `-T`        | 开/关仓门切换    |
| `-r`        | 弹出 CD-ROM      |
| `-f`        | 弹出软驱         |
| `-d`        | 显示默认设备     |
| `-n`        | 显示操作但不执行 |
| `-a ON/OFF` | 自动弹出         |

## 示例

```bash
eject                               # 弹出默认光驱
eject -T                            # 切换光驱托盘开关
eject /dev/sr0                      # 弹出指定光驱设备
eject /dev/sdb1                     # 安全卸载 USB 设备
eject -n                            # 显示操作但不执行（模拟）
eject -d                            # 显示默认弹出设备
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-eject.html)

## 🔗 相关文档

{% post_link 磁盘与存储管理/磁盘与存储管理-mount %}
