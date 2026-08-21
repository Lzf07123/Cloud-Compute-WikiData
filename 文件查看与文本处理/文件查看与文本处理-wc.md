---
title: 文件查看与文本处理-wc
description: wc
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-wc` 📄 — 统计行/词/字符数

## 作用

wc（word count）统计文件的行数、单词数、字符数或字节数，是代码行数统计的常用工具。

## 语法

```
wc [选项] 文件
```

## 用法

wc 默认输出行数、单词数、字节数。`-l` 只统计行数，`-w` 只统计单词数，`-c` 只统计字节数，`-m` 统计字符数（含多字节字符）。配合管道使用：`cat FILE.TXT | wc -l`。

## 常用参数

| 参数 | 说明               |
| ---- | ------------------ |
| `-l` | 行数               |
| `-w` | 单词数             |
| `-c` | 字节数             |
| `-m` | 字符数（含多字节） |
| `-L` | 最长行的字符数     |

## 示例

```bash
wc FILE.TXT                    # 统计行、单词、字节数
wc -l FILE.TXT                 # 只统计行数
wc -w FILE.TXT                 # 只统计单词数
cat FILE.TXT | wc -l           # 管道统计行数
wc *.py                        # 批量统计多个文件
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-wc.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 4：文本统计与处理](../学习路线与课程/week1-day4-文本统计与处理.md) | [Day 5：本周串联实操](../学习路线与课程/week1-day5-本周串联实操.md)

## 🔗 相关文档

[文件查看与文本处理-sort](文件查看与文本处理-sort.md) | [文件查看与文本处理-uniq](文件查看与文本处理-uniq.md) | [文件查看与文本处理-head](文件查看与文本处理-head.md) | [文件查看与文本处理-tail](文件查看与文本处理-tail.md)
