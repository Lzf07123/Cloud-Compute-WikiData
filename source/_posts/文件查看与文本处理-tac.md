---
title: 文件查看与文本处理-tac
description: tac
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-tac` 📄 — 反向输出文件

## 作用

tac（cat 的反向拼写）从最后一行开始反向输出文件内容，行顺序完全颠倒，适合从日志末尾开始查看。

## 语法

```
tac [选项] 文件
```

## 用法

tac 与 cat 输出顺序相反，最后一行在第一行输出。`-b` 在每个记录前加分隔符，`-r` 使用正则分隔符，`-s` 指定记录分隔符（默认换行符）。可配合管道：`tac FILE.TXT | head` 查看文件末尾几行。

## 常用参数

| 参数        | 说明                 |
| ----------- | -------------------- |
| `-b`        | 在每个记录前加分隔符 |
| `-r`        | 使用正则分隔符       |
| `-s 分隔符` | 指定记录分隔符       |

## 示例

```bash
tac FILE.TXT                  # 反向输出文件（末行在前）
tac -s '.' FILE.TXT           # 以句号为分隔符反转段落
tac FILE.TXT | head           # 查看文件末尾几行（配合 head）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-tac.html)
