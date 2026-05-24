---
title: 进程管理-uptime
description: uptime
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-uptime` ⚙️ — 查看系统运行时间

## 作用

uptime 一行显示当前时间、系统运行时长、登录用户数、1/5/15 分钟平均负载。

## 语法

```
uptime [选项]
```

## 用法

uptime 输出的平均负载若持续超过 CPU 核数表明系统过载。例如 8 核 CPU 负载持续 > 8 说明有瓶颈。`-p` 显示友好格式的运行时长，`-s` 自启动时间点。

## 常用参数

| 参数 | 说明                   |
| ---- | ---------------------- |
| `-p` | 显示友好格式的运行时长 |
| `-s` | 自启动时间点           |

## 示例

```bash
uptime           # 查看运行时长和平均负载
uptime -p        # 显示友好格式的运行时长（如 up 1 hour, 15 minutes）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-uptime.html)
