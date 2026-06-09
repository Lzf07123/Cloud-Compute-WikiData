---
title: 文件与目录管理-rm
description: rm
tags:
  - linux
  - command
  - file-management
created: 2026-05-24
updated: 2026-05-24
category: 文件与目录管理
---

# `文件与目录管理-rm` 📁 — 删除文件或目录

## 作用

rm（remove）用于永久删除文件或目录，操作不可逆（不经过回收站）。默认不能删除目录，需加 `-R` 递归删除。

## 语法

```
rm [选项] 文件...
```

## 用法

rm 直接删除数据块，无法通过常规手段恢复。`-f` 强制删除忽略警告，`-R` 递归删除目录及内容，`-i` 交互式逐文件确认。**极度危险操作**：`rm -rf /` 会删除整个系统（现代 Linux 默认启用 `--preserve-root` 保护）。建议养成交互删除习惯或先 `ls` 确认。

## 常用参数

| 参数              | 说明                       |
| ----------------- | -------------------------- |
| `-f`              | 强制删除，忽略不存在的文件 |
| `-i`              | 交互式逐一确认             |
| `-R`              | 递归删除目录               |
| `-d`              | 删除空目录                 |
| `-v`              | 显示详细过程               |
| `--preserve-root` | 禁止删除 `/`（默认启用）   |

## 示例

```bash
rm FILE.TXT              # 删除文件（默认交互确认）
rm -f FILE.TXT           # 强制删除不提示
rm -i FILE.TXT           # 逐文件确认删除
rm -rf DIR/              # 递归强制删除目录
rm -rfv DIR/             # 递归强制删除并显示过程
rm -d EMPTYDIR/          # 删除空目录
rm -rf DIR/*             # 删除目录下所有内容
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-rm.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week1-day2-文件与目录操作 "Day 2：文件与目录操作" %} | {% post_link 学习路线与课程/week1-day5-本周串联实操 "Day 5：本周串联实操" %}
