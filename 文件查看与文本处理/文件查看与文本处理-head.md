---
title: 文件查看与文本处理-head
description: head
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-head` 📄 — 显示文件开头

## 作用

head 显示文件前 N 行内容（默认 10 行），适合快速查看文件头部、日志开头、CSV 列名等。

## 语法

```
head [选项] 文件
```

## 用法

head 默认输出前 10 行。`-n 20` 指定行数，`-c 100` 显示前 100 个字节，`-q` 不显示文件名标题。支持同时查看多个文件，此时会显示文件名标题。

## 常用参数

| 参数   | 说明             |
| ------ | ---------------- |
| `-n N` | 显示前 N 行      |
| `-c N` | 显示前 N 字节    |
| `-q`   | 不显示文件名标题 |
| `-v`   | 显示详细信息     |

## 示例

```bash
head FILE.TXT                    # 显示前 10 行
head -n 20 FILE.TXT              # 显示前 20 行
head -c 100 FILE.TXT             # 显示前 100 字节
head -n 5 FILE1 FILE2            # 同时查看多个文件头部
head -q -n 1 *.csv               # 不显示文件名，只看首行列标题
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-head.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 3：文件内容查看](../学习路线与课程/week1-day3-文件内容查看.md) | [Day 5：本周串联实操](../学习路线与课程/week1-day5-本周串联实操.md)

## 🔗 相关文档

[文件查看与文本处理-tail](文件查看与文本处理-tail.md) | [文件查看与文本处理-less](文件查看与文本处理-less.md) | [文件查看与文本处理-more](文件查看与文本处理-more.md) | [文件查看与文本处理-cat](文件查看与文本处理-cat.md)
