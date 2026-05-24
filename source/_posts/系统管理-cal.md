---
title: 系统管理-cal
description: cal
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-cal` 🖥️ — 显示日历

## 作用

cal（calendar）在终端输出月历或年历。

## 语法

```
cal [选项] [年份]
```

## 用法

cal 默认显示当前月。`2026` 全年日历，`-3` 显示前/当/后三月，`-j` 儒略历，`-y` 当年日历。终端快速查日期的工具。

## 常用参数

| 参数 | 说明         |
| ---- | ------------ |
| `-3` | 前/当/后三月 |
| `-y` | 全年日历     |
| `-j` | 儒略历       |
| `-m` | 周一为第一天 |

## 示例

```bash
cal                                   # 显示当前月日历
cal 2026                              # 显示 2026 年全年日历
cal -3                                # 查看前/当/后三个月
cal -y                                # 显示当年全年日历
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-cal.html)
