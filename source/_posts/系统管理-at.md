---
title: 系统管理-at
description: at
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-at` 🖥️ — 一次性定时任务

## 作用

at 在指定时间执行一次性命令，时间格式灵活。

## 语法

```
at 时间
```

## 用法

at 适合延迟执行。时间格式：`10:00`、`now + 2 hours`、`tomorrow`。交互模式输入命令后 Ctrl+D 提交。`atq` 查看队列，`atrm N` 删除任务。

## 常用参数

| 参数      | 说明           |
| --------- | -------------- |
| `atq`     | 查看待执行队列 |
| `atrm N`  | 删除任务 N     |
| `-f 文件` | 从文件读取命令 |
| `-l`      | 列出任务       |

## 示例

```bash
at 10:00                              # 在 10:00 执行命令（交互输入）
at now + 2 hours                      # 两小时后执行
atq                                   # 查看待执行的任务队列
atrm 5                                # 删除任务 ID 为 5 的任务
echo "BACKUP.SH" | at 02:00 tomorrow  # 管道指定明天凌晨 2 点执行备份
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-at.html)
