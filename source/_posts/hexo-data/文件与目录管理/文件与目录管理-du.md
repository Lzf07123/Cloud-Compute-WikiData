---
title: 文件与目录管理-du
description: du
tags:
  - linux
  - command
  - file-management
created: 2026-05-24
updated: 2026-05-24
category: 文件与目录管理
---

# `文件与目录管理-du` 📁 — 统计文件/目录的磁盘使用量

## 作用

du（disk usage）递归统计文件或目录占用的磁盘空间，用于查找大文件和大目录。

## 语法

```
du [选项] [路径]
```

## 用法

du 递归计算每个子目录的大小。`-h` 人类可读格式，`-s` 汇总总计，`-d N` 限制递归深度。查找大文件时配合 `sort -h` 使用：`du -sh | sort -h`。`--exclude` 排除指定模式。`-t 阈值` 只显示超过指定大小的目录。

## 常用参数

| 参数             | 说明               |
| ---------------- | ------------------ |
| `-h`             | 人类可读格式       |
| `-s`             | 汇总总计           |
| `-d N`           | 限制递归深度       |
| `-c`             | 汇总总计           |
| `--exclude=模式` | 排除匹配的文件     |
| `-t 大小`        | 只显示超过指定大小 |
| `-a`             | 显示所有文件       |
| `--time`         | 显示最后修改时间   |

## 示例

```bash
du -sh                         # 统计当前目录各子目录大小
du -sh --exclude="*.LOG"      # 排除 .log 文件后统计
du -d 1                        # 只统计第一层目录
du -sh /home                   # 统计 /home 目录
du -sh | sort -h               # 排序找出最大目录
du -sh --time                  # 显示最后修改时间
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-du.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week4-day3-系统资源监控 "Day 3：系统资源监控" %} | {% post_link 学习路线与课程/week4-day5-本周串联实操 "Day 5：本周串联实操" %}
