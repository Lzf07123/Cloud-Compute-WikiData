---
title: 文件查看与文本处理-uniq
description: uniq
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-uniq` 📄 — 去重或统计重复行

## 作用

uniq 报告或删除文件中重复的行。**注意**：只处理连续重复行，通常需先 sort 排序。

## 语法

```
uniq [选项] [输入文件 [输出文件]]
```

## 用法

uniq 只能去除相邻重复行，因此常与 sort 配合使用：`sort FILE.TXT | uniq`。`-c` 统计重复次数，`-d` 只显示重复行，`-u` 只显示唯一行，`-i` 忽略大小写。`-w N` 仅比较前 N 个字符。

## 常用参数

| 参数             | 说明              |
| ---------------- | ----------------- |
| `-c`             | 统计每行出现次数  |
| `-d`             | 只显示重复行      |
| `-u`             | 只显示唯一行      |
| `-i`             | 忽略大小写        |
| `-w N`           | 只比较前 N 个字符 |
| `--all-repeated` | 显示所有重复行    |

## 示例

```bash
sort FILE.TXT | uniq            # 排序后去重
sort FILE.TXT | uniq -c         # 统计每行出现次数
sort FILE.TXT | uniq -d         # 只显示重复行
sort FILE.TXT | uniq -u         # 只显示唯一行
sort FILE.TXT | uniq -i         # 忽略大小写去重
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-uniq.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week1-day4-文本统计与处理 "Day 4：文本统计与处理" %} | {% post_link 学习路线与课程/week1-day5-本周串联实操 "Day 5：本周串联实操" %}
