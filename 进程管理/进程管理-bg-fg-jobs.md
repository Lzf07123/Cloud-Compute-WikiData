---
title: bg-fg-jobs
description: bg fg jobs
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-bg-fg-jobs` ⚙️ / `fg` / `jobs` — 作业管理

## 作用

bg/fg/jobs 管理 Shell 前后台作业，支持暂停、后台运行、前台恢复等操作。

## bg

将暂停（Ctrl+Z 挂起）的作业在后台继续运行。

```bash
bg               # 将当前暂停作业放到后台
bg %1            # 后台运行作业号 1
```

## fg

将后台作业恢复到前台运行。

```bash
fg               # 将最近后台作业调到前台
fg %2            # 将作业号 2 调到前台
```

## jobs

列出当前 Shell 的后台作业列表及状态。

```bash
jobs             # 列出所有后台作业
jobs -l          # 显示作业对应的 PID
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-bg.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 4：前后台控制与重定向管道](../学习路线与课程/week2-day4-前后台与重定向管道.md) | [Day 5：本周串联实操](../学习路线与课程/week2-day5-本周串联实操.md)

## 🔗 相关文档

[进程管理-nohup](进程管理-nohup.md) | [进程管理-time](进程管理-time.md) | [进程管理-ps](进程管理-ps.md) | [进程管理-kill](进程管理-kill.md)
