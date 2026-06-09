---
title: 文件与目录管理-touch
description: touch
tags:
  - linux
  - command
  - file-management
created: 2026-05-24
updated: 2026-05-24
category: 文件与目录管理
---

# `文件与目录管理-touch` 📁 — 创建文件或更新时间戳

## 作用

touch 用于创建空白新文件（若文件不存在），或更新已有文件的访问时间和修改时间为当前时间。

## 语法

```
touch [选项] 文件...
```

## 用法

touch 最常用于快速创建空文件，如占位文件、锁文件、日志文件。也用于触发构建系统重新编译（更新修改时间戳）。`-a` 只修改访问时间，`-m` 只修改内容时间，`-t` 指定具体时间而非当前时间，`-r 参考文件` 将时间设为与参考文件相同。

## 常用参数

| 参数          | 说明                     |
| ------------- | ------------------------ |
| `-a`          | 只修改访问时间           |
| `-m`          | 只修改修改时间           |
| `-c`          | 不创建文件（仅更新时间） |
| `-t 时间`     | 使用指定时间             |
| `-r 参考文件` | 使用参考文件的时间       |
| `-d 字符串`   | 解析字符串为时间         |

## 示例

```bash
touch FILE.TXT                      # 创建空文件（已存在则更新时间戳）
touch FILE1 FILE2 FILE3             # 批量创建多个文件
touch -a FILE.TXT                   # 仅修改访问时间
touch -d "2026-01-01 12:00" FILE.TXT  # 修改为指定时间
touch -r REFERENCE.TXT TARGET.TXT   # 用参考文件的时间戳
touch /var/lock/APP.LOCK            # 创建锁文件占位
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-touch.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week1-day2-文件与目录操作 "Day 2：文件与目录操作" %} | {% post_link 学习路线与课程/week1-day5-本周串联实操 "Day 5：本周串联实操" %}
