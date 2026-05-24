---
title: 系统管理-date
description: date
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-date` 🖥️ — 显示或设置日期时间

## 作用

date 按指定格式显示系统日期时间，root 可设置系统时间。

## 语法

```
date [选项] [+格式]
```

## 用法

date 格式化输出：`+%Y` 年、`%m` 月、`%d` 日、`%H` 时、`%M` 分、`%S` 秒。`-s 字符串` 设置时间。`-d 字符串` 解析日期。脚本中常用 `$(date +%s)` 生成时间戳。

## 常用参数

| 参数        | 说明        |
| ----------- | ----------- |
| `+%Y-%m-%d` | 年-月-日    |
| `+%H:%M:%S` | 时:分:秒    |
| `+%s`       | Unix 时间戳 |
| `-s 字符串` | 设置时间    |
| `-d 字符串` | 解析日期    |
| `-u`        | UTC 时间    |

## 示例

```bash
date                           # 显示当前日期时间
date "+%Y-%m-%d %H:%M:%S"     # 自定义格式输出
date +%s                       # 输出 Unix 时间戳
date -d "2026-05-24 12:00"    # 解析指定日期
date -u                        # 显示 UTC 时间
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-date.html)
