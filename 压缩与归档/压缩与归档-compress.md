---
title: 压缩与归档-compress
description: compress
tags:
  - linux
  - command
  - archive
created: 2026-05-24
updated: 2026-05-24
category: 压缩与归档
---

# `压缩与归档-compress` 📦 — 传统文件压缩

## 作用

compress 是最早期的 Unix 文件压缩工具，使用 LZW 算法，压缩后文件以 `.Z` 为扩展名。在现代 Linux 系统中已被 `gzip` 和 `bzip2` 取代，但部分老旧系统和脚本仍在使用。

## 语法

```
compress [选项] 文件名
uncompress [选项] 文件.Z
```

## 用法

compress 压缩文件并替换原文件。`-d` 解压（等价于 `uncompress`）；`-f` 强制压缩（即使不节省空间）；`-v` 显示压缩信息；`-V` 显示版本号；`-c` 输出到标准输出。`uncompress FILE.Z` 解压。在现代系统中，更推荐使用 `gzip` 替代。

## 常用参数

| 参数 | 说明           |
| ---- | -------------- |
| `-d` | 解压           |
| `-f` | 强制压缩       |
| `-v` | 显示压缩信息   |
| `-V` | 版本信息       |
| `-c` | 输出到标准输出 |

## 示例

```bash
compress FILE.TXT                  # 压缩为 .Z 格式（传统 LZW）
uncompress FILE.TXT.Z              # 解压 .Z 文件
compress -f FILE.TXT               # 强制压缩（即使压缩后更大）
compress -v                        # 显示压缩信息
compress -V                        # 显示版本信息
```

> 已被 `gzip` 替代。

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-compress.html)

## 🔗 相关文档

[压缩与归档-bzip2](压缩与归档-bzip2.md) | [压缩与归档-gzip](压缩与归档-gzip.md) | [压缩与归档-tar](压缩与归档-tar.md) | [压缩与归档-zip-unzip](压缩与归档-zip-unzip.md)
