---
title: 压缩与归档-gzip
description: gzip
tags:
  - linux
  - command
  - archive
created: 2026-05-24
updated: 2026-05-24
category: 压缩与归档
---

# `压缩与归档-gzip` 📦 — 文件压缩与解压

## 作用

gzip（GNU zip）是 Linux 最广泛使用的文件压缩工具，基于 DEFLATE 算法。压缩后文件以 `.gz` 为扩展名，常与 `tar` 组合使用。压缩率高、速度快，是日常文件压缩的首选工具。

## 语法

```
gzip [选项] 文件名
gunzip [选项] 文件名.gz
```

## 用法

gzip 将指定文件压缩为 `.gz` 格式并替换原文件。`-d` 解压（等价于 `gunzip`）；`-r` 递归压缩/解压；`-c` 输出到标准输出（保留原文件）；`-n` 不保存原始文件名和时间戳；`-l` 显示压缩比；`-k` 保留原文件（默认删除原文件）；`-1~-9` 压缩级别（`-1` 最快，`-6` 默认，`-9` 最高压缩比）；`-t` 测试文件完整性。`gunzip FILE.GZ` 解压文件。

## 常用参数

| 参数    | 说明                     |
| ------- | ------------------------ |
| `-d`    | 解压                     |
| `-r`    | 递归                     |
| `-c`    | 输出到标准输出           |
| `-n`    | 不保存原始文件名和时间戳 |
| `-l`    | 显示压缩比               |
| `-k`    | 保留原文件               |
| `-1~-9` | 压缩级别                 |
| `-t`    | 测试完整性               |

## 示例

```bash
gzip FILE.TXT                      # 压缩文件（原文件被替换）
gzip -9 FILE.TXT                   # 最高压缩比压缩
gzip -d FILE.TXT.GZ                # 解压 .gz 文件
gzip -r -k /HOME/USER             # 递归压缩目录并保留原文件
gzip -c FILE.TXT > FILE.TXT.GZ     # 压缩到标准输出（保留原文件）
gzip -t FILE.TXT.GZ                # 测试压缩文件完整性
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-gzip.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week3-day5-打包与压缩 "Day 5：打包与压缩" %}
