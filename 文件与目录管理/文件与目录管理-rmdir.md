---
title: 文件与目录管理-rmdir
description: rmdir
tags:
  - linux
  - command
  - file-management
created: 2026-05-24
updated: 2026-05-24
category: 文件与目录管理
---

# `文件与目录管理-rmdir` 📁 — 删除空目录

## 作用

rmdir（remove directory）用于删除空目录，非空目录需使用 `rm -r`。`-p` 可递归删除空父目录。

## 语法

```
rmdir [选项] 目录...
```

## 用法

rmdir 只能删除空目录，若目录非空则报错。实际工作中 rmdir 不常用，多数场景直接使用 `rm -rf` 替代。`-p` 选项可自底向上删除空父目录链。`-v` 显示删除过程。

## 常用参数

| 参数                         | 说明             |
| ---------------------------- | ---------------- |
| `-p`                         | 递归删除空父目录 |
| `-v`                         | 显示详细过程     |
| `--ignore-fail-on-non-empty` | 忽略非空目录错误 |

## 示例

```bash
rmdir EMPTYDIR                 # 删除空目录
rmdir -p A/B/C                 # 递归删除空父目录（删除 C 后尝试删 B，再删 A）
rmdir DIR1 DIR2 DIR3           # 批量删除多个空目录
rmdir -v TEMPDIR               # 删除空目录并显示过程
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-rmdir.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 2：文件与目录操作](../学习路线与课程/week1-day2-文件与目录操作.md) | [Day 5：本周串联实操](../学习路线与课程/week1-day5-本周串联实操.md)

## 🔗 相关文档

[文件与目录管理-mkdir](文件与目录管理-mkdir.md) | [文件与目录管理-rm](文件与目录管理-rm.md) | [文件与目录管理-touch](文件与目录管理-touch.md) | [文件与目录管理-ls](文件与目录管理-ls.md)
