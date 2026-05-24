---
title: 系统管理-timedatectl
description: timedatectl
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-timedatectl` 🖥️ — 管理时间和日期

## 作用

timedatectl 是 systemd 系统的时间和日期管理命令，用于查看和设置系统时间、时区、NTP 同步等。替代传统的 `date`、`hwclock`、`tzdata` 等工具。

## 语法

```
timedatectl [选项] [操作]
```

## 用法

timedatectl 不带参数时显示当前时间、时区、NTP 同步状态等概要信息。`set-time HH:MM:SS` 设置时间；`set-timezone 时区` 设置时区；`set-ntp yes/no` 启用/禁用 NTP 自动同步；`list-timezones` 列出可用时区。常用时区如 `Asia/Shanghai`、`America/New_York`、`UTC`。

## 常用参数

| 参数                    | 说明          |
| ----------------------- | ------------- |
| `set-time 时间`         | 设置系统时间  |
| `set-timezone 时区`     | 设置时区      |
| `set-ntp yes/no`        | 启用/禁用 NTP |
| `list-timezones`        | 列出可用时区  |
| `--adjust-system-clock` | 调整硬件时钟  |
| `--no-pager`            | 不分页输出    |

## 示例

```bash
timedatectl                             # 查看当前时间/时区/NTP 状态
timedatectl set-timezone Asia/Shanghai  # 设置时区为上海
timedatectl set-ntp yes                 # 启用 NTP 自动同步
timedatectl list-timezones | grep -i asia  # 搜索亚洲时区
timedatectl set-time 14:30:00           # 设置系统时间
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-timedatectl.html)
