---
title: 文件查看与文本处理-cat
description: cat
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-cat` 📄 — 查看文件内容

## 作用

cat（concatenate）用于查看小文件内容、合并多个文件、创建文件，是文件查看最基础的工具。

## 语法

```
cat [选项] 文件...
```

## 用法

cat 将文件内容输出到标准输出。`-n` 显示行号，`-b` 只给非空行编号，`-s` 压缩连续空行为一行，`-E` 在行尾显示 `$`。查看大文件建议用 less。合并文件：`cat FILE1 FILE2 > MERGED`。

## 常用参数

| 参数 | 说明            |
| ---- | --------------- |
| `-n` | 显示行号        |
| `-b` | 非空行编号      |
| `-s` | 压缩连续空行    |
| `-E` | 行尾显示 `$`    |
| `-T` | TAB 显示为 `^I` |
| `-v` | 显示非打印字符  |

## 示例

```bash
cat FILE.TXT                      # 查看文件内容
cat -n FILE.TXT                   # 显示行号
cat FILE1.TXT FILE2.TXT > MERGED.TXT  # 合并多个文件
cat > NEWFILE.TXT                 # 创建新文件（Ctrl+D 结束输入）
cat -bs FILE.TXT                  # 非空行编号并压缩空行
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-cat.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week1-day3-文件内容查看 "Day 3：文件内容查看" %} | {% post_link 学习路线与课程/week1-day5-本周串联实操 "Day 5：本周串联实操" %}
