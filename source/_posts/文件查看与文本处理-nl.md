---
title: 文件查看与文本处理-nl
description: nl
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-nl` 📄 — 带行号输出文件

## 作用

nl（number lines）输出文件并添加行号，比 cat -N 更智能，支持自定义编号格式。

## 语法

```
nl [选项] 文件
```

## 用法

nl 默认给非空行编号。`-b a` 给所有行编号（包括空行），`-b t` 只给非空行编号。`-n rn` 右对齐编号，`-n ln` 左对齐编号，`-n rz` 右对齐补零。`-w N` 行号宽度（位数），`-v N` 起始行号。`-s 分隔符` 自定义行号与内容间的分隔符。

## 常用参数

| 参数        | 说明               |
| ----------- | ------------------ |
| `-b a`      | 所有行编号         |
| `-b t`      | 非空行编号（默认） |
| `-n rn`     | 右对齐编号         |
| `-n ln`     | 左对齐编号         |
| `-n rz`     | 右对齐补零         |
| `-w N`      | 行号宽度（位数）   |
| `-v N`      | 起始行号           |
| `-s 分隔符` | 自定义行号分隔符   |

## 示例

```bash
nl FILE.TXT                   # 非空行编号输出
nl -b a FILE.TXT              # 所有行（含空行）编号
nl -n rn FILE.TXT             # 行号右对齐
nl -w 10 FILE.TXT             # 行号宽度设为 10 位
nl -v 10 FILE.TXT             # 起始行号从 10 开始
nl -s ": " FILE.TXT           # 行号与内容间用 ": " 分隔
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-nl.html)
