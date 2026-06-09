---
title: 文件查看与文本处理-split
description: split
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-split` 📄 — 分割大文件

## 作用

split 将大文件按行数或大小分割成多个小文件，适合拆分日志或大数据集。

## 语法

```
split [选项] [输入 [前缀]]
```

## 用法

split 按行数 `-l N` 或大小 `-b 100M` 分割文件。`-d` 使用数字后缀（默认字母），`-a N` 后缀长度。`-e` 在空行处分割。输出文件默认命名 `XAA`、`XAB`...，可指定前缀。

## 常用参数

| 参数        | 说明                       |
| ----------- | -------------------------- |
| `-l N`      | 每个文件 N 行              |
| `-b 大小`   | 每个文件 N 字节（如 100M） |
| `-d`        | 使用数字后缀               |
| `-a N`      | 后缀长度                   |
| `-a N`      | 字母后缀长度               |
| `--verbose` | 显示分割过程               |

## 示例

```bash
split -l 1000 BIGFILE.LOG      # 每 1000 行分割为一个文件
split -b 50M DATA.DUMP         # 每 50MB 分割为一个文件
split -d -a 3 -l 500 FILE.TXT LOG_  # 数字后缀 3 位，前缀为 LOG_
split -a 2 -b 10M FILE.TXT     # 字母后缀 2 位
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-split.html)
