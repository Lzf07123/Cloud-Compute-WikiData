---
title: 压缩与归档-tar
description: tar
tags:
  - linux
  - command
  - archive
created: 2026-05-24
updated: 2026-05-24
category: 压缩与归档
---

# `压缩与归档-tar` 📦 — 归档与压缩

## 作用

tar（tape archive）是 Linux 中最常用的归档工具，将多个文件或目录打包成一个归档文件，常结合 gzip/bzip2/xz 实现打包并压缩。是备份、软件分发、迁移数据的标准工具。

## 语法

```
tar [选项] [归档文件] [源文件...]
```

## 用法

tar 的选项分为操作模式和修饰符。核心操作：`-c` 创建归档；`-x` 解压归档；`-t` 查看归档内容。`-z` 通过 gzip 压缩/解压（`.tar.gz`）；`-j` 通过 bzip2（`.tar.bz2`）；`-J` 通过 xz（`.tar.xz`）；`--lzma` 通过 lzma（`.tar.lzma`）。`-v` 显示处理过程；`-f 文件` 指定归档文件名；`-C 目录` 指定解压目标目录；`--exclude=模式` 排除文件；`-u` 更新归档。

## 常用参数

| 参数             | 说明                     |
| ---------------- | ------------------------ |
| `-c`             | 创建归档                 |
| `-x`             | 解压归档                 |
| `-t`             | 查看归档内容             |
| `-z`             | gzip 压缩                |
| `-j`             | bzip2 压缩               |
| `-J`             | xz 压缩                  |
| `--lzma`         | lzma 压缩（`.tar.lzma`） |
| `-v`             | 显示处理过程             |
| `-f 文件`        | 指定文件名               |
| `-C 目录`        | 解压到目录               |
| `--exclude=模式` | 排除文件                 |

## 示例

```bash
tar -cvf ARCHIVE.TAR DIR/         # 打包目录为 .tar
tar -czvf ARCHIVE.TAR.GZ DIR/     # 打包并用 gzip 压缩
tar -xjf ARCHIVE.TAR.BZ2          # 解压 bzip2 归档到当前目录
tar -tvf ARCHIVE.TAR              # 查看归档内容列表
tar -czvf BACKUP.TAR.GZ --exclude="*.LOG" /VAR/LOG  # 打包排除 .LOG 文件
tar -xzf ARCHIVE.TAR.GZ -C /TARGET   # 解压 gzip 归档到指定目录
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-tar.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week3-day5-打包与压缩 "Day 5：打包与压缩" %}
