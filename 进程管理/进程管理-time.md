---
title: 进程管理-time
description: time
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-time` ⚙️ — 测量命令执行时间

## 作用

time 统计命令执行耗时，输出 real（实际）、user（用户 CPU）、sys（系统 CPU）三段时间。

## 语法

```
time 命令
```

## 用法

time 在命令执行完毕后输出统计：`REAL` 从开始到结束的总耗时，`USER` 用户态 CPU 时间，`SYS` 内核态 CPU 时间。`-p` POSIX 格式。`-f 格式` 自定义输出格式。脚本性能分析和基准测试的基础工具。

## 常用参数

| 参数      | 说明           |
| --------- | -------------- |
| `-p`      | POSIX 格式输出 |
| `-f 格式` | 自定义输出格式 |
| `-a`      | 追加到文件     |
| `-o 文件` | 输出到文件     |

## 示例

```bash
time sleep 2                          # 测量 sleep 2 秒的实际耗时
time ls -la                           # 测量 ls 命令的执行时间
time -p make                          # POSIX 格式输出编译耗时
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-time.html)

## 🔗 相关文档

[进程管理-bg-fg-jobs](进程管理-bg-fg-jobs.md) | [进程管理-nohup](进程管理-nohup.md) | [进程管理-ps](进程管理-ps.md) | [进程管理-uptime](进程管理-uptime.md)
