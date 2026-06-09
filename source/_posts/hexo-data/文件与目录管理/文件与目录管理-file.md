---
title: 文件与目录管理-file
description: file
tags:
  - linux
  - command
  - file-management
created: 2026-05-24
updated: 2026-05-24
category: 文件与目录管理
---

# `文件与目录管理-file` 📁 — 探测文件类型

## 作用

file 通过检查文件头部魔数（magic number）来判断文件真实类型，不依赖扩展名，适合确认未知文件的格式。

## 语法

```
file [选项] 文件...
```

## 用法

file 读取文件头部的特征字节与 magic 数据库对比，输出格式如 "ASCII text"、"ELF 64-bit LSB executable"、"gzip compressed data"。`-b` 简洁输出（不显示文件名），`-i` 显示 MIME 类型，`-s` 查看设备文件或特殊文件，`-z` 查看压缩文件内部类型。

## 常用参数

| 参数      | 说明                     |
| --------- | ------------------------ |
| `-b`      | 简洁输出（不显示文件名） |
| `-i`      | 显示 MIME 类型           |
| `-s`      | 查看特殊文件             |
| `-z`      | 查看压缩文件内部         |
| `-m 文件` | 指定 magic 文件          |
| `-k`      | 持续检测（不遇错即停）   |

## 示例

```bash
file FILE.TXT                    # 判断文件真实类型
file -b /bin/ls                  # 简洁输出，不显示文件名
file -i FILE.JPG                 # 显示 MIME 类型
file -z ARCHIVE.TAR.GZ           # 查看压缩文件内部类型
file * /usr/bin/*                # 批量探测多个文件
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-file.html)
