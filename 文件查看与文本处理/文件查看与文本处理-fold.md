---
title: 文件查看与文本处理-fold
description: fold
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-fold` 📄 — 折行显示

## 作用

fold 在指定宽度处换行，默认 80 列。适合格式化窄屏查看或处理固定宽度文本。

## 语法

```
fold [选项] 文件
```

## 用法

fold 将长行在指定列宽处截断换行。`-w N` 指定宽度（默认 80），`-s` 在空格处断行（避免单词截断）。默认在列边界处硬截断可能截断单词，`-s` 更友好。

## 常用参数

| 参数   | 说明                |
| ------ | ------------------- |
| `-w N` | 指定宽度（默认 80） |
| `-s`   | 在空格处断行        |
| `-b`   | 按字节而非列宽度量  |

## 示例

```bash
fold FILE.TXT                  # 默认 80 列换行
fold -w 40 FILE.TXT            # 40 列处换行
fold -w 50 -s FILE.TXT         # 50 列处空格断行（不截断单词）
fold -w 120 FILE.TXT           # 宽屏显示，120 列换行
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-fold.html)

## 🔗 相关文档

[文件查看与文本处理-rev](文件查看与文本处理-rev.md) | [文件查看与文本处理-column](文件查看与文本处理-column.md) | [文件查看与文本处理-tr](文件查看与文本处理-tr.md) | [文件查看与文本处理-cut](文件查看与文本处理-cut.md)
