---
title: zgrep-zcat-zless
description: zcat / zgrep / zless
tags:
  - linux
  - command
  - archive
created: 2026-05-24
updated: 2026-05-24
category: 压缩与归档
---

# `压缩与归档-zgrep-zcat-zless` / `zgrep` / `zless` 📦 — 查看压缩文件

## 作用

zcat、zgrep、zless 是 gzip 压缩文件的查看工具族，在不解压的情况下直接读取 `.gz` 文件内容，节省磁盘空间且方便快速搜索。相当于 `gzip -dc`、`grep`、`less` 的压缩文件封装。

## zcat

```
zcat [选项] 文件.gz
```

将压缩文件内容解压后输出到标准输出（相当于 `gunzip -c`）。用于查看日志归档、配置文件备份等。支持 `less` 配合分页查看：`zcat FILE.GZ | less`。

## zgrep

```
zgrep [选项] 模式 文件.gz
```

在压缩文件中直接搜索文本模式。支持所有 `grep` 选项（`-i`、`-n`、`-v` 等）。`zgrep -i ERROR /VAR/LOG/SYSLOG*.GZ` 搜索压缩日志中的错误。

## zless

```
zless 文件.gz
```

分页查看压缩文件内容（相当于 `zcat FILE.GZ | less`）。支持 `less` 的所有操作（上下翻页、搜索、跳转等）。比先解压再查看更高效。

## 示例

```bash
zcat FILE.TXT.GZ                   # 查看压缩文件内容（不解压）
zcat FILE.GZ | less                # 分页查看压缩文件
zgrep -i ERROR LOG.GZ              # 在压缩文件中搜索（忽略大小写）
zgrep -n "FAILED" AUTH.LOG.GZ     # 搜索并显示匹配行号
zless FILE.GZ                      # 分页浏览压缩文件
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-zcat.html)
