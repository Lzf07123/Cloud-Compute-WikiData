---
title: 权限与用户管理-chmod
description: chmod
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-chmod` 🔐 — 修改文件权限

## 作用

chmod（change mode）修改文件或目录的读/写/执行权限，支持数字模式（如 755）和符号模式（如 U+RWX）。

## 语法

```
chmod [选项] 权限 文件
```

## 用法

chmod 支持数字模式：`rwx` 分别对应 4/2/1，如 `755` = u=rwx,g=rx,o=rx。符号模式：`u+rwx` 所有者加执行，`g-w` 组移除写，`a+X` 所有人加执行。`-R` 递归修改目录。`chmod +X` 为目录添加执行权限；对已有至少一个执行位的文件也添加。

## 常用参数

| 参数                | 说明             |
| ------------------- | ---------------- |
| `-R`                | 递归修改         |
| `-v`                | 显示详细过程     |
| `-c`                | 有变更时才报告   |
| `--reference=RFILE` | 参考文件权限     |
| `755`               | u=rwx,g=rx,o=rx  |
| `644`               | u=rw,g=r,o=r     |
| `600`               | u=rw,g=---,o=--- |

## 示例

```bash
chmod 755 SCRIPT.SH           # 所有者可读写执行，其他人可读执行
chmod 644 FILE.TXT            # 所有者可读写，其他人只读
chmod +X SCRIPT.SH            # 添加可执行权限
chmod -R 755 DIR/             # 递归设置目录权限
chmod u+rwx,g+r,o+r FILE      # 符号模式：所有者全权限，组和其他只读
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-chmod.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 1：用户身份与基本权限](../学习路线与课程/week2-day1-用户身份与基本权限.md) | [Day 5：本周串联实操](../学习路线与课程/week2-day5-本周串联实操.md)

## 🔗 相关文档

[权限与用户管理-chown](权限与用户管理-chown.md) | [权限与用户管理-chgrp](权限与用户管理-chgrp.md) | [权限与用户管理-umask](权限与用户管理-umask.md) | [权限与用户管理-usermod](权限与用户管理-usermod.md)
