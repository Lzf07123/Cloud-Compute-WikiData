---
title: 压缩与归档-bzip2
description: bzip2
tags:
  - linux
  - command
  - archive
created: 2026-05-24
updated: 2026-05-24
category: 压缩与归档
---

# `压缩与归档-bzip2` 📦 — 文件压缩（高压缩比）

## 作用

bzip2 基于 Burrows-Wheeler 算法，提供比 gzip 更高的压缩比，压缩后文件以 `.bz2` 为扩展名。适合压缩较大的文本文件，压缩速度较慢但压缩率更高。配套工具 `bunzip2` 解压。

## 语法

```
bzip2 [选项] 文件名
bunzip2 [选项] 文件.bz2
```

## 用法

bzip2 压缩文件并替换原文件。`-d` 解压（等价于 `bunzip2`）；`-k` 保留原文件；`-c` 输出到标准输出；`-t` 测试完整性；`-1~-9` 压缩级别（`-1` 最快，`-9` 最高压缩比，默认 `-9`）；`-v` 显示详细信息；`-s` small 模式（降低内存使用）。常与 `tar` 搭配：`tar -cjvf ARCHIVE.TAR.BZ2 DIR/`。

## 常用参数

| 参数    | 说明                       |
| ------- | -------------------------- |
| `-d`    | 解压                       |
| `-k`    | 保留原文件                 |
| `-c`    | 输出到标准输出             |
| `-t`    | 测试完整性                 |
| `-1~-9` | 压缩级别                   |
| `-v`    | 详细信息                   |
| `-s`    | small 模式（降低内存使用） |

## 示例

```bash
bzip2 FILE.TXT                     # 压缩为 .bz2（原文件被替换）
bzip2 -9 FILE.TXT                  # 最高压缩比压缩
bunzip2 FILE.TXT.BZ2               # 解压 .bz2 文件
bzip2 -k FILE.TXT                  # 压缩并保留原文件
bzip2 -t FILE.TXT.BZ2              # 测试压缩文件完整性
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-bzip2.html)

## 🔗 相关文档

[压缩与归档-gzip](压缩与归档-gzip.md) | [压缩与归档-xz](压缩与归档-xz.md) | [压缩与归档-tar](压缩与归档-tar.md) | [压缩与归档-compress](压缩与归档-compress.md)
