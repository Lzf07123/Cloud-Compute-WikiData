---
title: shutdown
description: shutdown / reboot / halt / poweroff
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-shutdown` / `reboot` / `halt` / `poweroff` 🖥️ — 系统关机与重启

## 作用

shutdown、reboot、halt、poweroff 用于管理系统关机、重启和停止操作。shutdown 是最安全的关机方式，能通知已登录用户并正确终止进程；reboot 重启系统；halt 停止 CPU；poweroff 关闭电源。

## 语法

```
shutdown [选项] [时间] [消息]
reboot [选项]
halt [选项]
poweroff [选项]
```

## 用法

shutdown 支持定时操作：`shutdown -H now` 立即关机，`shutdown -r +5` 5 分钟后重启，`shutdown -c` 取消已计划的关机。时间格式支持 `now`、`+分钟`（如 `+10` 表示 10 分钟后）、`HH:MM` 绝对时间（如 `23:00`）。可附加消息广播给在线用户。reboot、halt、poweroff 不带参数时执行默认操作，`--force` 强制立即执行。

## 常用参数

| 参数        | 说明             |
| ----------- | ---------------- |
| `-H`        | 停机（halt）     |
| `-P`        | 关机（poweroff） |
| `-r`        | 重启（reboot）   |
| `-c`        | 取消计划关机     |
| `-k`        | 仅发送警告不关机 |
| `--force`   | 强制立即执行     |
| `--no-wall` | 关机前不发送广播 |

## 示例

```bash
shutdown -H now                 # 立即关机
shutdown -r +5                  # 5 分钟后重启
shutdown -r 23:00               # 指定时间重启
shutdown -c                     # 取消已计划的关机
reboot                          # 重启系统
reboot --force                  # 强制重启
poweroff                        # 关闭电源
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-shutdown.html)

## 🔗 相关文档

[系统管理-systemctl](系统管理-systemctl.md)
