---
title: 文件查看与文本处理-awk
description: awk
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-awk` 📄 — 文本处理语言

## 作用

awk 是强大的文本分析工具，将行按分隔符拆分为字段（`$1`、`$2`...），支持条件判断、循环、数学运算和自定义格式输出。

## 语法

```
awk [选项] '模式 {动作}' 文件
```

## 用法

awk 以记录（行）和字段（列）的方式处理文本。`-F:` 指定冒号为字段分隔符。内置变量：`$0` 整行、`$1`~`$N` 各字段、`NR` 行号、`NF` 字段数。`BEGIN` 在处理前执行，`END` 在处理后执行。常用于格式化报表、提取日志特定列、数据统计。

## 常用参数

| 参数         | 说明           |
| ------------ | -------------- |
| `-F 分隔符`  | 指定字段分隔符 |
| `-v VAR=VAL` | 设置变量       |
| `-f`         | 指定脚本文件   |
| `$0`         | 整行内容       |
| `$1`~`$N`    | 第 N 个字段    |
| `NR`         | 当前行号       |
| `NF`         | 当前行字段数   |
| `BEGIN {}`   | 首次执行前运行 |
| `END {}`     | 最后执行后运行 |

## 示例

```bash
awk '{print $1, $3}' FILE.TXT           # 打印每行第 1、3 列
awk -F: '{print $1}' /etc/passwd        # 以冒号分隔，打印用户名列
awk '{sum += $1} END {print sum}' DATA.TXT  # 统计第一列数值总和
awk '/ERROR/ {print NR, $0}' LOG.TXT    # 搜索含 ERROR 的行并显示行号
awk 'NR > 1 && NR < 10' FILE.TXT        # 打印第 2 到 9 行
awk -F, '{print NR, NF, $0}' DATA.CSV   # CSV 文件：行号、列数、整行
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-awk.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 4：awk 列处理](../学习路线与课程/week3-day4-awk列处理.md)

## 🔗 相关文档

[文件查看与文本处理-grep](文件查看与文本处理-grep.md) | [文件查看与文本处理-sed](文件查看与文本处理-sed.md) | [文件查看与文本处理-cut](文件查看与文本处理-cut.md) | [文件查看与文本处理-sort](文件查看与文本处理-sort.md)
