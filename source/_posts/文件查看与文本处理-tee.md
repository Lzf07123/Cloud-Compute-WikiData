---
title: 文件查看与文本处理-tee
description: tee
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-tee` 📄 — 双向输出

## 作用

tee 同时将 stdin 数据输出到 stdout 和一个或多个文件，常用于在管道处理中同时查看和保存中间结果。

## 语法

```
命令 | tee [选项] 文件
```

## 用法

tee 复制 stdin 到 stdout 的同时写入文件。`-a` 追加而非覆盖。适合调试管道：`CMD1 | tee STEP1.LOG | CMD2 | tee STEP2.LOG | CMD3`。也可用于同时查看和保存命令输出。

## 常用参数

| 参数 | 说明                 |
| ---- | -------------------- |
| `-a` | 追加到文件（不覆盖） |
| `-i` | 忽略中断信号         |

## 示例

```bash
CMD | tee OUTPUT.LOG           # 同时输出到屏幕和文件
CMD | tee -a APPEND.LOG        # 追加到文件而非覆盖
CMD | tee FILE1.LOG FILE2.LOG  # 同时写入多个文件
CMD1 | tee -a DEBUG.LOG | CMD2 # 调试管道，保存中间结果
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-tee.html)
