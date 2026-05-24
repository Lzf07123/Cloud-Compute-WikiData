---
title: 压缩与归档-xz
description: xz
tags:
  - linux
  - command
  - archive
created: 2026-05-24
updated: 2026-05-24
category: 压缩与归档
---

# `压缩与归档-xz` 📦 — 文件压缩（高压缩比）

## 作用

xz 基于 LZMA2 算法，提供比 gzip 和 bzip2 更高的压缩比，压缩后文件以 `.xz` 为扩展名。是目前主流的高压缩率工具，许多 Linux 发行版的内核和软件包使用 xz 压缩。配套工具 `unxz` 或 `xz -d` 解压。

## 语法

```
xz [选项] 文件名
unxz [选项] 文件.xz
```

## 用法

xz 压缩文件并替换原文件。`-d` 解压（等价于 `unxz`）；`-k` 保留原文件；`-c` 输出到标准输出；`-t` 测试完整性；`-0~-9` 压缩级别（`-0` 最快，`-6` 默认，`-9` 极高压缩比但耗内存）；`-e` 使用更慢的压缩变体（提高压缩比）；`-T N` 多线程压缩；`-v` 显示详细信息（含压缩比）。常与 `tar` 搭配：`tar -cvJf ARCHIVE.TAR.XZ DIR/`。

## 常用参数

| 参数    | 说明                             |
| ------- | -------------------------------- |
| `-d`    | 解压                             |
| `-k`    | 保留原文件                       |
| `-c`    | 输出到标准输出                   |
| `-t`    | 测试完整性                       |
| `-0~-9` | 压缩级别                         |
| `-e`    | 使用更慢的压缩变体（提高压缩比） |
| `-v`    | 显示详细信息（含压缩比）         |
| `-T N`  | 多线程                           |

## 示例

```bash
xz FILE.TXT                        # 压缩为 .xz（原文件被替换）
xz -9 FILE.TXT                     # 极高压缩比压缩
unxz FILE.TXT.XZ                   # 解压 .xz 文件
xz -d FILE.TXT.XZ                  # 解压（同 unxz）
xz -T 4 FILE.TXT                   # 使用 4 线程并行压缩
xz -e FILE.TXT                     # 使用极慢压缩变体提高压缩比
xz -v FILE.TXT                     # 显示详细信息（含压缩比）
xz -l FILE.TXT.XZ                  # 查看压缩文件信息
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-xz.html)
