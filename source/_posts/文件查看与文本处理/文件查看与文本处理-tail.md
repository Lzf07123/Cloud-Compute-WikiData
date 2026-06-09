---
title: 文件查看与文本处理-tail
description: tail
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-tail` 📄 — 显示文件末尾

## 作用

tail 显示文件末尾 N 行（默认 10 行），`-F` 实时追踪文件新增内容，是监控日志文件的核心工具。

## 语法

```
tail [选项] 文件
```

## 用法

tail 默认输出后 10 行。`-n 20` 指定行数，`-F` 实时追踪新增内容（日志监控），`--pid=PID` 在进程退出后停止追踪。`-c 100` 显示后 100 字节。调试日志时 `tail -F /var/log/APP.LOG` 持续查看新输出。

## 常用参数

| 参数        | 说明             |
| ----------- | ---------------- |
| `-n N`      | 显示后 N 行      |
| `-F`        | 实时追踪新增内容 |
| `--pid=PID` | 进程退出后停止   |
| `-c N`      | 显示后 N 字节    |
| `-q`        | 不显示文件名标题 |
| `-v`        | 显示详细信息     |

## 示例

```bash
tail FILE.TXT                    # 显示文件末尾 10 行
tail -n 50 FILE.TXT              # 显示末尾 50 行
tail -F /var/log/syslog          # 实时跟踪日志更新
tail -n 100 -F APP.LOG           # 先显示末 100 行，再持续跟踪
tail -c 200 FILE.TXT             # 显示末尾 200 字节
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-tail.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week1-day3-文件内容查看 "Day 3：文件内容查看" %} | {% post_link 学习路线与课程/week1-day5-本周串联实操 "Day 5：本周串联实操" %}
