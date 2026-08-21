---
title: 系统管理-crontab
description: crontab
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-crontab` 🖥️ — 管理定时任务

## 作用

crontab 编辑和管理用户级 cron 定时任务，实现周期性自动执行。

## 语法

```
crontab [选项]
```

## 用法

crontab 管理 cron 表。`-e` 编辑任务，`-l` 列出任务，`-r` 清空任务。格式：`分 时 日 月 周 命令`。`*` 任意，`*/N` 每 N 单位，`N,M` 多值，`N-M` 范围。

## 常用参数

| 参数      | 说明                 |
| --------- | -------------------- |
| `-e`      | 编辑任务             |
| `-l`      | 列出任务             |
| `-r`      | 清空所有任务         |
| `-u 用户` | 管理指定用户（root） |

## 示例

```bash
crontab -e                            # 编辑当前用户的 cron 任务
crontab -l                            # 列出当前用户的定时任务
crontab -r                            # 清空所有 cron 任务
crontab -u ALICE -l                   # root 查看 alice 用户的定时任务
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-crontab.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 4：定时任务 crontab](../学习路线与课程/week4-day4-定时任务.md) | [Day 5：本周串联实操](../学习路线与课程/week4-day5-本周串联实操.md)

## 🔗 相关文档

[系统管理-at](系统管理-at.md) | [系统管理-systemctl](系统管理-systemctl.md) | [系统管理-timedatectl](系统管理-timedatectl.md)
