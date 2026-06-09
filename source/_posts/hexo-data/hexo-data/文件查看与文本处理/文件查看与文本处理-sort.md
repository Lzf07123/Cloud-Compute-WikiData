---
title: 文件查看与文本处理-sort
description: sort
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-sort` 📄 — 文本排序

## 作用

sort 对文本行按字典序或数值排序，支持按列排序、去重、逆序等，是数据处理的基础工具。

## 语法

```
sort [选项] 文件
```

## 用法

sort 默认按字典序升序排列。`-n` 数值排序（识别数字大小），`-r` 逆序，`-k N` 按第 N 列排序，`-u` 去重（等价于 `sort | uniq`），`-t 分隔符` 指定列分隔符。常与 cut、uniq 组成管道链处理表格数据。

## 常用参数

| 参数        | 说明                      |
| ----------- | ------------------------- |
| `-n`        | 数值排序                  |
| `-r`        | 逆序排序                  |
| `-u`        | 去重                      |
| `-k N`      | 按第 N 列排序             |
| `-t 分隔符` | 指定列分隔符              |
| `-h`        | 人类可读数值排序（K/M/G） |
| `-f`        | 忽略大小写                |
| `-c`        | 检查是否已排序            |

## 示例

```bash
sort FILE.TXT                  # 字典序升序排列
sort -n FILE.TXT               # 按数值大小排序
sort -n -r DATA.TXT            # 按数值逆序排序
sort -k 2 -t: /etc/passwd     # 以冒号分隔，按第 2 列排序
sort -u FILE.TXT               # 排序并去重
du -sh | sort -h               # 磁盘使用按人类可读大小排序
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-sort.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week1-day4-文本统计与处理 "Day 4：文本统计与处理" %} | {% post_link 学习路线与课程/week1-day5-本周串联实操 "Day 5：本周串联实操" %}
