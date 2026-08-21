---
title: 文件查看与文本处理-xargs
description: xargs
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-xargs` 📄 — 批量执行命令

## 作用

xargs 从标准输入构建并执行命令行，将输入数据作为参数传递给指定命令，常用于批量文件操作。

## 语法

```
命令 | xargs [选项] [命令 [初始参数]]
```

## 用法

xargs 将管道输入转换为命令参数。`-n N` 每批 N 个参数，`-P N` 并行执行 N 个进程，`-I {}` 指定替换字符串（用 {} 代表输入的每个项）。经典组合：`find . -name "*.log" | xargs rm -f`。`-t` 打印执行命令（调试），`-0` 配合 find -print0 处理含空格的文件名。

## 常用参数

| 参数           | 说明              |
| -------------- | ----------------- |
| `-n N`         | 每批 N 个参数     |
| `-P N`         | 并行执行 N 个进程 |
| `-I 替换串`    | 指定替换字符串    |
| `-0`           | 以 NUL 分隔输入   |
| `-t`           | 打印执行命令      |
| `-R N`         | 最多替换次数      |
| `--max-args=N` | 每批最多参数数    |

## 示例

```bash
find . -name "*.log" | xargs rm -f      # 批量删除 .log 文件
find . -name "*.txt" | xargs grep "HELLO"   # 在多个文件中搜索
find . -type f -print0 | xargs -0 wc -l  # 安全处理含空格的文件名
echo FILE1 FILE2 FILE3 | xargs -n 1 echo # 每批执行 1 个参数
find . -name "*.py" | xargs -P 4 wc -l   # 4 进程并行统计
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-xargs.html)

## 🔗 相关文档

[文件查看与文本处理-grep](文件查看与文本处理-grep.md) | [文件查看与文本处理-sed](文件查看与文本处理-sed.md) | [文件查看与文本处理-awk](文件查看与文本处理-awk.md) | [文件查看与文本处理-cut](文件查看与文本处理-cut.md)
